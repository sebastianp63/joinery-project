<template>
  <div class="uk-card uk-card-default uk-card-body">
    <div>
      <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-small-bottom">
        <p class="uk-h4">Template preview</p>
      </div>
      <div class="wrapper">
        <div class="top">
          <p
            v-if="templateData.width"
            class="width-text uk-text-large uk-text-secondary uk-text-bold"
          >{{templateData.width}} {{templateData.unit}}</p>
        </div>
        <div class="left">
          <p
            v-if="templateData.height"
            class="height-text uk-text-large uk-text-secondary uk-text-bold"
          >{{templateData.height}} {{templateData.unit}}</p>
        </div>
        <div
          class="bord center"
          v-bind:class="{
      'top-border': templateData.veneer.top ,
      'bottom-border': templateData.veneer.bottom,
      'left-border':templateData.veneer.left ,
      'right-border':templateData.veneer.right ,
      }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    templateData: Object,
  },
  methods: {},
};
</script>

<style lang="scss" scoped>
* {
  box-sizing: border-box;
}

.wrapper {
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(37.5px, auto);

  div {
    position: relative;
  }

  .bord {
    background-color: burlywood;
  }
}

.width-text {
  width: 100%;
  position: absolute;
  top: 100%;
  left: 50%;
  transform: translate(-50%, -75%);
}

.height-text {
  width: 200px;
  position: absolute;
  top: 50%;
  left: 100%;
  transform: rotate(270deg) translate(-50%, -75%);
  transform-origin: 0 0;
}

.top {
  grid-column: 2 / 7;
  grid-row: 1 / 2;
}

.left {
  grid-column: 1 /2;
  grid-row: 2/ 7;
}
.center {
  grid-column: 2 / 7;
  grid-row: 2 / 7;
}

.top-border {
  border-top: 5px solid brown;
}

.bottom-border {
  border-bottom: 5px solid brown;
}

.left-border {
  border-left: 5px solid brown;
}

.right-border {
  border-right: 5px solid brown;
}
</style>
