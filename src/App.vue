<template>
  <calendar :initialYear="2017" :initialMonth="5" />
</template>

<script>
import Calendar from "./components/Calendar.vue";
export default {
  name: "App",
  components: { Calendar },
};
</script>

<style></style>
