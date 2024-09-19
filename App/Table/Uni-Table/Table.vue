<template>
  <uni-table class="table" border stripe emptyText="暂无更多数据">
    <uni-tr>
      <uni-th width="1px" v-for="item of columnDes" :key="item" align="left">{{
        item
      }}</uni-th>
    </uni-tr>
    <!-- -------- td ------------ -->
    <uni-tr v-for="item of tableList" :key="item">
      <uni-td v-for="field of columnFields" :key="item" align="left">{{
        valueDesMapOfFields?.[field] ? valueDesMapOfFields[field][item[field]] : item[field]
      }}</uni-td>
    </uni-tr>
  </uni-table>
</template>

<script setup>
const props = defineProps({
  /// 表头的文字说明
  columnDes: {
    type: Array,
    default: () => [],
  },
  /// 表头的文字说明对应的字段
  columnFields: {
    type: Array,
    default: () => [],
  },
  /// 表格数据
  tableList: {
    type: Array,
    default: () => [],
  },
  /// 字段实际的值对应的文字
  valueDesMapOfFields: {
    type: Object,
    default: () => {},
  },
});
</script>

<style lang="scss" scoped>
.table {
  margin: 2vh 0;
  padding: 0 4%;
  box-sizing: border-box;
  $border-style: 0.8px solid #bfddff;
  :deep() {
    th {
      background-color: #d9eaff;
      color: #1d2129;
      font-weight: 400;
    }
    table {
      border: $border-style;
      border-bottom: unset;
      tr {
        th,
        td {
          font-size: 0.8rem;
          border-bottom: $border-style;
          &:not(:last-child) {
            border-right: $border-style;
          }
        }
      }
    }
  }
}
</style>
