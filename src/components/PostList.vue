<template>
  <div v-show="posts.length > 0">
    <h3>Список пользователей</h3>
    <PostItem v-for="post in posts" :post="post" :key="post.id" @remove="$emit('remove', post)" />
  </div>
  <h2 v-show="posts.length === 0">Список пуст</h2>
</template>
<script>
import PostItem from '@/components/PostItem.vue';
export default {
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
  components: { PostItem },
};
</script>
<style>
h2 {
  text-align: center;
  color: red;
}
</style>
