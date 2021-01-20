<template>
    <div class="form-group mt-2 comment rounded p-2" v-show="showReplyInput">
      <label for="reply">Add your reply</label>
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
</template>

<script>
  import { mapGetters } from 'vuex'
  import { BFormInput } from 'bootstrap-vue'

  export default {
    name: 'NewReply',
    components: {
      BFormInput
    },
    props: ["showReplyInput"],
    computed: {
      ...mapGetters({
        users: 'users',
        currentUser: 'currentUser'
      }),
    },
    data() {
      return {
        content: '',
        commentUser: ''
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
            this.commentUser = user
            this.$store.dispatch('loadUser', user.id)
            this.content = ""
          }
        }); 
      },
      sendComment() {
        if (this.commentUser){
          const comment = {
            page: this.$route.params.page_id,
            parent: null,
            poster: this.commentUser.id,
            content: this.content,
          }
          this.$store.dispatch('postPageComments', comment)
          this.content = ""
        }
        else {
          alert("Please select a User")
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
