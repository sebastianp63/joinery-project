<template>
  <div
    class="border-template .uk-background-default uk-align-center uk-padding-small uk-margin-medium-left uk-margin-medium-right"
    style=" min-height: 85vh"
  >
    <div class="uk-grid-small uk-child-width-expand@s uk-height-1-1" uk-grid>
      <div class="uk-width-1-3@m">
        <div>
          <template-form
            @showPreview="shown = $event "
            @onUpdated="onTemplateUpdate"
            @addTemplate="addNewTemplate"
          />
        </div>
        <div class="uk-margin-top">
          <transition name="fade">
            <template-preview v-show="shown" v-bind:templateData="templateData"></template-preview>
          </transition>
        </div>
      </div>

      <div style="height:1033px">
        <template-container :templates="templates" @removeRow="removeTemplate($event)" />
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

axios.defaults.headers.post["Content-Type"] = "application/json";

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
      templates: [],
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
      }
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

    addNewTemplate: function() {
      if (this.templateData.height == 0 || this.templateData.width == 0) {
        console.log("Cannot add template with no dimension");
      } else {
        let exist = this.templates.filter(el => {
          return el.id == this.templateData.id;
        });
        if (!exist.length > 0) {
          this.templates.push(_.cloneDeep(this.templateData));
        } else {
          console.log("this themplate has been already exist");
        }
        this.templateData.id += 1;
      }
    },

    removeTemplate: function(index) {
      const indexItemToRemove = index - 1;
      this.templates.splice(indexItemToRemove, 1);
      this.templateData.id = index;
      for (let i = indexItemToRemove; i < this.templates.length; i++) {
        this.templates[i].id = this.templateData.id;
        if (this.templateData.id < this.templates.length)
          this.templateData.id++;
      }
    }
  },

  mounted() {
    eventBus.$on("makeOrder", () => {
      let obj = {};
      if (
        this.firstName == "" ||
        this.firstName.length < 3 ||
        this.lastName == "" ||
        this.lastName.length
      ) {
        console.log("invalid user data");
      }
      obj.firstName = this.firstName;
      obj.lastName = this.lastName;
      obj.templates = this.templates;
      let order = JSON.stringify(obj);

      console.log(order);

      axios
        .post("/api/save/order", order)
        .then(function(response) {
          console.log(response);
        })
        .catch(function(error) {});
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

.max-h {
  height: 100%;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
</style>
