<template>
  <button :class="classes">
    <span class="toggler__text">{{ text }}</span>
    <span class="toggler__icon">
      <i class="fas fa-angle-down" />
    </span>
  </button>
</template>

<script>
export default {
  props: {
    drawerOpen: Boolean,
  },
  computed: {
    classes() {
      return this.drawerOpen ? "toggler active" : "toggler";
    },
    text() {
      return this.drawerOpen ? "Less" : "More";
    },
  },
};
</script>
