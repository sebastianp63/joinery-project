<template>
  <div class="uk-card uk-card-default uk-card-body">
    <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-medium-bottom">
      <p class="uk-h4">Create your template</p>
    </div>
    <form class="uk-form-stacked">
      <div class="uk-margin">
        <label class="uk-form-label label-text" for="form-width">Width:</label>
        <div class="uk-form-controls">
          <input
            class="uk-input"
            id="form-width"
            type="number"
            v-model="state.width"
            @keypress="isNumber($event)"
          />
        </div>
      </div>

      <div class="uk-margin">
        <label class="uk-form-label label-text" for="form-height">Height:</label>
        <div class="uk-form-controls">
          <input
            class="uk-input"
            id="form-height"
            type="number"
            v-model="state.height"
            @keypress="isNumber($event)"
          />
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
  </div>
</template>


<script>
import { eventBus } from "../main";

export default {
  name: "templateForm",
  props: ["id", "template"],

  data() {
    return {
      isShowedPreview: false,
      validateData: {
        maxWidthForCm: 280,
        maxHeightForCm: 207,
        maxWidthForMm: 2800,
        maxHeightForMm: 2070
      },
      state: {
        width: 100,
        height: 100,
        units: "mm",
        glue: ""
      }
    };
  },
  created: function() {
    this.state.width = this.template.width;
    this.state.height = this.template.height;
    this.state.units = this.template.units;
  },
  watch: {
    state: {
      handler(old, val) {
        const reg = new RegExp("^([1-9]|[1-9]d+)$");
        const test = reg.test(val.height);

        if (val.height < 0) {
          val.height = 0;
        }

        if (val.width < 0) {
          val.width = 0;
        }

        if (val.width.toString().charAt(0) == 0) {
          val.width = "";
        }

        if (val.height.toString().charAt(0) == 0) {
          val.height = "";
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
      const v = new Set(this.state.glue);

      if (this.state.glue.includes(glue)) {
        v.delete(glue);
      } else {
        v.add(glue);
      }
      this.state.glue = Array.from(v.values()).join("");
    },
    addTemplate(event) {
      this.$emit("addTemplate");
    },
    showPreview(event) {
      this.isShowedPreview = !this.isShowedPreview;
      this.$emit("showPreview", this.isShowedPreview);
    },
    isNumber($event) {
      const keyCode = $event.keyCode ? $event.keyCode : $event.which;
      if (keyCode < 48 || keyCode > 57) {
        $event.preventDefault();
      }
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