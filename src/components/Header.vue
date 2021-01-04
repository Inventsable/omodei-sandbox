<template>
  <div class="title-header">
    <div class="title-spacer"></div>
    <span class="title-content">{{ title }}</span>
    <div class="title-spacer"></div>
  </div>
</template>

<script>
export default {
  props: {
    title: {
      type: String,
      default: "space mono",
    },
  },
};
</script>

<style>
.title-header {
  display: flex;
  justify-content: center;
  flex-wrap: nowrap;
  align-items: center;
  width: 100%;
}

.title-content {
  white-space: nowrap;
  padding: 0px 40px;
  letter-spacing: 0.4ch;
}

.title-spacer {
  height: 2px;
  width: 100%;
  background: var(--text-col);
}
</style>