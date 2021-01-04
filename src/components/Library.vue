<template>
  <div class="library-wrapper">
    <!-- <Popup
      ref="popup"
      :state="state"
      :position="mousePos"
      @close="state = false"
    /> -->
    <div class="library-name">
      <Header :title="parseCamelCase(name)" />
    </div>
    <div class="library-content">
      <div
        class="library-value"
        v-for="i in values"
        :key="i.name"
        @mouseenter="i.hover = true"
        @mouseleave="i.hover = false"
        @click="copyAndDisplay(i.value, $event)"
      >
        <div class="library-value-preview">
          <Cubic-Bezier :value="i.value" :hover="i.hover" />
        </div>
        <div class="library-value-name">
          {{ parseCamelCase(i.name) }}
        </div>
        <div
          class="library-value-display"
          :style="[
            {
              color: i.hover ? 'var(--text-col)' : 'transparent',
            },
          ]"
        >
          {{ i.value.replace("cubic-bezier(", "").replace(/\)$/, "") }}
        </div>
        <div class="library-value-demo">
          <Demo :transition="i.value" :hover="i.hover" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { copy } from "cluecumber";

export default {
  props: {
    library: {
      type: Object,
      default: function () {
        return {};
      },
    },
    name: {
      type: String,
      default: "none",
    },
  },
  components: {
    "Cubic-Bezier": require("./CubicBezier.vue").default,
    Header: require("./Header.vue").default,
    Demo: require("./Demo.vue").default,
    Popup: require("./Popup.vue").default,
  },
  mounted() {
    this.constructValues();
  },
  data: () => ({
    values: [],
    mousePos: [0, 0],
    state: false,
  }),
  methods: {
    async copyAndDisplay(value, evt) {
      await copy(value);
      this.mousePos = [evt.pageX, evt.pageY];
      this.state = true;
      alert(`Copied "${value}" to clipboard`);
      // this.$refs.popup.refresh();
    },
    parseCamelCase(string) {
      if (!/[a-z][A-Z].*/.test(string)) return string;
      let temp = string.match(/[A-Z].*/);
      let prefix = string.replace(new RegExp(temp[0]), "");
      return `${prefix} ${temp[0]}`;
    },
    constructValues() {
      this.values = [];
      Object.keys(this.library).forEach((key) => {
        this.values.push({
          name: key,
          value: this.library[key],
          hover: false,
        });
      });
    },
  },
};
</script>

<style>
.library-wrapper {
  /* border: 2px solid red; */
  display: flex;
  justify-content: flex-start;
  flex-wrap: wrap;
  padding: 20px 0px;
}

.library-content {
  display: flex;
  flex-wrap: wrap;
}

.library-value {
  margin-bottom: 24px;
  cursor: pointer;
}

.library-name {
  width: 100%;
  margin: 20px;
  font-weight: 700;
  height: 60px;
  min-height: 60px;
  max-height: 60px;
  box-sizing: border-box;
}

.library-value-display {
  text-transform: none;
  font-size: 11px;
  transition: color 120ms var(--quint) 20ms;
  user-select: none;
}
</style>