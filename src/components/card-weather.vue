<template>
  <div
    class="backdrop-blur-xs min-w-[70px] bg-white/10 rounded-md p-3 flex flex-col items-center"
  >
    <p class="text-white/75 underline underline-offset-2 mb-2">
      {{ moment(item?.dt_txt).format("HH:mm") }}
    </p>
    <div class="backdrop-blur-md bg-white/10 rounded-sm">
      <img
        alt="icon"
        :src="
          'https://openweathermap.org/img/wn/' +
          item?.weather[0]?.icon +
          '@2x.png'
        "
      />
    </div>
    <p class="text-white/75 mt-2">
      {{ Math.ceil(item?.main?.temp - 273.15) }}&deg;C
    </p>
  </div>
</template>

<script setup>
import moment from "moment";

const props = defineProps({
  item: null,
});
</script>
