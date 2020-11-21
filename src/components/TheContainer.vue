<template>
  <main :class="classes">
    <div class="container__bg" />
    <slot />
  </main>
</template>

<script>
export default {
  props: {
    active: Boolean,
    nightMode: Boolean,
  },
  computed: {
    classes() {
      let classes = "container";
      if (this.nightMode) classes += " night";
      if (this.active) classes += " active";
      return classes;
    },
  },
};
</script>
