<template>
  <ul class="uk-pagination uk-flex-center" uk-margin>
    <li v-show="isPreviousButtonDisabled">
      <a @click="previousPage">
        <span uk-pagination-previous></span>
      </a>
    </li>

    <BasePaginationTrigger
      v-for="paginationTrigger in paginationTriggers"
      :key="paginationTrigger"
      :class="{
        'uk-active':
          paginationTrigger === currentPage
      }"
      :pageNumber="paginationTrigger"
      @loadPage="onLoadPage"
    ></BasePaginationTrigger>

    <li v-show="isNextButtonDisabled">
      <a @click="nextPage">
        <span uk-pagination-next></span>
      </a>
    </li>
  </ul>
</template>

<script>
import BasePaginationTrigger from './BasePaginationTrigger';

const _ = require('lodash');

export default {
  props: {
    visiblePagesCount: {
      type: Number,
      default: 5,
    },
    currentPage: {
      type: Number,
      required: true,
    },
    pageCount: {
      type: Number,
      required: true,
    },
  },
  components: {
    BasePaginationTrigger,
  },
  computed: {
    isPreviousButtonDisabled() {
      return !(this.currentPage === 1);
    },
    isNextButtonDisabled() {
      return !(this.currentPage === this.pageCount);
    },
    paginationTriggers() {
      if (this.pageCount >= this.visiblePagesCount) {
        const { currentPage } = this;
        const { pageCount } = this;
        const { visiblePagesCount } = this;
        const visiblePagesThreshold = (visiblePagesCount - 1) / 2;
        const pagintationTriggersArray = Array(this.visiblePagesCount - 1).fill(
          0,
        );

        if (currentPage <= visiblePagesThreshold + 1) {
          pagintationTriggersArray[0] = 1;
          const pagintationTriggers = pagintationTriggersArray.map(
            (paginationTrigger, index) => pagintationTriggersArray[0] + index,
          );

          pagintationTriggers.push(pageCount);

          return pagintationTriggers;
        }

        if (currentPage >= pageCount - visiblePagesThreshold + 1) {
          const pagintationTriggers = pagintationTriggersArray.map(
            (paginationTrigger, index) => pageCount - index,
          );

          pagintationTriggers.reverse().unshift(1);

          return pagintationTriggers;
        }


        pagintationTriggersArray[0] = currentPage - visiblePagesThreshold + 1;
        const pagintationTriggers = pagintationTriggersArray.map(
          (paginationTrigger, index) => pagintationTriggersArray[0] + index,
        );

        pagintationTriggers.unshift(1);
        pagintationTriggers[pagintationTriggers.length - 1] = pageCount;

        return pagintationTriggers;
      } return ([...Array(this.pageCount).keys()].map(x => x += 1));
    },
  },
  methods: {
    nextPage() {
      this.$emit('nextPage');
    },
    previousPage() {
      this.$emit('previousPage');
    },
    onLoadPage(value) {
      this.$emit('loadPage', value);
    },
  },
};
</script>


<style lang="scss" scoped>
</style>
