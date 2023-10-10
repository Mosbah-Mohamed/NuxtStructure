<template>
  <div>
    <CoolLightBox :items="images" :index="index" @close="index = null">
    </CoolLightBox>

    <div class="images-wrapper">
      <div class="image" v-for="(image, imageIndex) in images" :key="imageIndex" :effect="'fade'" :useZoomBar="true"
        @click="index = imageIndex" :style="{ backgroundImage: `url(${image})` }"></div>
    </div>

    <button @click="openModal">Open Modal</button>
    <LazySuccessModal ref="modal">
      <h2>Modal Content</h2>
      <!-- Add your modal content here -->
    </LazySuccessModal>

  </div>
</template>

<script>
import CoolLightBox from "vue-cool-lightbox";
import "vue-cool-lightbox/dist/vue-cool-lightbox.min.css";

export default {
  components: {
    CoolLightBox,
  },
  data() {
    return {
      images: [
        "https://cdn.pixabay.com/photo/2015/04/23/22/00/tree-736885__340.jpg",
        "https://static.toiimg.com/photo/72975551.cms",
      ],
      index: null,

    };
  },

  methods: {
    openModal() {
      this.$refs.modal.isOpen = true;
    }
  }

};
</script>

<style>
.image {
  width: 300px;
  height: 300px;
  margin: 0 auto;
}
</style>
