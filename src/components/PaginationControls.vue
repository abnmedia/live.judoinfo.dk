<template>
  <ul v-if="pageCount > 1" class="pagination">
    <li v-if="page > 0" class="pagination-previous"><a @click.prevent="changePage(page-1)" href="#">&lt;</a></li>
    <li v-if="page > 2" class="pagination-number"><a @click.prevent="changePage(0)" href="#">1</a></li>
    <li v-if="page > 3" class="pagination-dots">...</li>

    <li v-if="page > 1" class="pagination-number"><a @click.prevent="changePage(page-2)" href="#">{{ page - 1 }}</a></li>
    <li v-if="page > 0" class="pagination-number"><a @click.prevent="changePage(page-1)" href="#">{{ page }}</a></li>
    <li class="pagination-current">{{ page + 1 }} </li>
    <li v-if="page < pageCount - 1" class="pagination-number"><a @click.prevent="changePage(page+1)" href="#">{{ page + 2}}</a></li>
    <li v-if="page < pageCount - 2" class="pagination-number"><a @click.prevent="changePage(page+2)" href="#">{{ page + 3}}</a></li>

    <li v-if="page < pageCount - 4" class="pagination-dots">...</li>
    <li v-if="page < pageCount - 3" class="pagination-number"><a @click.prevent="changePage(pageCount-1)" href="#">{{ pageCount }}</a></li>
    <li v-if="page < pageCount - 1" class="pagination-next"><a @click.prevent="changePage(page+1)" href="#">&gt;</a></li>
  </ul>
</template>
<script>

export default {
  name: 'PaginationControls',
  props: {
    page: Number,
    pageCount: Number,
  },
  emits: ['update:page'],
  methods: {
    changePage(page) {
      this.$emit('update:page', page);
    },
  }
}
</script>

<style scoped lang="scss">
  @import "../styles/constants.module.scss";

  $long-distance: 1.4em;
  $short-distance: 1.0em;

  .pagination {
    list-style-type: none;
    text-align: right;
    margin: 20px 0;
  }

  .pagination li {
    display: inline;
  }

  .pagination a, .pagination-dots {
    color: $color4;
  }

  .pagination-current {
    color: $color1;
    font-weight: 300;
  }

  .pagination a:hover {
    text-decoration: none;
    color: $color1;
  }

  .pagination-previous a {
    padding-right: $long-distance;
  }

  .pagination-number a , .pagination-current a {
    padding-left: $short-distance;
    padding-right: $short-distance;
  }

  .pagination-dots {
    padding-left: $long-distance;
    padding-right: $long-distance;
  }

  .pagination-next a {
    padding-left: $long-distance;
  }
</style>

