<template>
  <view>
    <uni-section
      v-if="!noTitle"
      class="section"
      :title="title"
      type="line"
    ></uni-section>
    <view
      class="table"
      v-for="(value, key) in tableObj"
      :key="key"
      :style="{ fontSize }"
    >
      <view
        class="key"
        :style="{
          flex: ratio[0],
          backgroundColor: keyBackground,
        }"
        >{{ key }}</view
      >
      <view
        class="value"
        :style="{ flex: ratio[1], backgroundColor: valueBackground }"
        >{{ value }}</view
      >
    </view>
  </view>
</template>

<script setup>
const props = defineProps({
  title: {
    type: String,
  },
  //{key: value}的对象
  tableObj: {
    type: Object,
    default: {},
  },
  //key和value的宽度比例
  ratio: {
    type: Array,
    default: () => [13, 17],
  },
  //不需要标题时加上
  noTitle: {
    type: Boolean,
    default: false,
  },
  //key的背景色
  keyBackground: {
    type: String,
    default: "#f2f8ff",
  },
  //value的背景色
  valueBackground: {
    type: String,
    default: "white",
  },
  fontSize: {
    type: String,
    default: "0.8rem",
  },
});
</script>

<style lang="scss" scoped>
:deep(.uni-section__content-title.distraction) {
  font-weight: bold;
}
.table {
  display: flex;
  --border-width: 0.5px;
  width: 100%;
  padding: 0 4%;
  box-sizing: border-box;
  &:last-of-type {
    .key,
    .value {
      border-bottom: var(--border-width) solid #bfddff;
    }
  }
  .key,
  .value {
    padding-left: 3%;
    box-sizing: border-box;
    color: #1d2129;
    border-width: var(--border-width) 0 0 var(--border-width);
    border-style: solid;
    border-color: #bfddff;
    line-height: 2;
    display: flex;
    align-items: center;
  }
  .key {
    flex: 13;
    background-color: #f2f8ff;
  }
  .value {
    flex: 17;
    background-color: white;
    border-right-width: var(--border-width);
  }
}
</style>
