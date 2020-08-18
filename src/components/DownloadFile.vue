<template>
  <div>
    <h2>Download CSV section</h2>
    <download-excel
      id="downloadButton"
      :data="json_data"
      :fields="json_fields"
      worksheet="My Worksheet"
      name="details.xls"
    >Download CSV File</download-excel>
  </div>
</template>

<script>
import DownloadExcel from "vue-json-excel";
import json from "./../assets/mock.json";
export default {
  name: "DownloadFile",
  components: {
    DownloadExcel,
  },
  data() {
    return {
      json_fields: {
        Id: "id",
        Variant: "product_details.variant",
        Title: "product_details.title",
        sku: "product_details.sku",
        Price: "product_details.price",
        Cogs: "product_details.cogs",
      },
    };
  },
  computed: {
    json_data() {
      return json;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#downloadButton {
  border: 0;
  cursor: pointer;
  background: rgb(119, 223, 70);
  color: #fff;
  padding: 0.7em 1em;
  font: inherit;
  margin: 1em;
}
</style>
