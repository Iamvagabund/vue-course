
<template>
  <div>
    <h1>{{ $store.state.isAuth ? 'Пользователь авторизован' : 'Авторизируйтесь, чтобы использовать сервис' }}</h1>
    <h1>{{ $store.state.likes }}</h1>
    <h1>{{ $store.getters.doubleLikes }}</h1>

    <h1>{{ $store.state.post.limit }}</h1>
  </div>
  <div>
    <div>
      <MyButton @click="$store.commit('incrementLikes')">
        Лайк
      </MyButton>
      <MyButton @click="$store.commit('decrementLikes')">
        Дизлайк
      </MyButton>
    </div>
    <h1>Страница с постами</h1>
    <!-- <MyInput 
      v-focus
      v-model="searchQuery"
      placeholder="Поиск..."
    /> -->
    <MyInput 
      v-focus
      :model-value="searchQuery"
      @update:model-value="setSearchQuery"
      placeholder="Поиск..."
    />
    <div class="app__btns">
      <MyButton
        @click="showDialog"
      >
        Создать пост
      </MyButton>
      <!-- <MySelect 
        v-model="selectedSort"
        :options="sortOptions"
      /> -->
      <MySelect 
        :model-value="selectedSort"
        @update:model-value="setSelectedSort"
        :options="sortOptions"
      />
    </div>
    <MyDialog v-model:show="dialogVisible">
      <PostForm 
        @create="createPost"
      />
    </MyDialog>
    <PostList 
      :posts="sortedAndSearchedPosts"
      @remove="removePost"
      v-if="!isPostsLoading"
    />
    <div v-else>Идет загрузка...</div>
    <div v-intersection="loadMorePosts" class="observer"></div>
  </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue'
import PostList from '@/components/PostList.vue'
import MySelect from '@/components/UI/MySelect.vue'
import {mapState, mapGetters, mapMutations, mapActions} from 'vuex'
import axios from 'axios'
export default {
  components: {
    PostForm, PostList, MySelect
  },
  data() {
    return {
      dialogVisible: false,
      // posts: [],
      // modificatorValue: '',
      // isPostsLoading: false,
      // selectedSort: '',
      // searchQuery: '',
      // page: 1,
      // limit: 10,
      // totalPages: 0,
      // sortOptions: [
      //   {value: 'title', name: 'По названию'},
      //   {value: 'body', name: 'По описанию'}
      // ]
    }
  },
  methods: {
    ...mapMutations({
      setPage: 'post/setPage',
      setSearchQuery: 'post/setSearchQuery',
      setSelectedSort: 'post/setSelectedSort'
    }),
    // ...mapMutations('post', ['setPage']),
    
    ...mapActions({
      loadMorePosts: 'post/loadMorePosts',
      fetchPosts: 'post/fetchPosts'
    }),
    // ...mapActions('post', ['loadMorePosts', 'fetchPosts']),
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
    // changePage(pageNumber) {
    //   this.page = pageNumber
    // },
    // async fetchPosts() {
    //   try {
    //     this.isPostsLoading = true
    //     const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
    //       params: {
    //         _page: this.page,
    //         _limit: this.limit
    //       }
    //     })
    //     this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
    //     this.posts = response.data
    //   } catch(e) {
    //     console.log(e)
    //   } finally {
    //     this.isPostsLoading = false
    //   }
    // },
    // async loadMorePosts() {
    //   try {
    //     this.page += 1;
    //     const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
    //       params: {
    //         _page: this.page,
    //         _limit: this.limit
    //       }
    //     })
    //     this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit)
    //     this.posts = [...this.posts, ...response.data]
    //   } catch(e) {
    //     console.log(e)
    //   } finally {

    //   }
    // }
  },
  mounted() {
    this.fetchPosts();

    // let options = {
    //   rootMargin: "0px",
    //   threshold: 1.0,
    // };
    // let callback = (entries, observer) => {
    //   if(entries[0].isIntersecting && this.page < this.totalPages) {
    //     this.loadMorePosts()
    //   }
    // }
    // let observer = new IntersectionObserver(callback, options);
    // observer.observe(this.$refs.observer)

  },
  computed: {
    ...mapState({
      posts: state => state.post.posts,
      isPostsLoading: state => state.post.isPostsLoading,
      selectedSort: state => state.post.selectedSort,
      searchQuery: state => state.post.searchQuery,
      page: state => state.post.page,
      limit: state => state.post.limit,
      totalPages: state => state.post.totalPages,
      sortOptions: state => state.post.sortOptions,
    }),
    ...mapGetters({
      sortedPosts: 'post/sortedPosts',
      sortedAndSearchedPosts: 'post/sortedAndSearchedPosts'
    }),
    // sortedPosts() {
    //   return [...this.posts].sort((post1, post2) => {
    //     return post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
    //   })
    // },
    // sortedAndSearchedPosts() {
    //   return this.sortedPosts.filter(post => post.title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    // }
  },
  watch: {
    // page() {
    //   this.fetchPosts()
    // }
  }
}
</script>

<style>

.app__btns {
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}

.page__wrapper {
  display: flex;
  margin-top: 15px;
}

.page {
  border: 1px solid black;
  padding: 10px;
}

.current-page {
  border: 2px solid teal;
}

.observer {
  height: 30px;
  background: green;
}
</style>
