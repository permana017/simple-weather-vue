<template>
  <main class="bg-main w-screen h-screen flex overflow-hidden">
    <div
      class="w-full hidden md:flex overflow-hidden flex-col p-5 pb-2 justify-between"
    >
      <div class="w-full flex justify-end items-center">
        <p class="text-white border-r-2 pr-4">
          {{ formattedDate(dataWeather[0]?.dt_txt, "DD MMMM YY") }}
        </p>
        <p class="text-white ml-4">
          {{ realtime }}
        </p>
      </div>
      <div class="flex w-full flex-col">
        <div class="w-full border-b-[3px] py-9 mb-4 border-white/75">
          <p
            class="text-4xl md:text-5xl lg:text-6xl xl:text-7xl font-normal text-white/75 text-right w-full"
          >
            {{ dataWeather[0]?.weather[0]?.description }}
          </p>
        </div>
        <div
          v-if="dataWeather.length"
          class="w-full flex justify-between gap-x-3 overflow-auto scrollbar-custom pb-3"
        >
          <template v-for="(item, index) in dataWeather" :key="index">
            <cardWeather v-if="index < 12" :item="item" />
          </template>
        </div>
        <div
          v-else
          class="w-full flex justify-between gap-x-3 overflow-auto scrollbar-custom pb-3"
        >
          <template v-for="index in 12" :key="index">
            <div
              class="backdrop-blur-xs min-w-[70px] bg-white/10 rounded-md p-3 flex flex-col gap-3 items-center"
            >
              <div class="w-full p-2 bg-slate-500 rounded animate-pulse"></div>
              <div
                class="w-full p-2 py-5 bg-slate-500 rounded animate-pulse"
              ></div>
              <div class="w-full p-2 bg-slate-500 rounded animate-pulse"></div>
            </div>
          </template>
        </div>
      </div>
    </div>
    <side-info :dataWeather="dataWeather" @select="handleSelect" />
  </main>
</template>

<script setup>
import { onMounted, reactive, ref } from "vue";
import cardWeather from "./card-weather.vue";
import axios from "axios";
import sideInfo from "./side-info.vue";
import moment from "moment";

// register in https://api.openweathermap.org to get appid
const appId = import.meta.env.VITE_SECRET_APPID;
const baseUrl = "https://api.openweathermap.org";
const dataWeather = ref([]);
const realtime = ref("");
const city = ref("");
const cityCordinate = reactive({
  lat: "",
  lon: "",
});

const setRealtime = () => {
  realtime.value = moment(new Date()).format("HH: mm : ss");
};

setInterval(setRealtime, 1000);

const getCityCordinate = async () => {
  try {
    const { data } = await axios.get(
      `${baseUrl}/geo/1.0/direct?q=${
        city.value ? city.value : "jakarta"
      }&appid=${appId}`
    );
    cityCordinate.lat = data[0].lat;
    cityCordinate.lon = data[0].lon;
    getDataWeather();
  } catch (error) {
    console.log(error);
  }
};

const getDataWeather = async () => {
  try {
    const { data } = await axios.get(
      `${baseUrl}/data/2.5/forecast?lat=${cityCordinate.lat}&lon=${cityCordinate.lon}&appid=${appId}`
    );
    dataWeather.value = data.list;
  } catch (error) {
    console.log(error);
  }
};

const handleSelect = (value) => {
  city.value = value;
  getCityCordinate();
};

const formattedDate = (date, format) => {
  return moment(date).format(format);
};

onMounted(() => {
  getCityCordinate();
});
</script>

<style scoped>
.bg-main {
  background-image: url("../assets/bg-rain.jpg");
  background-size: cover;
}

.scrollbar-custom::-webkit-scrollbar {
  width: 12px;
  height: 12px;
}

.scrollbar-custom::-webkit-scrollbar-thumb {
  background-color: gray;
  border-radius: 10px;
}

.scrollbar-custom::-webkit-scrollbar-track {
  border-radius: 10px;
}
</style>
