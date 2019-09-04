<template>
  <div class="home">
    <user-header />
    <my-template />
  </div>
</template>

<script>
// @ is an alias to /src

import MyTemplate from "../components/MyTemplate";
import UserHeader from "../components/UserHeader";

export default {
  name: "home",
  data: function() {
    return {};
  },
  components: {
    myTemplate: MyTemplate,
    userHeader: UserHeader
  }
};
</script>
