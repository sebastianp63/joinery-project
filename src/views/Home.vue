<template>
  <div class="home">
    <user-header />
    <my-template @emitData="tab.push($event)" />
    {{tab}}
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
      tab: []
    };
  },
  components: {
    myTemplate: MyTemplate,
    userHeader: UserHeader
  }
};
</script>
