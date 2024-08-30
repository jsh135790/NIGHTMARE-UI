<template>
  <div class="left-panel">
    <div class="logo">
      <h1>NIGHTMARE</h1>
    </div>
    <LeftItem :activeItem="activeItem" @updateActive="updateActiveItem"/>
    <div class="left-avatar">
      <div class="avatar"></div>
      <div class="name">
        <div class="name-text">
          Pilot1337
        </div>
        <div class="exp-time">
          <span>直到:</span> <strong>Never Expire</strong>
        </div>
      </div>
    </div>
  </div>
  <div class="right-panel">
    <div class="search-bar">
      <div class="save-icon">
        <SaveIcon /> 保存
      </div>
      <div class="weapon-group">
        <!-- <SingleSelect ref="singleSelect" :options="weaponOptions" v-model="singleSelectValue" @input="handleInput" v-if="activeItem === 'AimBot' || activeItem === 'MouseIcon'"/> -->
        <MultiSelect ref="multiSelect" :options="weaponOptions" v-model="multiSelectValues"/>
      </div>
      <div class="stack-icon" @click="toggleModal">
        <StackIcon/>
      </div>
      <div class="search-icon">
        <SearchIcon @click="IsActive = !IsActive"/>
        <input type="text" class="search-input" :class="{ active: IsActive }" placeholder="搜索 ..." />
      </div>
    </div>
    <HelloWorld v-show="activeItem === null" msg="欢迎访问NIGHTMARE-UI主页"/>
    <RagePanel v-show="activeItem === 'AimBot'"/>
    <LegitPanel v-show="activeItem === 'MouseIcon'"/>
    <VisualPanel v-show="activeItem === 'VisualIcon'"/>
    <InvPanel v-show="activeItem === 'InventIcon'"/>
    <MiscPanel v-show="activeItem === 'MiscIcon'"/>
    <ConfPanel v-show="activeItem === 'ConfIcon'"/>
    <!-- 引入 DetailModal 组件 -->
  </div>
  <DetailModal 
    :isVisible="isModalVisible"
    :title="modalTitle"
    :content="modalContent"
    @close="toggleModal"
  />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import LeftItem from './components/LeftItem.vue';
import SaveIcon from './components/icons/SaveIcon.vue';
import StackIcon from './components/icons/StackIcon.vue';
import SearchIcon from './components/icons/SearchIcon.vue';
import RagePanel from './components/RagePanel.vue';
import LegitPanel from './components/LegitPanel.vue';
import VisualPanel from './components/VisualPanel.vue';
import InvPanel from './components/InvPanel.vue';
import MiscPanel from './components/MiscPanel.vue';
import ConfPanel from './components/ConfPanel.vue';

import MultiSelect from './components/interactions/MultiSelect.vue';
import DetailModal from './components/DetailModal.vue';
export default {
  name: 'App',
  components: {
    HelloWorld,
    LeftItem,
    SaveIcon,
    StackIcon,
    SearchIcon,
    RagePanel,
    LegitPanel,
    VisualPanel,
    InvPanel,
    MiscPanel,
    ConfPanel,
    MultiSelect,
    DetailModal,
  },
  mounted() {
    document.addEventListener('click', this.handleClickOutside);
  },
  beforeUnmount() {
    document.removeEventListener('click', this.handleClickOutside);
  },
  data() {
    return {
      weaponOptions: [
        { label: '所有武器', value: 1 },
        { label: '手枪', value: 2 },
        { label: '步枪', value: 3 },
        { label: '狙击枪', value: 4 }
      ],
      singleSelectValue: null,
      multiSelectValues: [],
      activeItem: null, // 追踪当前激活的left-item
      IsActive: false,
      isModalVisible: false, // 控制弹窗显示
      modalTitle: 'About NIGHTMARE-UI', // 弹窗标题
      modalContent: 'nightmare.plus © 2020-2024' // 弹窗内容
    };
  },
  methods: {
    updateActiveItem(item) {
      this.activeItem = item;
      console.log("Current panel: ", this.activeItem);
    },
    handleInput(value) {
      this.singleSelectValue = value;
      console.log(value);
    },
    toggleModal() {
      
      this.isModalVisible = !this.isModalVisible; // 切换弹窗显示状态
      console.log(this.isModalVisible);
    },
    handleClickOutside(event) {
      for (let ref in this.$refs) {
        if (this.$refs[ref] && this.$refs[ref].$el && !this.$refs[ref].$el.contains(event.target)) {
          if(this.$refs[ref].closeDropdown){
            this.$refs[ref].closeDropdown();
          }
        }
      }
    }
  }
}
</script>

<style>
#app {
  position: relative;
  display: flex;
  width: 70%;
  height: 80vh;
  max-width: 1100px;
  max-height: 100vh;
  position: relative;
  box-shadow: 0 0 15px 15px rgba(0, 0, 0, 0.1);
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #333333;
  margin-top: 60px;
  overflow: visible;
}
.logo {
  position: relative; /* 确保 z-index 生效 */
  background: transparent;
  transition: all 0.6s cubic-bezier(0.25, 1, 0.5, 1);
  text-shadow: 0px 0px 0px rgba(0, 0, 0, 0.4);
  z-index: 0; /* 确保阴影在正确层级 */
}

