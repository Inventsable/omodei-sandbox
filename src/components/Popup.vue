<template>
  <div
    class="popup-wrapper"
    id="popup"
    ref="elt"
    v-if="state"
    :style="getPosition()"
  >
    <div class="popup-container">
      <div class="popup-content">
        {{ this.text }}
      </div>
      <div class="arrow-down-outline" />
      <div class="arrow-down" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    position: {
      type: Array,
      default: function () {
        return [100, 100];
      },
    },
    text: {
      type: String,
      default: "Copied to clipboard",
    },
    timeout: {
      type: Number,
      default: 2000,
    },
    state: {
      type: Boolean,
      default: false,
    },
    offset: {
      type: Number,
      default: 20,
    },
  },
  computed: {},
  data: () => ({
    realPos: [0, 0],
  }),
  mounted() {
    console.log("Hello");
    console.log(this.$refs);
  },
  watch: {
    state(val) {
      if (val) {
        this.$nextTick(() => {
          this.getRealPosition();
        });
        this.setClear();
        // this.debounce(this.setClear, 1200);
      }
    },
  },
  methods: {
    setClear() {
      setTimeout(() => {
        this.$emit("close");
      }, this.timeout);
    },
    refresh() {
      this.getRealPosition();
    },
    hide() {
      this.$emit("close");
    },
    getPosition() {
      let style = "";
      style = `
        left: ${this.realPos[0]}px;
        top: ${this.realPos[1]}px;
      `;
      return style;
    },
    getRealPosition() {
      let elt = this.$el;
      if (!elt) return null;
      try {
        let rect = this.$el.getBoundingClientRect();
        this.realPos = [
          this.position[0] - rect.width / 2,
          this.position[1] - rect.height - this.offset,
        ];
      } catch (err) {
        return null;
      }
    },
    debounce(func, delay) {
      let inDebounce;
      return function () {
        const context = this;
        const args = arguments;
        clearTimeout(inDebounce);
        inDebounce = setTimeout(() => func.apply(context, args), delay);
      };
    },
  },
};
</script>

<style>
.popup-wrapper {
  position: absolute;
  z-index: 10;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  flex-wrap: wrap;
  width: 220px;
}

.popup-content {
  width: 100%;
  padding: 20px 10px;
  background: var(--bg-col);
  border: 2px solid var(--text-col);
}
.arrow-down {
  position: absolute;
  width: 0;
  height: 0;
  border-left: 20px solid transparent;
  border-right: 20px solid transparent;
  border-top: 20px solid var(--bg-col);
  left: 91.5px;
  bottom: -18.5px;
}
.arrow-down-outline {
  position: absolute;
  bottom: -20.5px;
  left: 90px;
  width: 0;
  height: 0;
  border-left: 22px solid transparent;
  border-right: 22px solid transparent;
  border-top: 22px solid var(--text-col);
}
</style>