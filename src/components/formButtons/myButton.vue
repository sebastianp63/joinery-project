<template>
  <button
    @click="clickEvent"
    class="uk-button"
    v-bind:class="{'uk-button-secondary':secondary,'uk-button-primary': primary }"
  >{{text}}</button>
</template>

<script>
export default {
  name: "myButton",
  props: {
    primary: {
      type: Boolean,
      default: true
    },
    secondary: {
      type: Boolean,
      default: false
    },
    text: {
      type: String,
      required: true
    }
  },

  methods: {
    clickEvent(event) {
      console.log("clicked");
      this.$emit("clickEvent");
    }
  }
};
</script>