<template>
  <div
    class="uk-width-3-4 uk-align-center uk-background-secondary uk-padding-small uk-background-primary uk-margin-medium-top"
  >
    <div class="uk-child-width-expand@s" uk-grid>
      <form action class="uk-form-horizontal">
        <label class="text-label uk-form-label" for="first-name-template">First name: {{firstName}}</label>
        <div class="uk-form-controls">
          <input
            @input="getFirstName"
            v-model="firstName"
            class="uk-input"
            id="first-name-template"
            type="text"
            placeholder
          />
        </div>
      </form>
      <form action class="uk-form-horizontal">
        <label class="text-label uk-form-label" for="last-name-template">Last name: {{lastName}}</label>
        <div class="uk-form-controls">
          <input
            @input="getLastName"
            v-model="lastName"
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
  data() {
    return {
      firstName: "",
      lastName: ""
    };
  },
  methods: {
    getFirstName() {
      var letters = /^[A-Za-z]+$/;
      if (this.firstName.match(letters)) {
        this.$emit("inputFirstName", this.firstName);
      } else {
        console.log("incorrect value");
      }
    },
    getLastName() {
      var letters = /^[A-Za-z]+$/;
      if (this.lastName.match(letters)) {
        this.$emit("inputLastName", this.lastName);
      } else {
        console.log("incorrect value");
      }
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