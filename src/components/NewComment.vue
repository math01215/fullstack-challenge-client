<template>
  <div class="new-comment text-left">
    <div class="form-group mt-2 comment rounded p-2" v-show="showReplyInput">
      <label for="comment" v-if="comment">Add your reply</label>
      <label for="comment" v-else>Add Comment</label>
      <b-form-input
        ref="replyInput" 
        list="my-list-id"
        v-model="content"
        @input="changeSelectedItem"
        @keyup.enter.prevent="sendComment"
        placeholder="Your component with mentions support comes here..."
      ></b-form-input>
      <div v-if="content[0] === '@'">
        <datalist id="my-list-id">
          <option>Manual Option</option>
          <option v-for="user in users" :key="user.id">{{ '@' + user.first_name + ' ' + user.last_name }}</option>
        </datalist>
      </div>
    </div>
  </div>
</template>

<script>
  import { mapGetters } from 'vuex'
  import { BFormInput } from 'bootstrap-vue'

  export default {
    name: 'NewComment',
    components: {
      BFormInput
    },
    props: ["comment", "showReplyInput"],
    computed: {
      ...mapGetters({
        users: 'users',
        currentUser: 'currentUser'
      }),
    },
    data() {
      return {
        content: ''
      }
    },
    watch: {
      showReplyInput(v) {
        if( v ) {
          const el = this.$refs.replyInput.$el;
          if (el) { 
            this.$nextTick( () => {
              el.scrollIntoView({behavior: "smooth", block: "end"})
              el.focus()
            });
          }
        }
      }
    },

    methods: {
      changeSelectedItem() {
        this.users.forEach(user => {
          const username = "@" + user.first_name + " " + user.last_name
          if(this.content.toLowerCase() == username.toLowerCase()){
            console.log('item selected')
            this.$store.dispatch('loadUser', user.id)
            this.content = ""
          }
        }); 
      },
      sendComment() {
        if (this.currentUser){
          const comment = {
            page: this.$route.params.page_id,
            parent: this.comment?this.comment.id:null,
            poster: this.currentUser.id,
            content: this.content,
          }
          this.$store.dispatch('postPageComments', comment)
          this.content = ""
        }
        else {
          alert("Please select a User by clicking @")
        }
      }
    }
  }
</script>

<style lang="scss" scoped>

  .comment {
    background-color: azure;
  }

</style>
