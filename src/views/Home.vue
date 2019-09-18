<template>
  <div>
    <user-header @inputFirstName="firstName = $event" @inputLastName="lastName = $event" />
    <my-template :firstName="firstName" :lastName="lastName" />
  </div>
</template>

<script>
// @ is an alias to /src

import MyTemplate from "../components/MyTemplate.vue";
import UserHeader from "../components/UserHeader.vue";

export default {
  name: "home",
  data() {
    return {
      firstName: "",
      lastName: ""
    };
  },
  components: {
    myTemplate: MyTemplate,
    userHeader: UserHeader
  }
};
</script>


