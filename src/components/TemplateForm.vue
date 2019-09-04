<template>
  <div class="uk-card uk-card-default uk-card-body">
    <!-- <h3 class="uk-card-title">Temaplate details</h3> -->
    <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-small-bottom">
      <p class="uk-h4">Create your template</p>
    </div>
    <!-- <form class=" uk-text-left uk-form-horizontal uk-margin-large ">  -->
    <form class="uk-form-stacked">
      <div class="uk-margin">
        <label class="uk-form-label label-text" for="form-width">Width:</label>
        <div class="uk-form-controls">
          <input class="uk-input" id="form-width" type="number" v-model="state.width" />
        </div>
      </div>

      <div class="uk-margin">
        <label class="uk-form-label label-text" for="form-height">Height:</label>
        <div class="uk-form-controls">
          <input class="uk-input" id="form-height" type="number" v-model="state.height" />
        </div>
      </div>

      <div class="uk-margin">
        <label class="uk-form-label label-text" for="form-width">Unit:</label>
        <div class="uk-margin uk-grid-small uk-child-width-auto uk-grid">
          <label>
            <input
              class="uk-radio"
              type="radio"
              name="radio-unit"
              :checked="state.units === 'cm'"
              @input="state.units='cm'"
            /> [ cm ]
          </label>
          <label>
            <input
              class="uk-radio"
              type="radio"
              name="radio-unit"
              :checked="state.units === 'mm'"
              @input="state.units='mm'"
            /> [ mm ]
          </label>
        </div>
      </div>

      <div class="uk-margin uk-margin-medium-bottom">
        <label class="uk-form-label label-text" for="form-width">Side to stick:</label>
        <div class="uk-margin uk-grid-small uk-child-width-auto uk-grid">
          <label>
            <input
              class="uk-checkbox"
              type="checkbox"
              :checked="state.glue.includes('t')"
              @input="setGlue('t')"
            /> Top
          </label>
          <label>
            <input
              class="uk-checkbox"
              type="checkbox"
              :checked="state.glue.includes('b')"
              @input="setGlue('b')"
            /> Bottom
          </label>
          <label>
            <input
              class="uk-checkbox"
              type="checkbox"
              :checked="state.glue.includes('l')"
              @input="setGlue('l')"
            /> Left
          </label>
          <label>
            <input
              class="uk-checkbox"
              type="checkbox"
              :checked="state.glue.includes('r')"
              @input="setGlue('r')"
            /> Right
          </label>
        </div>
      </div>
    </form>
    <div class="button-wraper">
      <button class="uk-button uk-button-primary">Preview</button>
      <button @click="addTemplate" class="uk-button uk-button-secondary">Add template</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "templateForm",
  data() {
    return {
      validateData: {
        maxWidthForCm: 280,
        maxHeightForCm: 207,
        maxWidthForMm: 2800,
        maxHeightForMm: 2070
      },
      state: {
        id: 0,
        width: 100,
        height: 100,
        units: "mm",
        glue: ""
      }
    };
  },
  watch: {
    state: {
      handler(old, val) {
        if (val.height < 0) {
          val.height = 0;
        }

        if (val.width < 0) {
          val.width = 0;
        }

        if (val.width > this.validateData.maxWidthForCm && val.units === "cm") {
          val.width = this.validateData.maxWidthForCm;
        }

        if (
          val.height > this.validateData.maxHeightForCm &&
          val.units === "cm"
        ) {
          val.height = this.validateData.maxHeightForCm;
        }

        if (val.width > this.validateData.maxWidthForMm && val.units === "mm") {
          val.width = this.validateData.maxWidthForMm;
        }

        if (
          val.height > this.validateData.maxHeightForMm &&
          val.units === "mm"
        ) {
          val.height = this.validateData.maxHeightForMm;
        }

        this.$emit("onUpdated", this.state);
      },
      deep: true
    }
  },
  methods: {
    setGlue(glue) {
      let v = new Set(this.state.glue);

      if (this.state.glue.includes(glue)) {
        v.delete(glue);
      } else {
        v.add(glue);
      }
      this.state.glue = Array.from(v.values()).join("");
    },
    addTemplate: function(event) {
      this.$emit("addTemplate");
    }
  }
};
</script>

<style lang="scss" scoped>
.label-text {
  font-weight: bold;
  font-size: 16px;
}

.button-wraper {
  display: flex;
  justify-content: space-between;
}
</style>