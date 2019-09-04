<template>
  <div
    class="uk-margin-small-left uk-margin-small-right border-template .uk-background-default uk-align-center uk-padding-small"
  >
    <div class="uk-child-width-expand@s uk-text-center" uk-grid>
      <div class="uk-width-1-3@m">
        <template-form @onUpdated="onTemplateUpdate" @addTemplate="addNewTemplate()" />
      </div>
      <div class="uk-width-expand@m">
        <template-container :templates="templates" />
      </div>
    </div>
    {{templateData}}
    <div>{{templates}}</div>
  </div>
</template>

<script>
import TemplateForm from "@/components/TemplateForm.vue";
import TemplateContainer from "@/components/TemplateContainer.vue";

export default {
  props: {
    id: Number
  },
  name: "MyTemplate",
  data: function() {
    return {
      index: 1,
      templateData: {
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
    templateContainer: TemplateContainer
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
      console.log(this.templateData);
      // Webpack proxy API

      // axios.post('url', order).then((data) => {
      //   alert('Saved')
      // })
    },

    addNewTemplate() {
      this.templates.push({ id: this.index, template: this.templateData });
      // this.$set(this.templates, this.index, this.templateData);
      this.index += 1;
    }
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
