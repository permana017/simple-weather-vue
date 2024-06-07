<template>
  <div class="w-full md:max-w-sm h-full backdrop-blur-md bg-white/10 p-4 py-10">
    <div class="w-full flex justify-center md:m-0 px-5">
      <select
        id=""
        class="bg-transparent border-[1px] rounded-md border-white/75 p-2 text-white/75 w-full focus:outline-none"
        @change="handleSelect"
      >
        <option
          v-for="(item, index) in cities"
          :key="index"
          class="text-black bg-gray-400 backdrop-blur-sm max-w-[80%]"
          :value="item"
        >
          <p class="py-2">
            {{ item }}
          </p>
        </option>
      </select>
    </div>
    <div
      class="border-b-[1px] border-white/60 flex flex-col items-center py-8 mb-5"
    >
      <p class="text-lg font-semibold text-white text-center">
        {{ dataWeather[0]?.weather[0]?.description }}
      </p>
      <p v-if="dataWeather.length" class="text-6xl text-white py-5">
        {{ Math.ceil(dataWeather[0]?.main?.temp - 273.15) }}
        &deg;C
      </p>
      <p v-else class="text-6xl text-white py-5">0 &deg;C</p>
      <p class="text-white/60">
        wind speed
        {{ dataWeather[0]?.wind?.speed }}
      </p>
    </div>
    <p class="text-lg font-semibold text-white text-center py-3">
      The Five Day Forecast
    </p>
    <div class="flex flex-col items-center w-full overflow-auto h-full px-5">
      <div v-if="dataWeather.length" class="w-full">
        <template v-for="(item, index) in dataWeather" :key="index">
          <card-forecast
            v-if="formattedDate(item.dt_txt, 'HH') == '09'"
            :item="item"
          />
        </template>
      </div>
      <div v-else class="w-full flex flex-col gap-3 py-5">
        <template v-for="(item, index) in [1, 2, 3, 4]" :key="index">
          <div class="flex gap-2 backdrop-blur-0 p-2 rounded-md">
            <div class="w-14 h-12 bg-slate-500 rounded animate-pulse"></div>
            <div class="w-full flex flex-col justify-between">
              <p class="p-2.5 rounded bg-slate-500 w-full animate-pulse"></p>
              <p class="p-2 rounded bg-slate-500 w-40 animate-pulse"></p>
            </div>
          </div>
        </template>
      </div>
    </div>
  </div>
</template>
<script setup>
import moment from "moment";
import cardForecast from "./card-forecast.vue";

const props = defineProps({
  dataWeather: {
    type: Array,
    default: [],
  },
});

const emit = defineEmits(["select"]);

const cities = [
  "Jakarta",
  "Denpasar",
  "Bandung kota",
  "Surabaya",
  "Yogyakarta",
  "Padang",
  "Makassar",
  "Sumedang",
];

const handleSelect = (e) => {
  const { value } = e.target;
  emit("select", value);
};

const formattedDate = (date, format) => {
  return moment(date).format(format);
};
</script>
