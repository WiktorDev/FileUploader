<template>
  <div id="app">
    <FileUpload />
  </div>
</template>

<script>
import FileUpload from "./components/FileUpload.vue";
export default {
  name: "app",
  components: {
    FileUpload
  }
};
</script>
<style>
@tailwind base;
@tailwind components;
@tailwind utilities;
</style>