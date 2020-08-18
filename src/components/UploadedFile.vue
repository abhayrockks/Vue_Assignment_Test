<template>
  <div>
    <div>
      <h2>Upload CSV section</h2>
      <input type="file" @change="previewFiles" />
    </div>
    <li v-for="(item, index) in details" v-bind:key="index">{{item}}</li>
  </div>
</template>

<script>
import XLSX from "xlsx";
export default {
  name: "UploadedFile",
  data() {
    return {
      details: [],
      cog: [],
      noncog: [],
    };
  },
  computed: {},
  methods: {
    previewFiles(e) {
      var files = e.target.files,
        f = files[0];
      var reader = new FileReader();
      reader.onload = function (e) {
        var data = new Uint8Array(e.target.result);
        var workbook = XLSX.read(data, { type: "array" });
        let sheetName = workbook.SheetNames[0];
        let worksheet = workbook.Sheets[sheetName];
        var dataarray = XLSX.utils.sheet_to_json(worksheet);
        this.details.push(dataarray);
      };
      reader.readAsArrayBuffer(f);
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
li {
  list-style-type: none;
}
</style>
