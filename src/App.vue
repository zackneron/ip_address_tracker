<template>
  <router-view />
</template>

<style>
:root {
  --font: "Rubik", sans-serif;
  --dark-gray: hsl(0, 0%, 59%);
  --deep-dark-gray: hsl(0, 0%, 17%);
}
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: var(--font);
}
#app {
  text-align: center;
}
</style>
