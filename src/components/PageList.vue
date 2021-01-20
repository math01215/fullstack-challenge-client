<template>
  <ul class="list-group">
    <!-- TODO: display the links to pages from the 'pages' array -->
    <li 
      class="list-group-item" 
      v-for="page in pages" 
      :key="page.id"
    >
      <router-link 
        :to="{name: 'page-details', 
        params: {page_id: page.id}}"
      >
        Page {{page.id}}
      </router-link>
    </li>
  </ul>
</template>

<script>
  export default {
    name: 'PageList',
    props: {
      pages: {
        type: Array
      }
    }
  }
</script>

<style lang="scss" scoped>
</style>
