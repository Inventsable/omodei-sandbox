<template>
  <div class="cubic-bezier-wrapper">
    <div class="lazy-line-container" :style="getLazyStyle('horizontal')">
      <div class="lazy-line" :style="getLineStyle('horizontal')" />
    </div>
    <div class="lazy-line-container" :style="getLazyStyle('vertical')">
      <div class="lazy-line" :style="getLineStyle('vertical')" />
    </div>
    <svg
      v-if="isMounted"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      :width="size + anchorSize * 2 + padding * 2"
      :height="size + anchorSize * 2 + padding * 2"
      class="cubic-bezier-svg"
      :style="[
        {
          width: `${size + anchorSize * 2 + padding * 2}px`,
          height: `${size + anchorSize * 2 + padding * 2}px`,
        },
      ]"
    >
      <!-- <line x1="0" y1="100" x2="100" y2="0" stroke="#e0e0e0" stroke-width="4" /> -->
      <line
        :x1="points[0].x"
        :y1="points[0].y"
        :x2="points[0].x"
        :y2="points[3].y"
        stroke="var(--handle-dull)"
        :stroke-width="hover ? 0 : 2"
      />
      <line
        :x1="points[0].x"
        :y1="points[0].y"
        :x2="points[3].x"
        :y2="points[0].y"
        stroke="var(--handle-dull)"
        :stroke-width="hover ? 0 : 2"
      />
      <path
        :d="`M${points[0].x} ${points[0].y} C ${points[1].x} ${points[1].y}, ${points[2].x} ${points[2].y}, ${points[3].x} ${points[3].y}`"
        stroke="var(--handle-dull)"
        stroke-width="4"
        fill="transparent"
      />

      <line
        :x1="points[0].x"
        :y1="points[0].y"
        :x2="points[1].x"
        :y2="points[1].y"
        :stroke="hover ? 'var(--handle-color)' : 'var(--handle-dull)'"
        stroke-width="4"
        class="hover-stroke"
      />
      <line
        :x1="points[2].x"
        :y1="points[2].y"
        :x2="points[3].x"
        :y2="points[3].y"
        :stroke="hover ? 'var(--handle-color)' : 'var(--handle-dull)'"
        stroke-width="4"
        class="hover-stroke"
      />
      <circle
        :cx="points[1].x"
        :cy="points[1].y"
        r="5"
        :fill="hover ? 'var(--handle-color)' : 'var(--handle-dull)'"
        class="hover-fill"
      />
      <circle
        :cx="points[2].x"
        :cy="points[2].y"
        r="5"
        :fill="hover ? 'var(--handle-color)' : 'var(--handle-dull)'"
        class="hover-fill"
      />
      <rect
        :x="points[0].x - anchorSize"
        :y="points[0].y - anchorSize"
        :width="anchorSize * 2"
        :height="anchorSize * 2"
        :fill="hover ? 'var(--handle-color)' : 'var(--handle-dull)'"
        class="hover-fill"
      />
      <rect
        :x="points[3].x - anchorSize"
        :y="points[3].y - anchorSize"
        :width="anchorSize * 2"
        :height="anchorSize * 2"
        :fill="hover ? 'var(--handle-color)' : 'var(--handle-dull)'"
        class="hover-fill"
      />
    </svg>
  </div>
</template>

<script>
export default {
  props: {
    value: {
      type: String,
      default: null,
    },
    hover: {
      type: Boolean,
      default: false,
    },
    size: {
      type: Number,
      default: 100,
    },
    padding: {
      type: Number,
      default: 60,
    },
    anchorSize: {
      type: Number,
      default: 5,
    },
  },
  data: () => ({
    points: [],
    isMounted: false,
  }),
  mounted() {
    this.parsePointsFromValue();
    this.isMounted = true;
  },
  methods: {
    parsePointsFromValue() {
      let realPoints = [];
      if (/string/i.test(typeof this.value)) {
        let string = this.value.replace("cubic-bezier(", "").replace(/\)$/, "");
        let values = string.split(",").map((val) => {
          return +val;
        });
        this.createPointList(values);
      } else if (/object/i.test(typeof this.value)) {
        //
      }
    },
    createPointList(values) {
      values = values.map((value) => {
        return +(value * 100).toFixed(2);
      });
      this.points.push(
        this.normalizePoint({
          x: 0,
          y: 0,
        })
      );
      this.points.push(
        this.normalizePoint({
          x: values[0],
          y: values[1],
        })
      );
      this.points.push(
        this.normalizePoint({
          x: values[2],
          y: values[3],
        })
      );
      this.points.push(
        this.normalizePoint({
          x: 100,
          y: 100,
        })
      );
    },
    normalizePoint(obj) {
      return {
        x: obj.x + this.anchorSize + this.padding,
        y: obj.y * -1 + this.size + this.anchorSize + this.padding,
      };
    },
    getLazyStyle(direction) {
      let style = `
        position: absolute;
      `;
      if (direction == "horizontal") {
        style += `
          bottom: ${this.padding + this.anchorSize / 2 + 1}px;
          width: ${this.size}px;
        `;
      } else {
        style += `
          left: ${this.padding + this.anchorSize / 2 + 1}px;
          height: ${this.size}px;
          display: flex;
          align-items: flex-end;
        `;
      }
      return style;
    },
    getLineStyle(direction) {
      let style = ``;
      if (direction == "horizontal") {
        style += `
          height: 2px;
          width: ${this.hover ? 100 : 0}%;
        `;
      } else {
        style += `
          width: 2px;
          height: ${this.hover ? 100 : 0}%;
          animation-direction: reverse;
        `;
      }
      return style;
    },
  },
};
</script>

<style>
.cubic-bezier-wrapper svg {
  width: 100%;
  height: 100%;
}

.cubic-bezier-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: visible;
  position: relative;
}

.cubic-bezier-svg {
  /* border: 2px solid rgba(255, 255, 255, 0.1); */
}

.hover-stroke {
  transition: stroke 120ms var(--quart) 20ms;
}

.hover-fill {
  transition: fill 120ms var(--quart) 20ms;
}

.lazy-line-container: {
  /*  */
}
.lazy-line {
  background: rgba(255, 255, 255, 0.3);
  transition: height 200ms var(--quart) 20ms, width 200ms var(--quart) 20ms;
}
</style>