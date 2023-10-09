<template>
  <div>
    <input type="file" ref="fileInput" @change="handleFileChange" />
    <div v-if="image" class="image-container">
      <img :src="image.url" alt="Uploaded Image" />
      <button @click="removeImage" class="remove-button">x</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      image: null,
    };
  },
  methods: {
    handleFileChange() {
      const fileInput = this.$refs.fileInput;
      const file = fileInput.files[0];

      if (file) {
        const reader = new FileReader();

        reader.onload = (e) => {
          this.image = { url: e.target.result, file };
        };

        reader.readAsDataURL(file);
      } else {
        this.image = null; // Clear the image when no file is selected
      }

      fileInput.value = ''; // Clear the file input
    },
    removeImage() {
      this.image = null;
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
