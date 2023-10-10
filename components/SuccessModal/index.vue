<template>
  <b-modal v-model="isOpen" @hidden="onModalHidden" @click.native="closeModalOnOutsideClick"
    :no-close-on-backdrop="false">
    <slot></slot>
  </b-modal>
</template>

<script>
export default {
  data() {
    return {
      isOpen: false
    };
  },
  methods: {
    onModalHidden() {
      // Emit an event when the modal is closed
      this.$emit('modal-closed');
    }
  }
};
</script>
