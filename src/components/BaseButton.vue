<template>
  <button @click="buttonClick" class="btn" :class="[bgcolor, txtcolor]">
    {{ label }}
  </button>
</template>
<script>
export default {
  props: ['bgcolor', 'txtcolor', 'label'],
  emits: ['butt-click'],
  methods: {
    buttonClick() {
      this.$emit('butt-click', this.label)
    }
  }
}
</script>