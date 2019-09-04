<template>
  <div class="uk-card uk-card-default uk-card-body">
    <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-small-bottom">
      <p class="uk-h4">Your templates</p>
    </div>
    <!-- <table class="uk-table uk-table-striped"> -->
    <table class="uk-table uk-table-hover uk-table-divider">
      <thead>
        <tr>
          <th class="uk-text-center">Index</th>
          <th class="uk-text-center">Width</th>
          <th class="uk-text-center">Height</th>
          <th class="uk-text-center">Unit</th>
          <th class="uk-text-center">Glue</th>
        </tr>
      </thead>
      <!-- <tfoot>
        <tr>
          <td>Table Footer</td>
          <td>Table Footer</td>
          <td>Table Footer</td>
        </tr>
      </tfoot>-->
      <tbody v-for="item in templates" :key="item.id">
        <tr>
          <td>{{item.id}}</td>
          <td>{{item.template.width}}</td>
          <td>{{item.template.height}}</td>
          <td>[ {{item.template.unit}} ]</td>
          <td>{{formatGlue(item.template.veneer)}}</td>
        </tr>
      </tbody>
      <!-- {{templates}} -->
    </table>
  </div>
</template>

<script>
export default {
  name: "templateContainer",
  props: ["templates"],

  methods: {
    formatGlue(data) {
      let str = "";
      Object.keys(data).forEach(function(key) {
        console.log(key, data[key]);
        if (data[key] === true) {
          str =
            str +
            key
              .toString()
              .charAt(0)
              .toUpperCase();
        }
      });
      if (str.charAt(str.length - 1) === "-") {
      }

      console.log(str);
      return str;
    }
  }
};
</script>


<style lang="scss" scoped>
.uk-card {
  min-height: 100%;
}
</style>