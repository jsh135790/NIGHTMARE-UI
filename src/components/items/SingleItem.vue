<template>
  <div class="section-item">
    <div class="item-title">Crosshair</div>
    <SingleSmall :ref="refnote" :options="options"/>
  </div>
</template>

<script>
import SingleSmall from '../interactions/SingleSmall.vue';
export default {
  components: {
    SingleSmall
  },
  props: {
    options: {
      type: Array,
      required: true
    },
    refnote: {
      type: String,
      required: true
    }
  },
  methods: {
    handleClickOutside(event) {
      for (let ref in this.$refs) {
        if (!this.$refs[ref].$el.contains(event.target) && this.$refs[ref].closeDropdown) {
          console.log(this.$refs[ref].value);
          this.$refs[ref].closeDropdown();
        }
      }
    }
  }
}
</script>

<style scoped>
.section-item {
  width: 95%;
  padding: 7px 0;
  display: flex;
  align-items: center;
  border-top: #454f582f 1px solid;
}
</style>