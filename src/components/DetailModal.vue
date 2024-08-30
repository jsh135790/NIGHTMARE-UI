<template>
  <!-- <div v-show="isVisible" class="modal-overlay" @click.self="closeModal"> -->
  <transition name="fade">
    <div 
      v-show="isVisible"
      class="modal-content" 
      :style="{ top: topPosition + 'px', left: leftPosition + 'px' }"
      @mousedown.stop="initDrag"
      ref="modalContent"
    >
      <div class="modal-title" ref="modalTitle">
        <div class="title-text">
          <MiscIcon />
          <strong>{{ title }}</strong>
        </div>
        <button class="close-button" @click="closeModal">x</button>
      </div>
      <div class="modal-body">
        <div class="logo">
          <h1>NIGHTMARE.PLUS</h1>
        </div>
        <div class="author-info">
          <p>Author: <strong>Pilot1337</strong></p>
          <p>Branch: <strong>Beta 1.8.7.2</strong></p>
          <p>Updated: <strong>Aug 14 / 2024</strong></p>
          <p>Valid Until: <strong>Never Expire</strong></p>
        </div>
        <div class="copyright">
          <p>{{ content }}</p>
        </div>
        <div class="modal-item">
          <div class="item-title">自动保存</div>
          <SwitchBut />
        </div>
        <div class="modal-item">
          <div class="item-title">UI语言</div>
          <SingleSmall ref="infoSelect" :options="options"/>
        </div>
      </div>
    </div>
  </transition>
  <!-- </div> -->
</template>

<script>
import MiscIcon from './icons/MiscIcon.vue';
import SwitchBut from './interactions/SwitchBut.vue';
import SingleSmall from './interactions/SingleSmall.vue';
export default {
  name: 'DetailModal',
  components: {
    MiscIcon,
    SwitchBut,
    SingleSmall
  },
  props: {
    title: {
      type: String,
       required: true,
    },
    content: {
      type: String,
      required: true,
    },
    isVisible: {
      type: Boolean,
      required: true,
    }
  },
  mounted() {
    document.addEventListener('click', this.handleClickOutside);
  },
  beforeUnmount() {
    document.removeEventListener('click', this.handleClickOutside);
  },
  data() {
    return {
      options: [
        { label: '简体中文', value: 1 },
        { label: 'English', value: 2 },
        { label: '日本語', value: 3 },
      ],
      topPosition: 100, // 初始位置
      leftPosition: 100, // 初始位置
      isDragging: false,
      dragStartX: 0,
      dragStartY: 0,
      initialTop: 0,
      initialLeft: 0,
    }
  },
  methods: {
    closeModal() {
      this.$emit('close');
    },
    initDrag(event) {
      if (event.target !== this.$refs.modalTitle && !this.$refs.modalTitle.contains(event.target)) {
        return;
      }
      this.isDragging = true;
      this.dragStartX = event.clientX;
      this.dragStartY = event.clientY;
      this.initialTop = this.topPosition;
      this.initialLeft = this.leftPosition;

      document.addEventListener('mousemove', this.drag);
      document.addEventListener('mouseup', this.stopDrag);
    },
    drag(event) {
      if (!this.isDragging) return;
      const deltaX = event.clientX - this.dragStartX;
      const deltaY = event.clientY - this.dragStartY;
      this.topPosition = this.initialTop + deltaY;
      this.leftPosition = this.initialLeft + deltaX;
    },
    stopDrag() {
      this.isDragging = false;
      document.removeEventListener('mousemove', this.drag);
      document.removeEventListener('mouseup', this.stopDrag);
    },
    handleClickOutside(event) {
      for (let ref in this.$refs) {
        // console.log(this.$refs[ref]);
        if (this.$refs[ref] && this.$refs[ref].$el && !this.$refs[ref].$el.contains(event.target) && this.$refs[ref].closeDropdown) {
          this.$refs[ref].closeDropdown();
        }
      }
    }
  }
};
</script>
  
<style scoped>
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  /* background-color: rgba(0, 0, 0, 0.7); */
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal-content {
  position: absolute;
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 15px 15px rgba(0, 0, 0, 0.1);
  background-color: rgba(255, 255, 255, 0.9);
  padding-bottom: 10px;
  border-radius: 8px;
  width: 350px;
  max-width: 80%;
}
.modal-title {
  font-size: 14px;
  display: flex;
  align-items: center;
  padding: 5px 10px 5px 10px;
  border-bottom: #454f582f 1px solid;
}
.title-text {
  display: flex;
  align-items: center;
}
.title-text strong{
  margin-left: 5px;
}
.modal-title svg{
  width: 15px;
  height: 15px;
  fill: #17aef5;
}
.close-button {
  /* position: absolute;
  top: 10px;
  right: 10px; */
  margin-left: auto;
  background: none;
  border: none;
  font-size: 14px;
  font-weight: bold;
  cursor: pointer;
}
.modal-body .logo{
  font-size: 28px; /* Misans专用 */
  margin: 20px;
  padding-bottom: 20px;
  border-bottom: #454f582f 1px solid;
}
.author-info{
  text-align: left;
  margin-left: 20px;
}
.author-info p{
  margin-bottom: 10px;
}
.author-info strong{
  font-weight: normal;
  color: #17aef5;
}
.copyright{
  margin: 20px 20px 0 20px;
  padding-bottom: 20px;
  border-bottom: #454f582f 1px solid;
}
.modal-item {
  height: 40px;
  margin: 0 20px;
  display: flex;
  align-items: center;
  border-bottom: #454f582f 1px solid;
}
.modal-body > :last-child {
  border-bottom: none;
}
/* 添加动画效果 */
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.4s cubic-bezier(0.25, 1, 0.5, 1);
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>