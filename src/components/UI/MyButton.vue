<template>
  <button class="btn">
    <slot></slot>
  </button>
</template>
<script>
export default {
  name: 'MyButton',
};
</script>
<style>
.btn {
  margin-top: 15px;
  align-self: flex-end;
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
  border-radius: 5px;
}
.btn:active {
  background: rgb(166, 218, 218);
}
</style>
