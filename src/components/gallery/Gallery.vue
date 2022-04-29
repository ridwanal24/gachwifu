<script setup>
import { ref } from "vue";
import galleryEmpty from "../../assets/gallery-empty.png";

const props = defineProps({
  isAddedToGallery: Boolean,
  imageList: Array,
  removeFromGallery: Function,
  changeCurrentImg: Function,
});

// console.log(props.ima);
function isImageListEmpty() {
  return props.imageList.length == 0;
}
</script>

<template>
  <div class="sm:w-full md:w-10/12 lg:w-10/12 m-auto">
    <div class="w-full rounded-md mx-auto bg-white mb-8 p-8">
      <div
        v-for="image in imageList"
        :key="image"
        class="
          lg:w-4/12
          md:w-6/12
          w-full
          hover:opacity-75
          relative
          cursor-pointer
          h-80
          bg-cyan-200
          inline-block
        "
      >
        <img class="object-cover z-0 h-full w-full" :src="image" alt="" />
        <span
          class="
            absolute
            z-10
            top-0
            right-0
            font-bold
            text-3xl text-gray-300
            hover:text-gray-400
            mr-2
          "
          @click="removeFromGallery(image)"
          >&times;</span
        >
      </div>
      <img
        :src="galleryEmpty"
        class="mx-auto my-60"
        :class="{ hidden: !isImageListEmpty() }"
        alt=""
      />
    </div>
  </div>
</template>
