<template>
  <div>
    <Car />
  </div>
</template>

<script>
// import Earth from "./components/Earth.vue";
import Car from "./components/Car.vue";

export default {
  name: "App",
  components: {
    Car,
    // Earth,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
</style>
