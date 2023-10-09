<template>
  <main>
    <!-- <LazyHomeHero />
    <LazyHomeAboutAzzm />
    <LazyHomeCourses />
    <LazyHomeWhyUs />
    <LazyHomeArticles /> -->

    <button @click="showToast">Show Toast</button>

    <form @submit="submitImages">
      <LazyMultipleUploading :multiple="false" ref="imageUploader" />
      <button type="submit">Submit</button>
    </form>

    <form @submit="submitImages">
      <LazySingleUpload :multiple="false" ref="imageUploader" />
      <button type="submit">Submit</button>
    </form>

    <div class="chat_icon flex-center" @click="whenUpload">
      <img data-src="@/assets/images/chat.svg" title="chat" v-lazy-load alt="chat image" width="100%" height="100%" />
    </div>

    <button class="scroll-to-top up" v-show="showButton" @click="scrollToTop">
      <font-awesome-icon :icon="['fas', 'plane-up']" />
    </button>
  </main>
</template>

<script>


export default {

  name: "indexPage",

  head() {
    return {
      title: "Home",
    }
  },

  // favicon

  // head() {
  //   return {
  //     link: [
  //       {
  //         rel: 'icon',
  //         type: 'image/x-icon',
  //         href: this.faviconUrl
  //       }
  //     ]
  //   }
  // },

  // async asyncData({ $axios }) {
  //   const { data } = await $axios.get('https://your-api.com/favicon')
  //   return { faviconUrl: data.url }
  // },


  data() {
    return {
      showButton: false,

      uploadedImages: [],
      uploadedImage: null
    }
  },



  async asyncData({ $axios, app, route }) {
  },




  computed: {
  },
  //  when component load

  mounted() {

    window.scrollTo(0, 0);
    this.$nextTick(() => {
      window.scrollTo(0, 0);
    });

    window.addEventListener("scroll", this.handleScroll);
  },

  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
  },


  // All methods and logic

  methods: {
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: "smooth"
      });
    },
    handleScroll() {
      this.showButton = window.pageYOffset > 300;
    },

    whenUpload() {
      this.$swal.fire({
        position: 'center',
        type: 'warning',
        title: 'بيتم تنفيذها علي الدومين الرسمي',
        showConfirmButton: false,
        timer: 3000
      });
    },

    showToast() {
      this.$iziToast.show({
        title: 'warning',
        class: 'warning',
        timeout: 50000,
        message: 'This is an iziToast notification!',
        position: 'topRight',
      })

      this.$iziToast.info({
        title: 'Hello',
        message: 'Welcome!',
      });

      this.$iziToast.success({
        title: 'OK',
        message: 'Successfully inserted record!',
      });

      this.$iziToast.warning({
        title: 'Caution',
        message: 'You forgot important data',
      });

      this.$iziToast.error({
        title: 'Error',
        message: 'Illegal operation',
      });

      this.$iziToast.question({
        timeout: 20000,
        close: false,
        overlay: true,
        displayMode: 'once',
        id: 'question',
        zindex: 999,
        title: 'Hey',
        message: 'Are you sure about that?',
        position: 'center',
        buttons: [
          ['<button><b>YES</b></button>', function (instance, toast) {

            instance.hide({ transitionOut: 'fadeOut' }, toast, 'button');

          }, true],
          ['<button>NO</button>', function (instance, toast) {

            instance.hide({ transitionOut: 'fadeOut' }, toast, 'button');

          }],
        ],
        onClosing: function (instance, toast, closedBy) {
          console.info('Closing | closedBy: ' + closedBy);
        },
        onClosed: function (instance, toast, closedBy) {
          console.info('Closed | closedBy: ' + closedBy);
        }
      });


    },

    async submitImages() {
      try {
        const formData = new FormData();
        const uploadedImages = this.$refs.imageUploader.images;

        for (let i = 0; i < uploadedImages.length; i++) {
          const image = uploadedImages[i];
          formData.append('images[]', image.file);
        }

        await this.$axios.$post('register', formData).then(response => {

        }).catch(error => {
          console.log('fail' + error.response.data.msg)

          this.$swal.fire({
            type: 'error',
            text: `${error.response.data.msg}`,
            timer: 2000,
            // confirmButtonColor: '#ff7400',
          })

        })

      } catch (error) {
        console.error(error)
      }
    },


  }
}
</script>

<style lang="scss" scoped>
.chat_icon {
  position: fixed;
  bottom: 30px;
  right: 30px;
  border-radius: 50px;
  width: 74px;
  height: 74px;
  background: #FFFFFF;
  box-shadow: 0px 3px 6px rgb(189 204 236 / 45%);
  cursor: pointer;
  z-index: 999;

  img {
    width: 33px;
    height: 33px
  }
}
</style>
