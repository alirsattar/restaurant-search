<template>
  <div id="SearchResultListPaginator" class="row mx-auto">
    <nav class="col-12">
      <ul class="pagination justify-content-center">
        <template
          v-for="page in this.$data.pages">
            <li
              class="page-item info page-link"
              v-bind:class="{active: isCurrentPage(page)}"
              v-bind:key="page">
                <a @click="goToPage(page)">{{ page }}</a>
            </li>    
        </template>
      </ul>
    </nav>
  </div>
</template>

<script>
  export default {
    name: "SearchResultListPaginator",
    props: {
      totalResults: Number,
      perPage:      Number,
      currentPage:  Number
    },
    data: ()=> {
      return {
        pages: []
      }
    },
    methods: {
      setNumberOfPages: function() {
        const pages = Math.ceil(this.$props.totalResults / this.$props.perPage);

        for(let i = 0; i < pages; i++) {
          const pageNumWithinRange = this.pageNumWithinRange(i, this.$props.currentPage - 5, this.$props.currentPage + 3);

          if (pageNumWithinRange) {
            this.$data.pages.push(i+1);
          }
        }
      },
      isCurrentPage: function(page) {
        return this.$props.currentPage === page;
      },
      goToPage: function(pageNum) {
        this.$emit('goToPage', pageNum);
      },
      pageNumWithinRange: function(pageNum, min, max) {
        return pageNum >= min && pageNum <= max;
      }
    },
    mounted: function() {
      this.setNumberOfPages();
    },
    watch: {
      totalResults: function() {
        this.$data.pages = [];
        this.setNumberOfPages();
      },
      currentPage: function() {
        this.$data.pages = [];
        this.setNumberOfPages();
      }
    }
  };
</script>

<style scoped>
  #SearchResultListPaginator {
    padding: 10px;
    margin: 0px 10px 10px 10px;
  }

  .page-link:hover {
    cursor: pointer;
  }
</style>