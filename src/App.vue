<template>
  <div
    ref="element"
    class="main"
    :class="shape"
  >
    <span>{{ text }}</span>
  </div>
  <a-divider></a-divider>

  <a-form-item field="name" label="shape">
    <a-radio-group v-model="shape">
      <a-radio value="square">正方形</a-radio>
      <a-radio value="circle">圆形</a-radio>
    </a-radio-group>
  </a-form-item>
  <a-form-item field="name" label="radius" v-if="shape === 'square'">
    <a-slider v-model="borderRadius" show-input />
  </a-form-item>
  <a-form-item field="name" label="text">
    <a-input v-model="text"></a-input>
  </a-form-item>
  <a-form-item field="name" label="text">
    <a-input type="color" v-model="textColor"></a-input>
  </a-form-item>
  <a-form-item field="name" label="textSize">
    <a-input-number v-model="textSize"></a-input-number>
  </a-form-item>
  <a-form-item field="name" label="background">
    <a-input type="color" v-model="backgroundColor"></a-input>
  </a-form-item>

  <a-button @click="onClick" type="primary" long> 下载 </a-button>
</template>
<script setup lang="ts">
import { computed, ref } from "vue";
import html2canvas from "html2canvas";

const shape = ref("square");
const borderRadius = ref(10)
const text = ref("test");
const textColor = ref("#aaa")
const textSize = ref(16)
const backgroundColor = ref("#ddd");
const element = ref<HTMLElement>()

const borderRadiusValue = computed(() => {
  return shape.value === "square" ? borderRadius.value : 0;
});

const onClick = () => {
  html2canvas(element.value as HTMLElement, {
    backgroundColor: "rgba(0,0,0,0)",
  }).then((canvas) => {
    const img = document.createElement("a");
    img.href = canvas
      .toDataURL("image/png")
      .replace("image/png", "image/octet-stream");
    img.download = "case.png";
    img.click();
  });
};
</script>

<style scoped>
.main {
  width: 200px;
  height: 200px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: v-bind(backgroundColor);
  color: v-bind(textColor);
  border-radius: v-bind(borderRadiusValue + 'px');
  font-size: v-bind(textSize + 'px');
  /* border: 1px solid #222; */
}

.circle {
  border-radius: 50%;
}
</style>
