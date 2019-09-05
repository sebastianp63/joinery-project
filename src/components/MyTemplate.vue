<template>
  <div
    class="uk-margin-small-left uk-margin-small-right border-template .uk-background-default uk-align-center uk-padding-small"
  >
    <div class="uk-child-width-expand@s uk-text-center" uk-grid>
      <div class="uk-width-1-3@m uk-text-center">
        <div>
          <template-form
            @showPreview="shown = $event "
            @onUpdated="onTemplateUpdate"
            @addTemplate="addNewTemplate"
          />
        </div>
        <div v-show="shown">
          <template-preview v-bind:templateData="templateData"></template-preview>
        </div>
      </div>

      <div class="uk-width-expand@m">
        <template-container :templates="templates" />
      </div>
    </div>
  </div>
</template>

<script>
import TemplateForm from "@/components/TemplateForm.vue";
import TemplateContainer from "@/components/TemplateContainer.vue";
import TemplatePreview from "@/components/TemplatePreview.vue";

import { eventBus } from "../main";

const _ = require("lodash");
const axios = require("axios");

export default {
  props: {
    id: Number,
    firstName: {
      type: String,
      default: ""
    },
    lastName: {
      type: String,
      default: ""
    }
  },
  name: "MyTemplate",
  data: function() {
    return {
      shown: false,
      templateData: {
        id: 1,
        width: 100,
        height: 100,
        veneer: {
          top: false,
          bottom: false,
          left: false,
          right: false
        },
        unit: "mm"
      },
      templates: []
    };
  },
  components: {
    templateForm: TemplateForm,
    templateContainer: TemplateContainer,
    templatePreview: TemplatePreview
  },
  methods: {
    onTemplateUpdate(data) {
      this.templateData.width = data.width;
      this.templateData.height = data.height;
      this.templateData.unit = data.units;
      this.templateData.veneer.top = data.glue.includes("t");
      this.templateData.veneer.bottom = data.glue.includes("b");
      this.templateData.veneer.left = data.glue.includes("l");
      this.templateData.veneer.right = data.glue.includes("r");
      // Webpack proxy API

      // axios.post('url', order).then((data) => {
      //   alert('Saved')
      // })
    },

    emitData: function() {
      this.$emit("emitData", this.templateData);
    },

    addNewTemplate() {
      this.templates.push(_.cloneDeep(this.templateData));
      // this.$set(this.templates, _.cloneDeep(this.templateData));
      this.templateData.id += 1;
    }
  },
  mounted() {
    eventBus.$on("makeOrder", () => {
      let obj = {};
      (obj.firstName = this.firstName),
        (obj.lastName = this.lastName),
        (obj.templates = this.templates);
      let order = JSON.stringify(obj);

      axios
        .post("/api/save/order", order)
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {
          console.log(error);
        });
    });
  }
};
</script>

<style lang="scss" scoped>
.border-template {
  border: 2px solid black;
}
* {
  box-sizing: border-box;
}
</style>
