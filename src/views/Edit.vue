<template>
  <div class="uk-width-3-4 uk-align-center">
    <div
      class="uk-tile uk-tile-secondary uk-padding-small uk-margin-medium-bottom uk-margin-large-top"
    >
      <p class="uk-h1 uk-text-uppercase">Edit template with id: #{{template.id}}</p>
    </div>

    <div class="uk-grid-small uk-child-width-expand@s uk-height-1-1" uk-grid>
      <div class="uk-width-1-3@m">
        <template-form :title="'Edit template'" :template="template">
          <my-button :text="'Edit'" :secondary="true"></my-button>
          <router-link to="/">Back to home</router-link>
        </template-form>
      </div>
      <div>
        <template-preview :templateData="template" class="uk-height-1-1"></template-preview>
      </div>
    </div>
  </div>
</template>

<script>
import TemplateForm from "../components/TemplateForm";
import TemplatePreview from "../components/TemplatePreview";
import MyButton from "../components/formButtons/MyButton";

export default {
  props: ["template"],
  components: {
    templateForm: TemplateForm,
    templatePreview: TemplatePreview,
    myButton: MyButton
  }
};
</script>