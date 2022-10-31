<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <MyInput v-model="searchQuery" placeholder="...Поиск"></MyInput>
    <div class="app__btns">
      <MyButton @click="showDialog" style="margin: 15px 5px"> Создать пост</MyButton>
      <MySelect :options="sortOptions" v-model="selectedSort"></MySelect>
    </div>

    <MyDialog v-model:show="dialogVisible"> <PostForm @create="createPost" /></MyDialog>

    <PostList :posts="sortedAndSearchedPosts" @remove="removePost" v-if="!isPostLoading" />
    <div v-else>Идет загрузка...</div>
    <!-- <div class="page__wrapper">
      <div
        :class="{
          'current-page': page === pageNumber,
        }"
        class="page"
        :key="pageNumber"
        v-for="pageNumber in totalPages"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div>
    </div> -->
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
      modificatorValue: '',
      dialogVisible: false,
      isPostLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      totalPages: 0,
      limit: 10,
      sortOptions: [
        { value: 'title', name: 'По названию' },
        { value: 'body', name: 'По содержимому' },
      ],
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
    // changePage(pageNumber) {
    //   this.page = pageNumber;
    //   this.fetchPosts();
    // },
    async fetchPosts() {
      try {
        this.isPostLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          },
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = response.data;
        console.log(response);
      } catch (e) {
        alert('Ошибка');
      } finally {
        this.isPostLoading = false;
      }
    },
    async loadMorePosts() {
      try {
        this.isPostLoading = true;
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts', {
          params: {
            _page: this.page,
            _limit: this.limit,
          },
        });
        this.totalPages = Math.ceil(response.headers['x-total-count'] / this.limit);
        this.posts = [this.posts, ...response.data];
        console.log(response);
      } catch (e) {
        alert('Ошибка');
      } finally {
        this.isPostLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) =>
        post1[this.selectedSort]?.localeCompare(post2[this.selectedSort])
      );
    },
    sortedAndSearchedPosts() {
      return this.sortedPosts.filter((post) =>
        post.title.toLowerCase().includes(this.searchQuery.toLowerCase())
      );
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
  cursor: pointer;
}
.current-page {
  border: 2px solid teal;
}
</style>
