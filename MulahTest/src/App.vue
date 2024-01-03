<template>
  <div >
    <label>Input table: </label>
    <input type="file" @change="loadCsvFile">
    <br>

    <table v-if="tableData.length" border="1px" style="width: 100%;">
      <tr v-for="(row, index) in tableData" :key="index">
        <td v-for="(cell, cellIndex) in row" :key="cellIndex">{{ cell }}</td>
      </tr>
    </table>
    <br>

    <table v-if="sumTableData.length" border="1px" style="width: 100%;">
      <tr>
        <th>Category</th>
        <th>Value</th>
      </tr>
      <tr v-for="(row, index) in sumTableData" :key="'sum' + index">
        <td v-for="(cell, cellIndex) in row" :key="'sumCell' + cellIndex">{{ cell }}</td>
      </tr>
    </table>

  </div>
</template>


<script>
export default {
  data() {
    return {
      tableData: [],
    };
  },

  computed: {
    sumTableData() {
      if (this.tableData.length < 20) return [];

      let alpha = Number(this.tableData[5][1]) + Number(this.tableData[20][1]); // A5 + A20
      let beta = Number(this.tableData[15][1]) / Number(this.tableData[7][1]);  // A15 / A7
      let charlie = Number(this.tableData[13][1]) * Number(this.tableData[12][1]); // A13 * A12

      return [['Alpha', alpha], ['Beta', beta], ['Charlie', charlie]];
    }
  },

  methods: {
    loadCsvFile(event) {
      const file = event.target.files[0];
      const reader = new FileReader();
      reader.onload = (e) => {
        const text = e.target.result;
        this.tableData = text.split(/\r\n|\n/).map(row => row.split(','));
      };
      reader.readAsText(file);
    },
  },
};
</script>

<style>
body {
  background-color: white;
  color: black;
}
</style>
