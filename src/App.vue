

<template>
  <TrackerTime />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import TrackerTime from "./components/TrackerTime.vue";

export default defineComponent({
  name: "App",
  components: { TrackerTime },
  data() {
    return {};
  },
});
</script>

<style lang="scss">
body {
  background-color: #e5e5e5;
}
</style>
