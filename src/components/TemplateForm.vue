<template>
  <div class="uk-card uk-card-default uk-card-body">
    <div class="uk-tile uk-tile-secondary uk-padding-small uk-margin-medium-bottom">
      <p class="uk-h4">{{title}}</p>
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
              :checked="state.unit === 'cm'"
              @input="state.unit='cm'"
            /> [ cm ]
          </label>
          <label>
            <input
              class="uk-radio"
              type="radio"
              name="radio-unit"
              :checked="state.unit === 'mm'"
              @input="state.unit='mm'"
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
              :checked="state.veneer.top"
              @input="setGlue('t')"
            /> Top
          </label>
          <label>
            <input
              class="uk-checkbox"
              type="checkbox"
              :checked="state.veneer.bottom"
              @input="setGlue('b')"
            /> Bottom
          </label>
          <label>
            <input
              class="uk-checkbox"
              type="checkbox"
              :checked="state.veneer.left"
              @input="setGlue('l')"
            /> Left
          </label>
          <label>
            <input
              class="uk-checkbox"
              type="checkbox"
              :checked="state.veneer.right"
              @input="setGlue('r')"
            /> Right
          </label>
        </div>
      </div>
    </form>

    <div class="button-wraper">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
export default {
  props: {
    template: {
      type: Object,
      default: function() {
        return {};
      }
    },
    title: {
      type: String,
      required: true
    }
  },
  name: "templateForm",
  data() {
    return {
      validateData: {
        maxWidthForCm: 280,
        maxHeightForCm: 207,
        maxWidthForMm: 2800,
        maxHeightForMm: 2070
      },
      glue: "",
      state: {
        width: 100,
        height: 100,
        veneer: {
          top: false,
          bottom: false,
          left: false,
          right: false
        },
        unit: "mm"
      }
    };
  },
  created() {
    if (!_.isEmpty(this.template)) {
      this.state = this.template;
    }
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
      const v = new Set(this.glue);
      if (this.glue.includes(glue)) {
        v.delete(glue);
      } else {
        v.add(glue);
      }
      this.glue = Array.from(v.values()).join("");

      this.state.veneer.top = this.glue.includes("t");
      this.state.veneer.bottom = this.glue.includes("b");
      this.state.veneer.left = this.glue.includes("l");
      this.state.veneer.right = this.glue.includes("r");
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
