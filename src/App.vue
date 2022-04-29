<script setup>
import RollImage from "./components/roll-image/RollImage.vue";
import Gallery from "./components/gallery/Gallery.vue";
import { reactive, ref } from "@vue/reactivity";
import { onMounted } from "@vue/runtime-core";

const currentPage = ref("roll");

const isLocalStorageSupport = ref(false);
const isAddedToGallery = ref(false);
const currentImg = ref("");
const imageList = ref([]);

onMounted(() => {
  imageList.value = getFromLocalStorage();
});

function getFromLocalStorage() {
  if (localStorage.getItem("gallery")) {
    return JSON.parse(localStorage.getItem("gallery"));
  } else {
    localStorage.setItem("gallery", JSON.stringify([]));
    return JSON.parse(localStorage.getItem("gallery"));
  }
}

function storeToLocalStorage(data) {
  localStorage.setItem("gallery", JSON.stringify(data));
}

try {
  localStorage.setItem("test", "test");
  localStorage.removeItem("test");
  isLocalStorageSupport.value = true;
} catch (e) {
  isLocalStorageSupport.value = false;
}

function addToGallery(data) {
  isAddedToGallery.value = true;
  imageList.value.push(data);
  storeToLocalStorage(imageList.value);
}
function removeFromGallery(data) {
  if (data == currentImg.value) {
    isAddedToGallery.value = false;
  }
  imageList.value = imageList.value.filter((item) => item != data);
  storeToLocalStorage(imageList.value);
  console.log(data);
}

function changeIsAddedToGallery(val) {
  isAddedToGallery.value = val;
}

function changeCurrentImg(data) {}
</script>

<template>
  <div class="contener min-h-screen">
    <div class="mt-2 text-center">
      <button
        class="inline-block rounded-tl-md rounded-tr-md p-2 px-5 mx-2"
        :class="{
          'bg-white': currentPage == 'roll',
          'text-gray-900': currentPage == 'roll',
          'text-white': currentPage != 'roll',
          'bg-gray-400': currentPage != 'roll',
          'hover:bg-gray-500': currentPage != 'roll',
        }"
        @click="currentPage = 'roll'"
      >
        Roll
      </button>
      <button
        class="inline-block rounded-tl-md rounded-tr-md p-2"
        :class="{
          'bg-white': currentPage == 'gallery',
          'text-gray-900': currentPage == 'gallery',
          'text-white': currentPage != 'gallery',
          'bg-gray-400': currentPage != 'gallery',
          'hover:bg-gray-500': currentPage != 'gallery',
        }"
        @click="currentPage = 'gallery'"
      >
        Gallery
      </button>
    </div>

    <RollImage
      :changeIsAddedToGallery="changeIsAddedToGallery"
      :addToGallery="addToGallery"
      :removeFromGallery="removeFromGallery"
      :imageList="imageList"
      :isAddedToGallery="isAddedToGallery"
      :currentImg="currentImg"
      :class="{ hidden: currentPage != 'roll' }"
    />

    <Gallery
      :changeCurrentImg="changeCurrentImg"
      :currentImg="currentImg"
      :addToGallery="addToGallery"
      :removeFromGallery="removeFromGallery"
      :imageList="imageList"
      :class="{ hidden: currentPage != 'gallery' }"
    />
  </div>
  <div class="mt-8 py-4 bg-gray-900 italic text-center">
    <p class="text-white">
      Created by
      <a
        class="font-bold hover:text-blue-500"
        href="https://github.com/ridwanal24"
        >ridwanal24</a
      >
      using Vue 3 with <span class="text-red-600"> &hearts; </span>.
    </p>
  </div>
</template>

<style>
</style>
