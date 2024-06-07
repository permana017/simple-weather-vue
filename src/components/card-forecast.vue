<template>
  <div class="flex items-center w-full justify-between mt-5">
    <div class="flex">
      <div class="w-12 h-12 backdrop-blur-md bg-white/10 rounded-md mr-2">
        <img
          alt="icon"
          :src="
            `https://openweathermap.org/img/wn/` +
            item?.weather[0]?.icon +
            `@2x.png`
          "
        />
      </div>
      <div>
        <p class="text-white">
          {{ moment(item?.dt_txt).format("dddd MMMM DD") }}
        </p>
        <p class="text-white/60 text-sm">
          {{ item?.weather[0]?.description }}
        </p>
      </div>
    </div>
    <div class="flex flex-col justify-center h-full p-0">
      <p class="text-white border-l-[1px] pl-4">
        {{ Math.ceil(item?.main?.temp - 273.15) }}&deg;
      </p>
    </div>
  </div>
</template>
<script setup>
import moment from "moment";

const props = defineProps({
  item: null,
});
</script>
