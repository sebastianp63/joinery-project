<template>
  <div class="uk-card uk-card-default uk-card-body">
    <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-small-bottom">
      <p class="uk-h4">Your templates</p>
    </div>
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
      <tbody v-for="(item, i) in templates" :key="item.id">
        <tr class="uk-visible-toggle uk-transition-toggle" tabindex="-1">
          <td>{{item.id}}</td>
          <td>{{item.width}}</td>
          <td>{{item.height}}</td>
          <td>[ {{item.unit}} ]</td>
          <td>[ {{formatGlue(item.veneer) ? formatGlue(item.veneer) : 'none' }} ]</td>
          <td>
            <div class="uk-width-auto">
              <ul class="uk-hidden-hover uk-iconnav uk-transition-fade">
                <li>
                  <a href="#" uk-icon="icon: pencil"></a>
                </li>

                <li>
                  <a href="#modal-example" uk-toggle uk-icon="icon: trash"></a>
                </li>
              </ul>
            </div>
          </td>
        </tr>
      </tbody>
    </table>

    <div id="modal-example" uk-modal>
      <div class="uk-modal-dialog uk-modal-body">
        <h2 class="uk-modal-title">Do you want delete this template?</h2>
        <p class="uk-text-right">
          <button class="uk-button uk-button-primary uk-modal-close" type="button">Cancel</button>
          <button class="uk-button uk-button-danger" type="button">Delete</button>
        </p>
      </div>
    </div>
    <button class="uk-button uk-button-danger" type="button" @click="sendData">Send</button>
  </div>
</template>

<script>
import { eventBus } from "../main";

export default {
  name: "templateContainer",
  props: ["templates"],

  methods: {
    formatGlue(data) {
      let str = "";
      Object.keys(data).forEach(function(key) {
        if (data[key] === true) {
          str =
            str +
            key
              .toString()
              .charAt(0)
              .toUpperCase() +
            "-";
        }
      });
      if (str.charAt(str.length - 1) == "-") {
        str = str.slice(0, str.length - 1);
      }
      return str;
    },

    sendData() {
      eventBus.makeOrder();
    }
  }
};
</script>


<style lang="scss" scoped>
.uk-card {
  min-height: 100%;
}
</style>