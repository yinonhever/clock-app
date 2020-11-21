<template>
  <Spinner v-if="loading" />
  <Error v-else-if="error" />
  <TheContainer v-else :active="showDrawer" :nightMode="nightMode">
    <QuoteSection v-bind="quote" @refresh="loadNewQuote" />
    <TimeSection
      v-bind="time"
      :drawerOpen="showDrawer"
      @toggle="toggleDrawer"
    />
    <DetailsDrawer v-bind="details" />
  </TheContainer>
</template>

<script>
import axios from "axios";
import moment from "moment";
import TheContainer from "./components/TheContainer";
import TimeSection from "./components/TimeSection";
import QuoteSection from "./components/QuoteSection";
import DetailsDrawer from "./components/DetailsDrawer";
import Spinner from "./components/Spinner";
import Error from "./components/Error";
import "./sass/main.scss";

export default {
  components: {
    TheContainer,
    TimeSection,
    QuoteSection,
    DetailsDrawer,
    Spinner,
    Error,
  },
  data() {
    return {
      time: {},
      details: {},
      quote: {},
      loading: true,
      error: false,
      showDrawer: false,
    };
  },
  computed: {
    nightMode() {
      const hour = moment(this.time.datetime).format("HH");
      return hour >= 18 || hour <= 4;
    },
  },
  methods: {
    loadData: async function() {
      try {
        const [detailsRes, locationRes] = await axios.all([
          axios.get("https://worldtimeapi.org/api/ip"),
          axios.get("https://freegeoip.app/json/"),
        ]);

        this.time = {
          datetime: detailsRes.data.datetime,
          timezone: detailsRes.data.abbreviation,
          city: locationRes.data.city,
          countryCode: locationRes.data.country_code,
        };

        this.details = {
          timezone: detailsRes.data.timezone,
          dayOfYear: detailsRes.data.day_of_year,
          dayOfWeek: detailsRes.data.day_of_week,
          week: detailsRes.data.week_number,
        };

        this.loading = false;

        setInterval(async () => {
          const { data } = await axios.get("https://worldtimeapi.org/api/ip");
          this.time = { ...this.time, datetime: data.datetime };
        }, 60000);
      } catch {
        this.error = true;
      }
    },
    loadNewQuote: async function() {
      this.quote.loading = true;

      const response = await axios.get("https://api.quotable.io/random");
      const { content, author } = response.data;

      this.quote = { content, author, loading: false };
    },
    toggleDrawer() {
      this.showDrawer = !this.showDrawer;
    },
  },
  mounted() {
    this.loadData();
    this.loadNewQuote();
  },
};
</script>
