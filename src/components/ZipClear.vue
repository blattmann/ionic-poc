<template>
  <ion-button
    v-if="info"
    color="light"
    expand="block"
    @click="$emit('clear-info')">
    Clear
  </ion-button>
</template>

<script>
export default {
  name: 'ClearInfo',
  props: {
    info: {
      type: Object,
      default: () => ({}),
    },
  },
};
</script>

