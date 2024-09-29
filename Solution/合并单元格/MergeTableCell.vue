<template>
  <div>
    <el-table
      :data="tableData"
      border
      :span-method="objectSpanMethod"
      :show-header="false"
    >
      <el-table-column prop="prop1" />
      <el-table-column prop="prop2" />
      <el-table-column prop="prop3" />
    </el-table>
  </div>
</template>

<script setup>
import { reactive } from "vue";
const tableData = [
  { prop1: "竖三", prop2: "竖二", prop3: "单元格1" },
  { prop1: "竖三", prop2: "竖二", prop3: "单元格2" },
  { prop1: "竖三", prop2: "横二", prop3: "横二" },
];
const spanMap = reactive({});
const props = ["prop1", "prop2", "prop3"]; /// 列字段
function objectSpanMethod({ rowIndex, columnIndex }) {
  return spanMap[rowIndex * props.length + columnIndex];
}
/**
 * @description 合并单元格
 * @description 思想是：先按行遍历，收集横向合并的单元格，再按列遍历，收集纵向合并的单元格
 * @description 每个单元在spanMap中的索引：rowIndex * 列数 + columnIndex
 * @description 每一次循环，将相邻相同值的单元格在spanMap中的索引放进一个数组
 * @description 若第一行的内容为[1,2,2]-->[[0],[1,2]]
 * @description 若第二行的内容为['k','k','x']-->[[3,4],[5]]
 * @description 最终第二行的spanMap: 第一个k, rowspan=2, colspan=1;
 * 第二个k被合并, rowspan=0, colspan=0;
 * x, rowspan=1, colspan=1
 */
function setSpanMap() {
  const rowLength = tableData.length; /// 行数
  const colLength = props.length; /// 列数
  let groupedIndex = [];
  //. 先按行遍历，收集横向合并的单元格
  for (let row = 0; row < rowLength; row++) {
    const rowData = tableData[row];
    const rowIndexAndValue = []; /// [{index,value}]
    for (let col = 0; col < colLength; col++) {
      let prop = props[col];
      let index = row * colLength + col;
      rowIndexAndValue.push({ index, value: rowData[prop] });
    }
    groupedIndex = groupedIndex.concat(groupAdjacent(rowIndexAndValue));
  }
  for (let arr of groupedIndex) {
    if (arr.length == 1) {
      spanMap[arr[0]] = { rowspan: 1, colspan: 1 };
    } else {
      spanMap[arr[0]] = { rowspan: 1, colspan: arr.length };
      for (let i = 1; i < arr.length; i++) {
        spanMap[arr[i]] = { rowspan: 0, colspan: 0 };
      }
    }
  }
  //. 再按列遍历，收集纵向合并的单元格
  for (let col = 0; col < colLength; col++) {
    const colData = tableData.map((row) => row[props[col]]);
    const colIndexAndValue = colData.map((value, index) => ({
      index: index * colLength + col,
      value,
    }));
    groupedIndex = groupAdjacent(colIndexAndValue);
    for (let arr of groupedIndex) {
      if (arr.length == 1) {
        spanMap[arr[0]].rowspan !== 0 && (spanMap[arr[0]].rowspan = 1);
      } else {
        spanMap[arr[0]].rowspan = arr.length;
        for (let i = 1; i < arr.length; i++) {
          spanMap[arr[i]] = { rowspan: 0, colspan: 0 };
        }
      }
    }
  }
}
/**
 * @description 输入：[{index:3,value:'k'},{index:4,value:'k'},{index:5,value:'x'}]
 * @description 输出：[[3,4],[5]]
 */
function groupAdjacent(arr) {
  const result = [];
  let temp = [arr[0].index];
  for (let i = 1; i < arr.length; i++) {
    if (arr[i].value === arr[i - 1].value) {
      temp.push(arr[i].index);
    } else {
      result.push(temp);
      temp = [arr[i].index];
    }
  }
  result.push(temp);
  return result;
}
setSpanMap();
</script>

<style lang="scss" scoped></style>