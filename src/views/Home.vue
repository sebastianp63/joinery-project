<template>
  <div class="home">
    <user-header @inputFirstName="firstName = $event" @inputLastName="lastName = $event" />
    <my-template :firstName="firstName" :lastName="lastName" />
  </div>
</template>

<script>
// @ is an alias to /src

import MyTemplate from "../components/MyTemplate";
import UserHeader from "../components/UserHeader";

export default {
  name: "home",
  data: function() {
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
