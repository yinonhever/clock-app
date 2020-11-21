<template>
  <section class="time">
    <div class="time__content">
      <div class="time__greeting">
        <img class="time__icon" :src="greetingIcon" :alt="greetingText" />
        <h4 class="time__greeting-text">{{ greetingText }}, it's currently</h4>
      </div>
      <h1 class="time__heading">
        <span class="time__time">{{ time }}</span>
        <span class="time__timezone">{{ timezone }}</span>
      </h1>
      <h3 class="time__location">In {{ location }}</h3>
    </div>
    <ToggleButton :drawerOpen="drawerOpen" @click="$emit('toggle')" />
  </section>
</template>

<script>
import moment from "moment";
import ToggleButton from "./ToggleButton";
import iconSun from "../assets/desktop/icon-sun.svg";
import iconMoon from "../assets/desktop/icon-moon.svg";

export default {
  components: { ToggleButton },
  props: {
    datetime: String,
    timezone: String,
    city: String,
    countryCode: String,
    drawerOpen: Boolean,
  },
  emits: ["toggle"],
  computed: {
    hour() {
      return moment(this.datetime).format("HH");
    },
    time() {
      const minutes = moment(this.datetime).format("mm");
      return `${this.hour}:${minutes} `;
    },
    location() {
      return `${this.city}, ${this.countryCode}`;
    },
    greetingText() {
      if (this.hour >= 5 && this.hour <= 11) return "Good morning";
      else if (this.hour >= 12 && this.hour <= 17) return "Good afternoon";
      else return "Good evening";
    },
    greetingIcon() {
      if (this.hour >= 5 && this.hour <= 17) return iconSun;
      else return iconMoon;
    },
    buttonMode() {
      return this.drawerOpen ? "less" : "more";
    },
  },
};
</script>
