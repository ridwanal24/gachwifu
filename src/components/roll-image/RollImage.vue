<script setup>
import { reactive, ref } from "vue";
import { opt } from "../../source/option-list.js";
import imageNull from "../../assets/image-null.png";
import LoadingCircle from "./LoadingCircle.vue";

const props = defineProps({
  isAddedToGallery: Boolean,
  imageList: Array,
  addToGallery: Function,
  removeFromGallery: Function,
  changeIsAddedToGallery: Function,
  currentImg: String,
});

const optionList = ref(opt);
const category = ref("waifu");
const currentImg = ref("");
const onFetching = ref(false);
const imgLoaded = ref(false);

async function fetchImage() {
  props.changeIsAddedToGallery(false);
  onFetching.value = true;
  currentImg.value = "";
  // changeImageLoaded(false);
  const res = await fetch(`https://api.waifu.pics/sfw/${category.value}`);
  const data = await res.json();
  currentImg.value = data.url;
  onFetching.value = false;

  if (props.imageList.includes(currentImg.value)) {
    props.changeIsAddedToGallery(true);
  }
}

// function changeImageLoaded(val) {
//   imgLoaded.value = val;
// }
</script>

<template>
  <div class="sm:w-full md:w-10/12 lg:w-8/12 m-auto">
    <div class="w-full rounded-md mx-auto bg-white mb-8 p-8">
      <form @submit.prevent="fetchImage">
        <select
          :disabled="onFetching"
          class="
            disabled:bg-gray-200
            hover:cursor-pointer
            w-full
            rounded
            p-1
            bg-white
            border-2 border-grey-500
          "
          v-model="category"
        >
          <option disabled>Select Category</option>
          <option
            selected
            v-for="option in optionList"
            :key="option.id"
            :value="option.value"
          >
            {{ option.value.charAt(0).toUpperCase() + option.value.slice(1) }}
          </option>
        </select>
        <button
          :disabled="onFetching"
          class="
            bg-blue-600
            text-white
            p-2
            w-full
            mx-auto
            block
            my-1
            rounded-md
            hover:bg-blue-700
            focus:bg-blue-800
            disabled:bg-gray-200
          "
        >
          ROLL IMAGE
        </button>
      </form>
    </div>
    <div class="currentImg bg-white rounded-md p-8">
      <div class="text-right">
        <button
          class="text-white p-1 my-1 rounded-md"
          :class="{
            'bg-green-600': !isAddedToGallery,
            'hover:bg-green-700': !isAddedToGallery,
            'focus:bg-green-800': !isAddedToGallery,
            'bg-gray-200': isAddedToGallery,
            'hover:bg-gray-300': isAddedToGallery,
            'focus:bg-gray-400': isAddedToGallery,
            hidden: currentImg == '',
          }"
          @click="
            isAddedToGallery
              ? removeFromGallery(currentImg)
              : addToGallery(currentImg)
          "
        >
          {{
            props.isAddedToGallery ? "Remove from gallery" : "Add to gallery"
          }}
        </button>
      </div>
      <img
        v-if="!onFetching && currentImg == ''"
        class="mx-auto w-full"
        :src="imageNull"
        alt="Waifu Image"
      />
      <LoadingCircle class="absolute" v-else-if="onFetching" />
      <img
        class="mx-auto"
        :class="{ hidden: currentImg == '' }"
        :src="currentImg"
        alt="Waifu Image"
      />
    </div>
  </div>
</template>

<style>
</style>

        :class="{ hidden: !imgLoaded }"