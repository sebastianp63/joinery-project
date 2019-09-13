<template>
  <div>
    <div
      class="uk-width-3-4 uk-align-center uk-background-secondary uk-padding-small uk-background-primary uk-margin-medium-top"
    >
      <div class="uk-child-width-expand@s" uk-grid>
        <form action class="uk-form-horizontal">
          <label class="text-label uk-form-label" for="first-name-template">First name:</label>
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
          <label class="text-label uk-form-label" for="last-name-template">Last name:</label>
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
    <div
      v-show="incorrectLastName || incorrectFirtName "
      class="uk-width-3-4 uk-align-center uk-alert-danger"
      uk-alert
    >
      <a class="uk-alert-close" uk-close></a>
      <p>Invalid user firstname or lastname</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'UserHeader',
  data() {
    return {
      firstName: '',
      lastName: '',
      incorrectFirtName: false,
      incorrectLastName: false,
    };
  },
  methods: {
    getFirstName() {
      const letters = /^[A-Za-z]+$/;
      if (this.firstName.match(letters)) {
        this.$emit(
          'inputFirstName',
          this.firstName.charAt(0).toUpperCase() + this.firstName.slice(1),
        );
        this.incorrectFirtName = false;
      } else {
        this.$emit('inputFirstName', '');
        this.incorrectFirtName = true;
      }
    },
    getLastName() {
      const letters = /^[A-Za-z]+$/;
      if (this.lastName.match(letters)) {
        this.$emit(
          'inputLastName',
          this.lastName.charAt(0).toUpperCase() + this.lastName.slice(1),
        );
        this.incorrectLastName = false;
      } else {
        this.$emit('inputLastName', '');
        this.incorrectLastName = true;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.text-label {
  color: white;
  font-weight: bold;
  font-size: 20px;
}
</style>
