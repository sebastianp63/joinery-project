<template>
  <div
    class="border-template .uk-background-default uk-align-center uk-padding-small uk-margin-medium-left uk-margin-medium-right"
    style=" min-height: 85vh"
  >
    <div v-if="success" class="uk-alert-success" uk-alert>
      <a class="uk-alert-close" uk-close></a>
      <p>Data send correctly</p>
    </div>

    <div class="uk-grid-small uk-child-width-expand@s uk-height-1-1" uk-grid>
      <div class="uk-width-1-3@m">
        <div>
          <template-form
            :title="'Create your template'"
            @showPreview="isShowedPreview = $event "
            @onUpdated="onTemplateUpdate"
          >
            <my-button
              :text="isShowedPreview ? 'Hide Preview': 'Show Preview'"
              @clickEvent="showPreview"
            ></my-button>
            <my-button :text="'Add template'" :secondary="true" @clickEvent="addNewTemplate"></my-button>
          </template-form>
        </div>
        <div class="uk-margin-top">
          <transition name="fade">
            <template-preview v-show="isShowedPreview" v-bind:templateData="templateData"></template-preview>
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
import MyButton from "./formButtons/MyButton";

import { eventBus } from "../main";
const fs = require("browserify-fs");
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
  data() {
    return {
      isShowedPreview: false,
      success: false,
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
    myButton: MyButton,
    templateForm: TemplateForm,
    templateContainer: TemplateContainer,
    templatePreview: TemplatePreview
  },

  methods: {
    onTemplateUpdate(data) {
      this.templateData = { id: this.templateData.id, ...data };
    },

    showPreview(event) {
      this.isShowedPreview = !this.isShowedPreview;
      this.$emit("showPreview", this.isShowedPreview);
    },

    emitData() {
      this.$emit("emitData", this.templateData);
    },

    addNewTemplate() {
      if (this.templates.length < 100) {
        if (this.templateData.height == 0 || this.templateData.width == 0) {
          console.log("Cannot add template with no dimension");
        } else {
          const exist = this.templates.filter(
            el => el.id == this.templateData.id
          );
          if (!exist.length > 0) {
            this.templates.push(_.cloneDeep(this.templateData));
          } else {
            console.log("this themplate has been already exist");
          }
          this.templateData.id += 1;
        }
      }
      const storage = JSON.stringify(this.templates);
      window.localStorage.setItem("storage", storage);
      console.log(JSON.parse(window.localStorage.getItem("storage")));
      // fs.writeFile("storage.txt", "storage", "utf8", err => {
      //   if (err) throw err;
      // });
    },

    removeTemplate(index) {
      const indexItemToRemove = index - 1;
      this.templates.splice(indexItemToRemove, 1);
      this.templateData.id = index;
      for (let i = indexItemToRemove; i < this.templates.length; i++) {
        this.templates[i].id = this.templateData.id;
        if (this.templateData.id <= this.templates.length) {
          this.templateData.id++;
        }
      }
    }
  },

  mounted() {
    eventBus.$on("makeOrder", () => {
      this.success = false;
      const obj = {};
      if (
        this.firstName === "" ||
        this.firstName.length < 3 ||
        this.lastName === "" ||
        this.lastName.length < 3 ||
        _.isEmpty(this.templates)
      ) {
        console.log("invalid user data");
      } else {
        obj.firstName = this.firstName;
        obj.lastName = this.lastName;
        obj.templates = this.templates;
        const order = JSON.stringify(obj);

        axios
          .post("/api/save/order", order)
          .then(response => {
            console.log(response);
            this.success = true;
          })
          .catch(error => {});
      }
    });
  },
  // watch: {
  //   $route(to, from) {
  //     console.log(JSON.parse(window.localStorage.getItem("storage")));
  //     this.templates = JSON.parse(window.localStorage.getItem("storage"));
  //     this.templateData.id = this.templates[this.templates.length - 1].id + 1;
  //   }
  // },
  beforeRouteUpdate(to, from, next) {
    console.log(JSON.parse(window.localStorage.getItem("storage")));
    this.templates = JSON.parse(window.localStorage.getItem("storage"));
    this.templateData.id = this.templates[this.templates.length - 1].id + 1;
    // react to route changes...
    // don't forget to call next()
    next();
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
.fade-enter,
.fade-leave-to {
  opacity: 0;
}
</style>
