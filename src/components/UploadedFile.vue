<template>
  <div>
    <div>
      <h2>Upload CSV section</h2>
      <input type="file" @change="previewFiles" />
    </div>
    <div>
      <h2>Empty Cogs</h2>
      <v-card v-for="(empty_item, empty_index) in emptycog" v-bind:key="empty_index">
        <div class="d-flex justify-space-between">
          <div>
            <v-card-title class="headline">{{empty_item.Title}}</v-card-title>
            <v-card-subtitle>
              {{empty_item.Variant}}
              SKU: {{empty_item.sku}} for: {{empty_item.Price}}
            </v-card-subtitle>
          </div>
          <p>
            <v-input class="ma-3" size="125" tile>
              COGS
              <input
                type="text"
                :value="empty_item.Cogs"
                v-on:keypress.enter="submit($event, empty_item.Id)"
              />
            </v-input>
          </p>
        </div>
      </v-card>
    </div>
    <div>
      <h2>Non Empty Cogs</h2>
      <v-card v-for="(item, index) in cog" v-bind:key="index">
        <div class="d-flex justify-space-between">
          <div>
            <v-card-title class="headline">{{item.Title}}</v-card-title>

            <v-card-subtitle>
              {{item.Variant}}
              SKU: {{item.sku}} for: {{item.Price}}
            </v-card-subtitle>
          </div>

          <v-input class="ma-3" size="125" tile>
            COGS
            {{item.Cogs}}
          </v-input>
        </div>
      </v-card>
    </div>
  </div>
</template>
<script>
import XLSX from "xlsx";
export default {
  name: "UploadedFile",
  data() {
    return {
      details: [],
      cogvalue: "",
    };
  },
  computed: {
    emptycog() {
      return this.details.filter(function (array) {
        return !array.Cogs;
      });
    },
    cog() {
      return this.details.filter(function (array) {
        return array.Cogs;
      });
    },
  },
  methods: {
    submit(e, id) {
      var data = this.details.findIndex(function (array) {
        return array.Id === id;
      });
      this.$set(this.details[data], "Cogs", e.target.value);
    },
    previewFiles(e) {
      var files = e.target.files,
        f = files[0];
      var reader = new FileReader();
      reader.onload = (e) => {
        var data = new Uint8Array(e.target.result);
        var workbook = XLSX.read(data, { type: "array" });
        let sheetName = workbook.SheetNames[0];
        let worksheet = workbook.Sheets[sheetName];
        var dataarr = XLSX.utils.sheet_to_json(worksheet);
        this.details = dataarr.slice();
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

input {
  border: 1px solid rgb(5, 5, 5);
  border-radius: 4px;
}
</style>
