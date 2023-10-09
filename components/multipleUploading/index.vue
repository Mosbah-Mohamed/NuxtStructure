<template>
  <div>
    <input type="file" ref="fileInput" @change="handleFileChange" :multiple="multiple" />
    <div v-for="(image, index) in images" :key="index" class="image-container">
      <img :src="image.url" alt="Uploaded Image" />
      <button @click="removeImage(index)" class="remove-button">x</button>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    multiple: Boolean, // To allow multiple file uploads
  },
  data() {
    return {
      images: [],
    };
  },
  methods: {
    handleFileChange() {
      const fileInput = this.$refs.fileInput;
      const files = fileInput.files;

      for (let i = 0; i < files.length; i++) {
        const file = files[i];
        const reader = new FileReader();

        reader.onload = (e) => {
          this.images.push({ url: e.target.result, file });
        };

        reader.readAsDataURL(file);
      }

      fileInput.value = ''; // Clear the file input
    },
    removeImage(index) {
      this.images.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.image-container {
  position: relative;
  display: inline-block;
  margin: 5px;
}

.remove-button {
  position: absolute;
  top: 0;
  right: 0;
  background-color: red;
  color: white;
  border: none;
  border-radius: 50%;
  padding: 5px;
  cursor: pointer;
}
</style>
