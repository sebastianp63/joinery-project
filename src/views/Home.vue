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


<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>