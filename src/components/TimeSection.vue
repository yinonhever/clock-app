<template>
  <section class="time">
    <div class="time__content">
      <div class="time__greeting">
        <img class="time__icon" :src="greetingIcon" :alt="greetingText" />
        <h4 class="time__greeting-text">
          {{ greetingText }}<span class="hidden-mobile">, it's currently</span>
        </h4>
      </div>
      <h1 class="time__heading">
        <span class="time__time">{{ time }}</span>
        <span class="time__timezone">{{ timezone }}</span>
      </h1>
      <h3 v-if="location" class="time__location">In {{ location }}</h3>
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
    datetime: [String, Object],
    timezone: String,
    city: String,
    countryCode: String,
    countryName: String,
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
      let location = "";
      if (this.city) {
        location += this.city;
        if (this.countryCode) location += `, ${this.countryCode}`;
      } else {
        if (this.countryName) location += this.countryName;
        else if (this.countryCode) location += this.countryCode;
      }
      return location;
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
  },
};
</script>
