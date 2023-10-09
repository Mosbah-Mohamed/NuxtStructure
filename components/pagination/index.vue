<template>
  <div>
    <button @click="previousPage" :disabled="currentPageCopy === 1">Previous</button>
    <span v-for="(pageNumber, index) in pageNumbers" :key="index"
      :class="{ active: currentPageCopy === pageNumber, ellipsis: pageNumber === '...' }"
      @click="changePage(pageNumber)">{{
        pageNumber }}</span>
    <button @click="nextPage" :disabled="currentPageCopy === totalPages">Next</button>
  </div>
</template>

<script>
export default {
  props: {
    totalPages: {
      type: Number,
      required: true
    },
    currentPage: {
      type: Number,
      required: true
    },
    pagesToShow: {
      type: Number,
      required: true
    },
    onPageChange: {
      type: Function,
      required: true
    }
  },
  data() {
    return {
      currentPageCopy: this.currentPage
    }
  },
  watch: {
    currentPage(newValue) {
      this.currentPageCopy = newValue;
    }
  },
  methods: {
    previousPage() {
      if (this.currentPageCopy > 1) {
        this.currentPageCopy--;
        this.onPageChange(this.currentPageCopy); // Emit the updated current page value
      }
    },
    nextPage() {
      if (this.currentPageCopy < this.totalPages) {
        this.currentPageCopy++;
        this.onPageChange(this.currentPageCopy); // Emit the updated current page value
      }
    },
    changePage(pageNumber) {
      if (pageNumber === '...') {
        return;
      }
      this.currentPageCopy = pageNumber;
      this.onPageChange(this.currentPageCopy); // Emit the updated current page value
    },
    // ...
  },
  computed: {
    pageNumbers() {
      let start, end;
      const pagesToShow = this.pagesToShow;
      const pageNumbers = [];

      if (this.totalPages <= pagesToShow) {
        // Show all pages
        start = 1;
        end = this.totalPages;
      } else {
        // Show a subset of pages, with ellipses on either end
        start = Math.max(this.currentPageCopy - Math.floor(pagesToShow / 2), 1);
        end = start + pagesToShow - 1;

        if (end > this.totalPages) {
          end = this.totalPages;
          start = end - pagesToShow + 1;
        }

      }

      pageNumbers.push(...Array.from({ length: end - start + 1 }, (_, i) => start + i));
      return pageNumbers;
    }
  }
};
</script>

<style>
.active {
  font-weight: bold;
}

.ellipsis {
  margin: 0 5px;
}
</style>
