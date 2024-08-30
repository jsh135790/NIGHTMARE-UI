<template>
  <div class="gear-container">
    <div class="gear-button" @click="togglePanel">
      <MiscBut />
    </div>
    <transition name="panelfade">
      <div ref="gearpanel" class="gear-panel" v-show="panelOpen">
        <div
          v-for="(item, index) in items"
          :key="index"
          class="section-item"
        >
          <div class="item-title">{{ item.name }}</div>
          <SwitchBut v-if="item.type === 'switch'" />
          <SlideBar v-if="item.type === 'slide'" :unit="item.unit" :min="item.min" :max="item.max" />
          <SingleSmall :ref="getItemRef(index, 'single')" v-if="item.type === 'single'" :options="item.options"/>
          <MultiSmall :ref="getItemRef(index, 'multi')" v-if="item.type === 'multi'" :options="item.options"/>
        </div>
      </div>
    </transition>
  </div>
</template>

<script>
import MiscBut from '../icons/MiscBut.vue';
import SwitchBut from './SwitchBut.vue';
import SlideBar from './SlideBar.vue';
import SingleSmall from './SingleSmall.vue';
import MultiSmall from './MultiSmall.vue';
export default {
  name: 'GearPanel',
  components: {
    MiscBut,
    SwitchBut,
    SlideBar,
    SingleSmall,
    MultiSmall,
  },
  props: {
    // 接收外部items
    items: {
      type: Array,
      required: true,
    },
  },
  mounted() {
    document.addEventListener('click', this.handleClickOutside);
  },
  beforeUnmount() {
    document.removeEventListener('click', this.handleClickOutside);
  },
  data() {
    return {
      panelOpen: false,
    }
  },
  methods: {
    togglePanel() {
      this.panelOpen = !this.panelOpen;
    },
    closePanel() {
      this.panelOpen = false;
    },
    getItemRef(index, type) {
      return `${type}ItemRef_${index}`;
    },
    handleClickOutside(event) {
      for (let ref in this.$refs) {
        const refValue = this.$refs[ref];
        
        if (Array.isArray(refValue)) {
          // Handle array of refs (e.g., multiSelect)
          refValue.forEach((component) => {
            if (component.$el && !component.$el.contains(event.target)) {
              if (component.closeDropdown) {
                component.closeDropdown();
              }
            }
          });
        } else {
          // Handle single refs
          if (refValue.$el && !refValue.$el.contains(event.target)) {
            if (refValue.closeDropdown) {
              refValue.closeDropdown();
            }
          }
        }
      }
    }
  },
}
</script>

<style scoped>
.gear-container {
  position: relative; /* 父容器相对定位 */
  margin-left: auto;
  overflow: visible;
}

.gear-button {
  margin-top: 2px;
  position: relative; /* 使 gear-panel 相对 gear-button 定位 */
  align-self: flex-end;
  cursor: pointer;
}

.gear-panel {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  position: absolute;
  width: 300px;
  top: 50%; /* 让 gear-panel 显示在 gear-button 下方 */
  left: 50%;
  margin-top: 2px;
  background: #b9b9b940;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  border: 1px solid #ccc;
  border-radius: 8px;
  box-shadow: 0 0 20px 10px rgba(0, 0, 0, 0.1);
  z-index: 99999;
}

/* 添加动画效果 */
.panelfade-enter-active, .panelfade-leave-active {
  transition: opacity 0.4s cubic-bezier(0.25, 1, 0.5, 1);
}
.panelfade-enter-from,
.panelfade-leave-to {
  opacity: 0;
}
</style>
