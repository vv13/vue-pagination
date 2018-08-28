<template>
  <div class="vue_pagination">
    <div class="vue_pagination_prev vue_pagination_item" @click="goPrev">&lt;</div>
    <ul class="vue_pagination">
      <li class="vue_pagination_item" @click="go(1)" :class="{_selected: currentPage === 1}" v-if="total > 0">1</li>
      <li v-if="isLoadLeftMore" class="vue_pagination_item" @click="go(currentPage - 3)">...</li>
      <li :key="i" v-for="i in pagers" class="vue_pagination_item" @click="go(i)" :class="{_selected: currentPage === i }">{{i}}</li>
      <li v-if="isLoadRightMore" class="vue_pagination_item" @click="go(currentPage + 3)">...</li>
      <li class="vue_pagination_item" @click="go(pageCount)" :class="{_selected: currentPage === pageCount}" v-if="pageCount > 1">{{pageCount}}</li>
    </ul>
    <div class="vue_pagination_next vue_pagination_item" @click="goNext">&gt;</div>
  </div>
</template>

<script>
export default {
  name: "VuePagination",

  data() {
    return {
      currentPage: 1,
    }
  },
  props: {
    total: {
      type: Number,
      default: 0,
    },
    pageSize: {
      type: Number,
      default: 10
    },
    page: {
      type: Number,
      default: 1
    },
  },

  computed: {
    pageCount() {
      return Math.ceil(this.total / this.pageSize);
    },
    isLoadLeftMore() {
      return this.pageCount > 7 && this.currentPage > 4;
    },
    isLoadRightMore() {
      return this.pageCount > 7 && this.currentPage + 3 < this.pageCount;
    },
    centerNumber() {
      if (!this.isLoadLeftMore) return 4;
      if (!this.isLoadRightMore) return this.pageCount - 3;
      return this.currentPage;
    },
    pagers() {
      let array = [];
      const pagerCount = 6;

      if (this.isLoadLeftMore && !this.isLoadRightMore) {
        const startPage = this.pageCount - (pagerCount - 2);
        for (let i = startPage; i < this.pageCount; i += 1) {
          array.push(i);
        }
      } else if (!this.isLoadLeftMore && this.isLoadRightMore) {
        for (let i = 2; i < pagerCount; i += 1) {
          array.push(i);
        }
      } else if (this.isLoadLeftMore && this.isLoadRightMore) {
        array = [
          this.centerNumber - 1,
          this.centerNumber,
          this.centerNumber + 1
        ];
      } else {
        for (let i = 2; i < this.pageCount; i += 1) {
          array.push(i);
        }
      }
      return array;
    }
  },

  methods: {
    goPrev() {
      if (this.currentPage === 1 || !this.total) {
        return;
      }
      this.go(this.currentPage - 1);
    },
    goNext() {
      if (this.currentPage === this.pageCount || !this.total) {
        return;
      }
      this.go(this.currentPage + 1);
    },
    go(currentPage) {
      this.currentPage = currentPage;
      this.$emit("update:page", currentPage);
      this.$emit("pageChange", currentPage);
    }
  },

  created() {
    this.currentPage = this.page
  },

  watch: {
    page(val) {
      this.currentPage = this.page
    },
  }
};
</script>

<style>
.vue_pagination {
  padding: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

.vue_pagination_item {
  width: 34px;
  height: 34px;
  line-height: 34px;
  text-align: center;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin: 0 5px;
  background: #fff;
  color: #666;
  cursor: pointer;
  list-style: none;
}

.vue_pagination_item._selected {
  background: #00ca97;
  color: #fff;
}
</style>
