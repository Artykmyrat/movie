<script setup>
import axios from "axios";
import { ref } from "vue";

const data = ref([]);
const loading = ref(false);
const error = ref(null);
const err = ref(false);
const info = ref("");
const showInfo = ref(false);
const isdark = ref(true);

async function fetchData() {
  showInfo.value = false;
  try {
    err.value = false;
    loading.value = true;
    const response = await axios.get(
      "http://www.omdbapi.com/?apikey=c0e3b734&t=" + info.value
    );
    data.value = response.data;
  } catch (err) {
    error.value = err.message;
    err.value = true;
  } finally {
    loading.value = false;
    err.value = false;
    showInfo.value = true;
  }

  if (data.value.Response == "False") {
    err.value = true;
    showInfo.value = false;
  }
}

async function changeTheme() {
  document.getElementById("theme").classList.toggle("dark");
  isdark.value = !isdark.value;
}
</script>

<template>
  <div class="dark dark:text-white" id="theme">
    <div
      class="relative flex justify-center items-center h-screen dark:bg-slate-800 bg-white"
    >
      <div
        class="dark:bg-slate-700 dark:text-white bg-slate-100 w-10/15 min-w-100 transition-all rounded-4xl drop-shadow-lg p-5 max-w-7xl"
      >
        <div class="flex gap-3">
          <input
            type="text"
            v-model="info"
            placeholder="Search Movie"
            class="rounded-full p-4 text-black out-line-none border border-slate-400 focus:outline-none border-md w-full dark:text-white"
            @keyup.enter="fetchData"
          />
          <button
            class="bg-trnasparent dark:text-white hover:bg-blue-500 transition text-slate-600 font-semibold hover:text-white py-2 px-4 border border-blue-500 hover:border-transparent font-bold p-4 px-8 rounded-full"
            @click="fetchData"
          >
            Search
          </button>
        </div>
        <div class="">
          <div class="text-center m-10 text-xl" v-show="loading">
            Loading.....
          </div>
          <div class="text-center m-10 text-xl" v-show="err">Error.....</div>
        </div>
        <div class="flex mt-5 gap-10" v-show="showInfo">
          <img
            class="w-100 rounded-4xl"
            :src="data.Poster"
            alt="Movie Poster"
          />
          <div class="flex flex-col max-w-1/2 justify-center mb-50">
            <div class="text-5xl font-semibold mb-10">
              {{ data.Title }}
            </div>
            <div class="text-[22px] flex gap-1 mb-5">
              Year:
              <div class="font-semibold">{{ data.Year }}</div>
            </div>
            <div class="text-[22px] flex gap-1 mb-5">
              Country:
              <div class="font-semibold">{{ data.Country }}</div>
            </div>
            <div class="text-[22px] flex gap-1 mb-5">
              Actors:
              <div class="font-semibold">{{ data.Actors }}</div>
            </div>
            <div class="text-xl mb-5">
              {{ data.Plot }}
            </div>
            <div class="text-[22px] flex gap-1">
              Genre:
              <div class="font-semibold">{{ data.Genre }}</div>
            </div>
            <div class="text-[22px] flex gap-1 mt-10">
              Rating:
              <div class="font-semibold">{{ data.imdbRating }}</div>
            </div>
          </div>
        </div>
      </div>
      <div
        class="transition scale-90 absolute bottom-10 right-10 z-10 dark:text-white bg-slate-700 dark:bg-white w-15 h-15 flex justify-center items-center rounded-md"
      >
        <button
          class="w-15 h-15 flex items-center justify-center"
          @click="changeTheme"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            x="0px"
            y="0px"
            width="40"
            height="40"
            viewBox="0 0 24 24"
            v-show="isdark"
          >
            <path
              d="M 11 0 L 11 3 L 13 3 L 13 0 L 11 0 z M 4.2226562 2.8085938 L 2.8085938 4.2226562 L 4.9296875 6.34375 L 6.34375 4.9296875 L 4.2226562 2.8085938 z M 19.777344 2.8085938 L 17.65625 4.9296875 L 19.070312 6.34375 L 21.191406 4.2226562 L 19.777344 2.8085938 z M 12 5 C 8.1458514 5 5 8.1458514 5 12 C 5 15.854149 8.1458514 19 12 19 C 15.854149 19 19 15.854149 19 12 C 19 8.1458514 15.854149 5 12 5 z M 12 7 C 14.773268 7 17 9.2267316 17 12 C 17 14.773268 14.773268 17 12 17 C 9.2267316 17 7 14.773268 7 12 C 7 9.2267316 9.2267316 7 12 7 z M 0 11 L 0 13 L 3 13 L 3 11 L 0 11 z M 21 11 L 21 13 L 24 13 L 24 11 L 21 11 z M 4.9296875 17.65625 L 2.8085938 19.777344 L 4.2226562 21.191406 L 6.34375 19.070312 L 4.9296875 17.65625 z M 19.070312 17.65625 L 17.65625 19.070312 L 19.777344 21.191406 L 21.191406 19.777344 L 19.070312 17.65625 z M 11 21 L 11 24 L 13 24 L 13 21 L 11 21 z"
            ></path>
          </svg>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            height="30px"
            width="30px"
            version="1.1"
            id="Capa_1"
            viewBox="0 0 56 56"
            xml:space="preserve"
            v-show="!isdark"
          >
            <path
              style="fill: #ffffff"
              d="M29,28c0-11.917,7.486-22.112,18-26.147C43.892,0.66,40.523,0,37,0C21.561,0,9,12.561,9,28  s12.561,28,28,28c3.523,0,6.892-0.66,10-1.853C36.486,50.112,29,39.917,29,28z"
            />
          </svg>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
