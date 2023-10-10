<template>
  <div class="navbar_page">
    <div class="container">
      <div class="row align-items-baseline custom-center">


      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "navbarPage",

  data() {
    return {
      logoImage: '',
      categories: [],
      avatar: ''
    }
  },

  created() {

  },

  mounted() {
    this.getUser();
    this.getData();
    this.getCategories();

  },


  methods: {

    // get user data from api

    async getUser() {
      try {
        return await this.$axios.get(`profile`).then(response => {
          this.avatar = response.data.data.avatar;

        }).catch(error => {
          console.log(error)
        })
      } catch (error) {
        console.log("catch : " + error)
      }
    },

    // get courses category data from api

    async getCategories() {
      try {
        return await this.$axios.get(`setting/categories`).then(response => {

          this.loading = true;

          // console.log(response.data.data.category_course)

          this.categories = response.data.data.category_course;

        }).catch(error => {
          console.log(error)
        })
      } catch (error) {
        console.log("catch : " + error)
      }
    },

    // get navbar data

    async getData() {
      try {
        return await this.$axios.get(`setting/layout`).then(response => {
          this.logoImage = response.data.data.logo;
          // console.log(response.data.data.log)
        }).catch(error => {
          console.log(error)
        })
      } catch (error) {
        console.log("catch : " + error)
      }
    },

    // handle logout

    handleLogOut() {

      this.$auth.logout();

      this.$cookies.remove('auth._token.local')
      // for set user from api
      window.localStorage.removeItem('vuex');

      this.$router.push(this.localePath({ path: "/" }));

    },

    onOver() {
      this.$refs.dropdown.visible = true;
    },
    onLeave() {
      this.$refs.dropdown.visible = false;
    },
    onOver2() {
      this.$refs.dropdownLogin.visible = true;
    },
    onLeave2() {
      this.$refs.dropdownLogin.visible = false;
    },

    whenUpload() {
      this.$swal.fire({
        position: 'center',
        type: 'warning',
        title: 'بيتم تنفيذها علي الدومين الرسمي',
        showConfirmButton: false,
        timer: 3000
      });
    }

  }
}
</script>

<style lang="scss" scoped>
.dropdown-menu {
  transition: 0.5s;
}
</style>
