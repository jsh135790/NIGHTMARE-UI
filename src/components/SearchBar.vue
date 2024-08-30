<template>
  <div class="search-icon" @click="IsActive = !IsActive">
    <SearchIcon />
    <input type="text" class="search-input" :class="{ active: IsActive }" placeholder="搜索 ..." />
  </div>
</template>

<script>
import SearchIcon from './icons/SearchIcon.vue';
export default {
  components: {
    SearchIcon
  },
  data() {
    return {
      IsActive: false
    }
  }
}
</script>

<style>
.search-icon{
  display: flex;
  align-items: center;
  margin-left: 5px;
  padding-top: 7px;
  padding-bottom: 10px;
  cursor: pointer;
}
.search-input {
  background-color: transparent;
  border: none;
  margin: 10px 50px;
  width: 0;
  outline: none;
  color: rgb(100, 100, 100);
  transition: width 1s;
  transition-delay: 0.5s;
  font-weight: bold;
}
.search-input.active {
  width: 100px;
}
</style>