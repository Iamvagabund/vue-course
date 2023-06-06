
<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <div class="app__btns">
      <MyButton
        @click="showDialog"
      >
        Создать пост
      </MyButton>
      <MySelect 
        v-model="selectedSort"
      />
    </div>
    <!-- <input type="text" v-model.trim="modificatorValue" /> -->
    <MyDialog v-model:show="dialogVisible">
      <PostForm 
        @create="createPost"
      />
    </MyDialog>
    <PostList 
      :posts="posts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>Идет загрузка...</div>
  </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue'
import PostList from '@/components/PostList.vue'
import MySelect from '@/components/UI/MySelect.vue'
import axios from 'axios'
export default {
  components: {
    PostForm, PostList, MySelect
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      modificatorValue: '',
      isPostsLoading: false,
      selectedSort: ''
    }
  },
  methods: {
    createPost(post) {
      this.posts.push(post)
      this.dialogVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true
    },
    async fetchPosts() {
      try {
        this.isPostsLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
        this.posts = response.data
      } catch(e) {
        console.log(e)
      } finally {
        this.isPostsLoading = false
      }
    }
  },
  mounted() {
      this.fetchPosts();
    }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.app {
  padding: 20px;
}

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
</style>
