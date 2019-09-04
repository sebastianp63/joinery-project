<template>
  <div
    class=" uk-width-3-4 uk-align-center uk-background-secondary uk-padding-small uk-background-primary uk-margin-medium-top"
  >
    <div class="uk-child-width-expand@s" uk-grid>
      <form action class="uk-form-horizontal">
        <label class="text-label uk-form-label" for="first-name-template">First name:</label>
        <div class="uk-form-controls">
          <input
            @input="getFirstName"
            class="uk-input"
            id="first-name-template"
            type="text"
            placeholder
          />
        </div>
      </form>
      <form action class="uk-form-horizontal">
        <label class="text-label uk-form-label" for="last-name-template">Last name:</label>
        <div class="uk-form-controls">
          <input
            @input="getLastName"
            class="uk-input"
            id="last-name-template"
            type="text"
            placeholder
          />
        </div>
      </form>
    </div>
  </div>
</template>

<script>
export default {
  name: "UserHeader",
  props: {
    firstName: String,
    lastName: String
  },
  methods: {
    getFirstName(e) {
      this.$emit("inputFirstName", e.target.value);
    },
    getLastName(e) {
      this.$emit("inputLastName", e.target.value);
    }
  }
};
</script>

<style lang="scss" scoped>
  .text-label {
    color: white;
    font-weight: bold;
    font-size: 20px;

  }
</style>