.logo h1 {
  font-size: 28px; /* Misans专用 */
  background: transparent;
  -webkit-background-clip: text;
  transition: all 0.6s cubic-bezier(0.25, 1, 0.5, 1);
  /* box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.4); 效果也牛逼 */
  z-index: 1; /* 确保 h1 的文本在正确层级 */
  position: relative;
}

.logo:hover {
  text-shadow: 0px 2px 15px rgba(0, 0, 0, 0.4);
  transform: scale(1.05);
  z-index: 0; /* 确保渐变和阴影在同一层级 */
}

.logo h1:hover {
  background: linear-gradient(to right, #5c88fe, #7657ff, #de61fd, #5c88fe);
  background-size: 400%;
  animation: bg-rgb 5s linear infinite;
  -webkit-background-clip: text;
  color: transparent;
}
@keyframes bg-rgb {
  0% {
    background-position: 400% 0; /* 起始位置 */
  }
  100% {
    background-position: 0 0; /* 结束位置，使背景图水平滚动 */
  }
}
.left-panel {
  overflow-y: scroll;
  width: 230px;
  display: flex;
  padding: 0 15px;
  flex-direction: column;
  background: #ffffff80;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
  backdrop-filter:  blur(10px);
  -webkit-backdrop-filter: blur(10px);
}
.left-avatar {
  display: flex;
  align-items: flex-start;
  margin-top: auto;
  margin-left: 5px;
  width: 100%;
}
.avatar {
  flex-shrink: 0;
  width: 45px;
  position: relative;
  aspect-ratio: 1/1;
  
  margin-bottom: 20px;
  margin-right: 10px;
  background-image: url(./assets/avatar.jpg);
  background-size: cover;
  border-radius: 50%;
}
.name {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
}
.name-text {
  font-size: 16px;
  font-weight: 600;
}
.name span {
  font-size: 14px;
  color: #454f58;
}
.name strong {
  font-weight: normal; /* Misans专用 */
  font-size: 16px;
  color: #17aef5;
}

.right-panel {
  width: calc(100% - 230px);
  display: flex;
  padding: 20px;
  padding-bottom: 50px;
  flex-direction: column;
  background-color: rgba(255, 255, 255, 0.8);
  border-top-right-radius: 10px;
  border-bottom-right-radius: 10px;
  backdrop-filter:  blur(10px);
  -webkit-backdrop-filter: blur(10px);
  overflow: visible scroll;
  /* overflow控制右侧UI区域不溢出，但是Y轴overflow状态会覆盖X轴，导致GearPanel被遮盖(解决不了) */
}

.right-panel svg{
  width: 20px;
  height: 20px;
  margin-right: 10px;
  fill: #343434c2;
}
.search-bar {
  display: flex;
  align-items: center;
  
  margin-bottom: 20px;
  border-bottom: #454f586d 1px solid;
}
.save-icon {
  display: flex;
  align-items: center;
  justify-items: center;
  padding: 5px 20px;
  margin-bottom: 10px;
  border-radius: 8px;
  border: #454f582f 2px solid;
  cursor: pointer;
  transition: border-color 0.6s cubic-bezier(0.25, 1, 0.5, 1);
}
.save-icon:hover {
  border-color: #454f58;
}
.stack-icon{
  margin-left: auto;
  padding-top: 5px;
  padding-bottom: 5px;
  cursor: pointer;
}
.search-icon{
  display: flex;
  align-items: center;
  margin-left: 5px;
  padding-top: 7px;
  padding-bottom: 10px;
  cursor: pointer;
}
.stack-icon svg, .search-icon svg {
  transition: fill 0.6s cubic-bezier(0.25, 1, 0.5, 1);
}
.stack-icon svg:hover, .search-icon svg:hover {
  fill: #454f58;
}
.search-input {
  background-color: transparent;
  border: none;
  width: 0;
  outline: none;
  color: rgb(100, 100, 100);
  transition: width 1s cubic-bezier(0.25, 1, 0.5, 1);
  font-weight: bold;
}
.search-input.active {
  width: 100px;
}
.logo {
  margin-top: 20px;
}

.logo img {
  width: 24px;
  height: 24px;
}

/* 子版块样式 */
.sub-panel {
  display: flex;
  flex-flow: row wrap;
  align-items: flex-start;
  gap: 10px;
  overflow: visible;
}
.sub-left, .sub-right {
  display: flex;
  flex-direction: column;
  width: calc(50% - 5px);
  gap: 10px;
  overflow: visible;
}
.section-text {
  margin-left: 10px;
  text-align: left;
  color: #778899;
  font-size: 8px;
}
.section-text h3 {
  letter-spacing: 0.1em;
  font-weight: lighter;
}
.sub-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: flex-start;
  /* width: calc(50% - 5px); */
  min-height: 0;
  padding: 0;
  background: #b9b9b930;
  border: #454f582f 2px solid;
  border-radius: 8px;
  overflow: visible;
}
.section-item {
  width: 95%;
  height: 40px;
  padding: 7px 10px 7px 10px;
  display: flex;
  align-items: center;
  border-top: #454f582f 1px solid;
  overflow: visible;
}
.sub-section > :first-child {
  border-top: none;
}
</style>
