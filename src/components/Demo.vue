<template>
  <div class="demo-wrapper">
    <div class="demo-line" />
    <div class="demo-container">
      <div :class="['demo-orb', dynamicClass]" :style="getOrbStyle()" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    hover: {
      type: Boolean,
      default: false,
    },
    transition: {
      type: String,
      default: "var(--quint)",
    },
  },
  data: () => ({
    toggle: true,
    timer: null,
  }),
  mounted() {},
  watch: {
    hover(val) {
      if (val) {
        this.toggle = !this.toggle;
        this.timer = setInterval(() => {
          this.toggle = !this.toggle;
        }, 1200);
      } else {
        clearInterval(this.timer);
        this.toggle = true;
      }
    },
  },
  computed: {
    dynamicClass() {
      return `toggle${this.toggle ? "A" : "B"}`;
    },
  },
  methods: {
    getOrbStyle() {
      return `transition: left 1000ms ${
        this.transition
      } 20ms, color 120ms var(--quart) 20ms; background: var(--handle-${
        !this.hover ? "dull" : "color"
      })`;
    },
  },
};
</script>

<style>
.demo-wrapper {
  position: relative;
  display: flex;
  margin: 10px 20px;
  justify-content: center;
  align-items: center;
}

.demo-orb {
  width: 12px;
  height: 12px;
  background: white;
  border-radius: 12px;
  position: absolute;
  bottom: -6px;
}

.toggleA {
  left: 0%;
}
.toggleB {
  left: 100%;
}

.demo-container {
  display: flex;
  justify-content: flex-start;
  width: 100%;
  position: relative;
}

.demo-line {
  position: absolute;
  width: 100%;
  background: var(--handle-dull);
  height: 2px;
  margin: 0px 20px;
}
</style>