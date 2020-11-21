<template>
  <section class="details">
    <div class="details__container">
      <div class="details__side">
        <div class="details__item">
          <h6 class="details__subtitle">Current timezone</h6>
          <h2 class="details__title">{{ location }}</h2>
        </div>
        <div class="details__item">
          <h6 class="details__subtitle">Day of the year</h6>
          <h2 class="details__title">{{ dayOfYear }}</h2>
        </div>
      </div>
      <div class="details__seperator" />
      <div class="details__side">
        <div class="details__item">
          <h6 class="details__subtitle">Day of the week</h6>
          <h2 class="details__title">{{ dayOfWeek }}</h2>
        </div>
        <div class="details__item">
          <h6 class="details__subtitle">Week number</h6>
          <h2 class="details__title">{{ week }}</h2>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: {
    timezone: String,
    dayOfWeek: Number,
    dayOfYear: Number,
    week: Number,
  },
  computed: {
    location() {
      return this.timezone.replace(/_/gi, " ");
    },
  },
};
</script>
