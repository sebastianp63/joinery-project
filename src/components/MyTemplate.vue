<template>
  <div
    class="uk-margin-small-left uk-margin-small-right border-template .uk-background-default uk-align-center uk-padding-small"
  >
    <div class="uk-child-width-expand@s uk-text-center" uk-grid>
      <div class="uk-width-1-3@m">
        <template-form />
      </div>
      <div class="uk-width-expand@m">
        <template-container />
      </div>
    </div>
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
      isHidden: false,
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
      }
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

      // Webpack proxy API

      // axios.post('url', order).then((data) => {
      //   alert('Saved')
      // })
    },
    showData: function(value) {
      this.width = value;
      console.log(this.width);
    },
    hideTemplate(data) {
      data ? (this.isHidden = true) : (this.isHidden = false);
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
