<template>
  <div class="page-details container mt-4">
    <div class="row">
    <div class="col-12">
      <h1 v-if="currentPage">{{ currentPage.title }}</h1>
      <p v-if="currentPage">{{ currentPage.description }}</p>
      <hr/>
      <div class="d-flex flex-column">
        <Comment
          class="mt-2"
          v-for="comment in currentPageComments"
          :comment="comment"
          :key="comment.id"
        />
        <NewComment :showReplyInput="true" />
      </div>
    </div>
    </div>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import moment from 'moment'
  import Comment from '@/components/Comment'
  import NewComment from '@/components/NewComment'

  export default {
    name: 'PageDetails',
    components: {
      Comment,
      NewComment
    },
    computed: {
      ...mapGetters({
        currentPageComments: 'currentPageComments',
        currentPage: 'currentPage'
      })
    },
    data() {
      return {
        sampleComments: [
          {
            id: 1,
            owner: {
              first_name: 'John',
              last_name: 'Doe'
            },
            content: 'This is a simple comment',
            posted: moment().format('lll'),
            replies: [
              {
                id: 2,
                owner: {
                  first_name: 'John',
                  last_name: 'Clark'
                },
                content: 'This is a simple reply',
                posted: moment().add(1, 'hours').format('lll'),
              },
              {
                id: 3,
                owner: {
                  first_name: 'Joanna',
                  last_name: 'Clarkson'
                },
                content: 'Hey <a href="/user/249dab0a-e859-43f9-916e-92e14b54cb96">John Doe</a> nice to meet you',
                posted: moment().add(1, 'days').format('lll'),
              }
            ]
          },
          {
            id: 10,
            owner: {
              first_name: 'Jo',
              last_name: 'Knicks'
            },
            content: 'This is a great comment',
            posted: moment().format('lll'),
            replies: [
              {
                id: 12,
                owner: {
                  first_name: 'Sammy',
                  last_name: 'Oreiley'
                },
                content: 'Really unbelievable!',
                posted: moment().add(3, 'hours').format('lll'),
              },
              {
                id: 13,
                owner: {
                  first_name: 'Ally',
                  last_name: 'Samsom'
                },
                content: 'Hey Sammy, did you see the comment of <a href="/user/249dab0a-e859-43f9-916e-92e14b54cb96">John Doe</a>?',
                posted: moment().add(2, 'days').format('lll'),
              }
            ]
          },
          {
            id: 20,
            owner: {
              first_name: 'Anna',
              last_name: 'Richardson'
            },
            content: 'Well done!',
            posted: moment().format('lll'),
            replies: []
          },
        ]
      }
    },
    mounted() {
      this.$store.dispatch('loadPage', this.$route.params.page_id)
      this.$store.dispatch('loadPageComments', this.$route.params.page_id)
    }
  }
</script>

<style scoped>

</style>
