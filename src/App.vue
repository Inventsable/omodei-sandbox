<template>
  <div id="app">
    <router-view />
  </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Space+Mono:wght@400;700&display=swap");

:root {
  --text-col: #d7d7d7;
  --bg-col: #1c1c1c;
  --back: #000;
  --handle-color: #00fb6a;
  --handle-dull: #333;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: var(--text-col);
  background: var(--bg-col);

  display: flex;
  justify-content: center;
  font-family: "Space Mono", monospace;
}

body,
html {
  margin: 0;
  padding: 0;
}
</style>
