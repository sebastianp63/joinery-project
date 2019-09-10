<template>
  <div class="uk-card uk-card-default uk-card-body uk-height-1-1">
    <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-small-bottom">
      <p class="uk-h4">Created templates {{currentPage}}</p>
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
      <tbody>
        <tr
          v-for="(item, i) in visibleTemplates"
          :key="item.id"
          class="uk-visible-toggle uk-transition-toggle"
          tabindex="-1"
        >
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
                  <a
                    @click="indexRow = i + 1"
                    href="#modal-example"
                    uk-toggle
                    uk-icon="icon: trash"
                  ></a>
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
          <button
            class="uk-button uk-button-primary uk-modal-close uk-margin-right"
            type="button"
          >Cancel</button>
          <button
            @click="$emit('removeRow',indexRow)"
            class="uk-button uk-button-danger uk-modal-close"
            type="button"
          >Delete - {{indexRow}}</button>
        </p>
      </div>
    </div>

    <button class="uk-button uk-button-danger" type="button" @click="sendData">Send</button>
    <div class="uk-position-bottom">
      <pagination-template
        v-show="pageCount>1"
        :current-page="currentPage"
        :page-count="pageCount"
        @nextPage="pageChangeHandle('next')"
        @previousPage="pageChangeHandle('previous')"
        @loadPage="pageChangeHandle"
      ></pagination-template>
    </div>
    {{currentPage}} - {{pageCount}}
  </div>
</template>

<script>
import { eventBus } from "../main";
import Pagination from "./Pagination";

export default {
  name: "templateContainer",
  props: ["templates"],
  components: {
    "pagination-template": Pagination
  },
  data() {
    return {
      currentPage: 0,
      pageSize: 10,
      pageCount: 1,
      visibleTemplates: [],
      indexRow: {
        type: Number,
        default: null
      }
    };
  },

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

    pageChangeHandle(value) {
      switch (value) {
        case "next":
          this.currentPage += 1;
          break;
        case "previous":
          this.currentPage -= 1;
          break;
        default:
          this.currentPage = value;
      }

      const from = this.currentPage * this.pageSize;
      const to = from + this.pageSize;
      this.visibleTemplates = this.templates.slice(from, to);
    },

    sendData() {
      eventBus.makeOrder();
    },
    getTemp() {}
  },
  watch: {
    templates: function() {
      const from = this.currentPage * this.pageSize;
      const to = from + this.pageSize;
      this.visibleTemplates = this.templates.slice(from, to);
      this.pageCount = Math.ceil(this.templates.length / this.pageSize);
    }
  }
};
</script>


<style lang="scss" scoped>
.uk-card {
  min-height: 100%;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>