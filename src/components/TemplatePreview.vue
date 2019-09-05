<template>
  <div class="uk-card uk-card-default uk-card-body">
    <div>
      <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-small-bottom">
        <p class="uk-h4">Template preview</p>
      </div>
      <div class="wrapper">
        <div class="one">
          <p
            class="width-text uk-text-large uk-text-secondary uk-text-bold"
          >{{templateData.width}} {{templateData.unit}}</p>
        </div>
        <div class="two"></div>
        <div class="three"></div>
        <div class="four">
          <p
            class="height-text uk-text-large uk-text-secondary uk-text-bold"
          >{{templateData.height}} {{templateData.unit}}</p>
        </div>
        <div
          class="bord five"
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
    templateData: Object
  },
  methods: {}
};
</script>

<style lang="scss" scoped>
* {
  box-sizing: border-box;
}
.wrapper {
  max-width: 940px;
  margin: 0 auto;

  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-gap: 10px;
  grid-auto-rows: minmax(100px, auto);

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
  width: 100%;
  position: absolute;
  top: 50%;
  left: 100%;
  transform: rotate(270deg) translate(-50%, -75%);
  transform-origin: 0 0;
}

.one {
  grid-column: 2 / 4;
  grid-row: 1 / 2;
}
.two {
  grid-column: 4 / 5;
  grid-row: 2 / 4;
}
.three {
  grid-column: 2/4;
  grid-row: 4 / 5;
}
.four {
  grid-column: 1 /2;
  grid-row: 2/ 4;
}
.five {
  grid-column: 2 / 4;
  grid-row: 2 / 4;
}

.vertical-text {
  writing-mode: vertical-rl;
  text-orientation: mixed;
}

.btn-top {
  position: absolute;
  height: 60px;
  bottom: 0;
  left: 0;
  width: 100%;
}

.btn-right {
  position: absolute;
  top: 0;
  left: 0;
  width: 60px;

  height: 100%;
}

.btn-bottom {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 50px;
}

.btn-left {
  position: absolute;
  top: 0;
  right: 0;
  width: 60px;
  height: 100%;
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