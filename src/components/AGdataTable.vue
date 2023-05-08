<template>
  <h1>Data Table</h1>
  <ag-grid-vue
    class="ag-theme-alpine"
    style="width: 100%; height: 600px"
    :columnDefs="columnDefs.value"
    :rowData="rowData.value"
    :defaultColDef="defaultColDef"
    rowSelection="multiple"
    animateRows="true"
    :popupParent="popupParent"
    @cell-clicked="cellWasClicked"
    @grid-ready="onGridReady"
    pagination = true
    rowGroupPanelShow="always"
    :suppressRowClickSelection="true"
  >
  </ag-grid-vue>

  <div class="BtnBox">
    <a href="#" style="--clr: #3c8ce7" @click.prevent="deselectRows"
      ><span>Cancel</span><i></i
    ></a>
    <a href="#" style="--clr: #ff96f9" @click="reset()"><span>reset</span><i></i></a>
  </div>
</template>

<script>
import { AgGridVue } from "ag-grid-vue3";
import { reactive, onMounted, ref, h } from "vue";
import "ag-grid-enterprise";
import "ag-grid-community/styles/ag-grid.css";
import "ag-grid-community/styles/ag-theme-alpine.css";

const SimpleComp = {
  setup(props) {
    const { params } = props;
    return () => h("span", params.value);
  },
};

export default {
  name: "AGdataTable",
  components: {
    AgGridVue,
    SimpleComp,
  },

  setup(props) {
    const gridApi = ref(null);
    const columnApi = ref(null);
    const onGridReady = (params) => {
      gridApi.value = params.api;
      columnApi.value = params.columnApi;
    };

    const rowData = reactive({ value: [] });
    const columnDefs = reactive({
      value: [
        {
          field: "athlete",
          cellRenderer: "SimpleComp",
          filter: "agTextColumnFilter",
          floatingFilter: true,
          headerCheckboxSelection: true,
          checkboxSelection: true,
          showDisabledCheckboxes: true,
        },
        { field: "age", filter: "agNumberColumnFilter",floatingFilter: true, },
        { field: "country" },
        { field: "year" },
        { field: "date" },
        { field: "sport" },
        { field: "gold" },
        { field: "silver" },
        { field: "bronze" },
        { field: "total" },
      ],
    });

    const defaultColDef = {
      sortable: true,
      filter: true,
      enableRowGroup: true,
      filterParams: { debounceMs: 0 },
    };

    const deselectRows = () => {
      gridApi.value.deselectAll();
    }

    const reset = () => {
      gridApi.value.setFilterModel(null);
    }


    onMounted(() => {
      fetch("https://www.ag-grid.com/example-assets/olympic-winners.json")
        .then((res) => res.json())
        .then((remoteRowData) => (rowData.value = remoteRowData));
    });

    return {
      rowData,
      columnDefs,
      defaultColDef,
      onGridReady,
      deselectRows,
      reset,
      popupParent: document.body,
      cellWasClicked: (event) => {
        console.log("cell was clicked", event.data, event.node.isSelected());
      },
    };
  },
};
</script>