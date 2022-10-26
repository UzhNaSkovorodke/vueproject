<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <MyButton @click="fetchPosts">Получить посты</MyButton>

    <MyDialog v-model:show="dialogVisible"> <PostForm @create="createPost" /></MyDialog>

    <PostList :posts="posts" @remove="removePost" />
  </div>
</template>

<script>
import PostForm from '@/components/PostForm.vue';
import PostList from '@/components/PostList.vue';
import MyDialog from './components/UI/MyDialog.vue';
import axios from 'axios';
export default {
  components: {
    PostList,
    PostForm,
  },
  data() {
    return {
      posts: [],
      dialogVisible: false,
      modificatorValue: '',
    };
  },
  methods: {
    createPost(post) {
      this.posts.push(post);
      this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter((p) => p.id !== post.id);
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
      try {
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts/?_limit=10');
        this.posts = response.data;
        console.log(response);
      } catch (e) {
        alert('Ошибка');
      }
    },
  },
  components: { PostForm, PostList, MyDialog },
};
</script>
<style>
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
.app {
  padding: 20px;
}
</style>
