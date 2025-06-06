<script setup lang="ts">
import {defineComponent, onMounted, ref} from "vue";
import {createUniver, LocaleType} from "@univerjs/presets";
import { UniverSheetsCorePreset } from "@univerjs/presets/preset-sheets-core";
import sheetsCoreZhCN from "@univerjs/presets/preset-sheets-core/locales/zh-CN";
import {merge} from "@univerjs/core";


import "@univerjs/presets/lib/styles/preset-sheets-core.css";

// Vue3-Component
const Vue3Component = defineComponent({
  setup(props) {
    console.log('Vue3Component', props);
    return () => `<div style={{ width: 100, height: 100, background: '#fff' }}>Popup Content</div>`
  }
});
// Vue3-Component

onMounted(()=>{
  const { univer, univerAPI } = createUniver({
    locale: LocaleType.ZH_CN,
    locales: {
      [LocaleType.ZH_CN]: merge(
          {},
          sheetsCoreZhCN,
      )
    },
    presets: [
      UniverSheetsCorePreset({
        container: "container"
      }),
      // UniverSheetsTablePreset(),
    ]
  });
  const univerData=ref({
    id: "gyI0JO",
    sheetOrder: ["test2"],
    name: "",
    appVersion: "0.5.0",
    styles: {
    },
    sheets: {
      test2: {
        id: "test2",
        name: "测试1",
        tabColor: "",
        hidden: 0,
        rowCount: 500,
        columnCount: 50,
        freeze: {},
        mergeData: [],
        rowData: {},
        columnData: {},
        cellData: {},
        showGridlines: 1,
        rightToLeft: 0
      }
    }
  })
  const workbook = univerAPI.createWorkbook(univerData.value);

  const fWorksheet = workbook.getActiveSheet();
  const fRange = fWorksheet.getRange('A1:J10');

  //register univer component
  univerAPI.registerComponent(
      'myPopup',
      Vue3Component,
      {
        framework: 'vue3',
      }
  );
  //register univer component

  // 将弹出窗口附加到范围的第一个单元格
  // 如果 disposeable 为 null，则表示 popup 添加失败
  const disposeable = fRange.attachPopup({
    // componentKey 必须是一个组件或已注册组件的键
    componentKey: 'myPopup',
  });
  console.log('disposeable', disposeable);



})
</script>

<template>
  <div id="container" style="width:100vw;height:90vh;min-height:200px;" />
</template>

<style scoped>

</style>
