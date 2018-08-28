<template>
  <div id="app">
    <vue-pagination :total="100"></vue-pagination>
  </div>
</template>

<script>
// # import VuePagination from "./components/VuePagination";
import VuePagination from "@vv13/vue-pagination";

export default {
  name: "app",

  data() {
    return {
      page: 2
    };
  },
  components: {
    VuePagination
  },
  mounted() {
    setTimeout(() => {
      this.page = 5;
    }, 1000);
  }
};
</script>
