<template>
  <div class="left-text">
    <h3>{{ labelText }}</h3>
  </div>
</template>

<script>
export default {
  props: {
    labelText: String
  }
}
</script>

<style>
.left-text {
  margin-top: 20px;
  margin-left: 5px;
  text-align: left;
  color: #778899;
  font-size: 10px;
}
.left-text h3 {
  font-weight: lighter;
}
</style>