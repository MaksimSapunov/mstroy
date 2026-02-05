<script setup lang="ts">
import { AgGridVue } from 'ag-grid-vue3'
import { ref } from 'vue'
import { type CellClassParams, type ColDef, type ValueFormatterParams } from 'ag-grid-community'
import {
  type TreeItem,
  TreeItemCategory,
  type TreeItemForAgGrid,
  TreeStore,
} from '@/classes/TreeStore.ts'

const rowData = ref<TreeItemForAgGrid[]>([])

const items: TreeItem[] = [
  { id: 1, parent: null, label: 'Айтем 1' },
  { id: '91064cee', parent: 1, label: 'Айтем 2' },
  { id: 3, parent: 1, label: 'Айтем 3' },
  { id: 4, parent: '91064cee', label: 'Айтем 4' },
  { id: 5, parent: '91064cee', label: 'Айтем 5' },
  { id: 6, parent: '91064cee', label: 'Айтем 6' },
  { id: 7, parent: 4, label: 'Айтем 7' },
  { id: 8, parent: 4, label: 'Айтем 8' },
]
const itemsInit = new TreeStore(items)
rowData.value = itemsInit.getAgGridFormat()
const colDefs = ref<ColDef<TreeItemForAgGrid>[]>([
  {
    field: 'id',
    headerName: '№ п/п',
    cellDataType: 'string',
    headerClass: 'font-bold',
    cellClass: 'font-bold',
    valueFormatter: (item: ValueFormatterParams<TreeItemForAgGrid>) =>
      item.node?.rowIndex ? String(item.node.rowIndex + 1) : '',
  },
  {
    field: 'category',
    headerName: 'Категория',
    showRowGroup: true,
    cellRenderer: 'agGroupCellRenderer',
    cellRendererParams: {
      suppressCount: true,
    },
    valueFormatter: (item: ValueFormatterParams<TreeItemForAgGrid>) => item.value.category,
    headerClass: 'font-bold',
    cellClassRules: {
      'font-bold': (item: CellClassParams) => item.value === TreeItemCategory.GROUP,
    },
  },
  {
    field: 'label',
    headerName: 'Наименование',
    headerClass: 'font-bold',
    cellClassRules: {
      'font-bold': (item: CellClassParams) => item.value === TreeItemCategory.GROUP,
    },
  },
])
</script>

<template>
  <main class="main">
    <div class="caption">Режим: просмотр</div>
    <ag-grid-vue
      :rowData="rowData"
      :columnDefs="colDefs"
      style="height: 500px"
      :treeData="true"
      :groupDefaultExpanded="-1"
      :defaultColDef="{
        flex: 1,
      }"
      groupDisplayType="custom"
      :getDataPath="(data) => data.path"
    />
  </main>
</template>
