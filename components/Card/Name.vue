<template>
  <div class="card">
    <p>{{ name }}</p>
    <b @click="removeName">X</b>
  </div>
</template>

<script setup>
const props = defineProps({
  name: {
    type: String,
  },
});

const emit = defineEmits(["remove"]);
const removeName = function () {
  emit("remove");
};
</script>

<style></style>
