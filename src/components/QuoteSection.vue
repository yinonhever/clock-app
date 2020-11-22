<template>
  <section class="quote">
    <div class="quote__wrapper">
      <div class="quote__container">
        <QuoteSpinner v-if="loading" />
        <div v-else class="quote__content">
          <p class="quote__text">“{{ content }}”</p>
          <p class="quote__author">{{ author }}</p>
        </div>
      </div>
      <i class="quote__refresh fas fa-sync-alt" @click="$emit('refresh')" />
    </div>
  </section>
</template>

<script>
import QuoteSpinner from "./QuoteSpinner";

export default {
  components: { QuoteSpinner },
  props: {
    content: String,
    author: String,
    loading: Boolean,
  },
  emits: ["refresh"],
};
</script>
