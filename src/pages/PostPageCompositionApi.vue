
<template>
  <div>
    <h1>{{ likes }}</h1>
    <button @click="addLike">add likes</button>
    <h1>Страница с постами</h1>
    <MyInput 
      v-model="searchQuery"
      placeholder="Поиск..."
    />
    <div class="app__btns">
      <MyButton
      >
        Создать пост
      </MyButton>
      <MySelect 
        v-model="selectedSort"
        :options="sortOptions"
      />
    </div>
    <MyDialog v-model:show="dialogVisible">
      <PostForm 
      />
    </MyDialog>
    <PostList 
      :posts="sortedAndSearchedPosts"
      v-if="!isPostsLoading"
    />
    <div v-else>Идет загрузка...</div>
  </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue'
import PostList from '@/components/PostList.vue'
import MySelect from '@/components/UI/MySelect.vue'
import MyButton from '@/components/UI/MyButton.vue'
import MyInput from '@/components/UI/MyInput.vue'
import {ref} from 'vue'
import { usePosts } from "@/hooks/usePosts"
import { useSortedPosts } from "@/hooks/useSortedPosts"
import { useSortedAndSearchedPosts } from "@/hooks/useSortedAndSearchedPosts"

export default {
  components: {
    PostForm, PostList, MySelect
  },
  data() {
    return {
      dialogVisible: false,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По описанию'}
      ]
    }
  },
  setup(props) {
    const likes = ref(2)
    const {posts, totalPages, isPostsLoading} = usePosts(10)
    const {selectedSort, sortedPosts} = useSortedPosts(posts)
    const {searchQuery, sortedAndSearchedPosts} = useSortedAndSearchedPosts(sortedPosts)

    const addLike = () => {
      likes.value +=1
    }

    return {
      likes, addLike, posts, totalPages, isPostsLoading, selectedSort, sortedPosts, searchQuery, sortedAndSearchedPosts
    }
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
