<template>
  <button @click="buttonClick" class="btn" :class="[bgcolor, txtcolor]">
    {{ label }}
  </button>
</template>
<script>
export default {
  props: ["bgcolor", "txtcolor", "label"],
};
</script>