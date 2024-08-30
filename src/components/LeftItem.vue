<template>
  <LeftText labelText="自瞄" />
  <div class="left-div">
    <div class="left-item" :class="{ active: activeItem === 'AimBot' }" @click="setActive('AimBot')">
        <AimBot /> 暴力
    </div>
    <div class="left-item" :class="{ active: activeItem === 'MouseIcon' }" @click="setActive('MouseIcon')">
        <MouseIcon /> 合法
    </div>
  </div>
  <LeftText labelText="普通" />
  <div class="left-div">
    <div class="left-item" :class="{ active: activeItem === 'VisualIcon' }" @click="setActive('VisualIcon')">
        <VisualIcon /> 视觉
    </div>
    <div class="left-item" :class="{ active: activeItem === 'InventIcon' }" @click="setActive('InventIcon')">
        <InventIcon /> 库存
    </div>
    <div class="left-item" :class="{ active: activeItem === 'MiscIcon' }" @click="setActive('MiscIcon')">
        <MiscIcon /> 杂项
    </div>
  </div>
  <LeftText labelText="预设" />
  <div class="left-div">
    <div class="left-item" :class="{ active: activeItem === 'ConfIcon' }" @click="setActive('ConfIcon')">
        <ConfIcon /> 配置
    </div>
  </div>
</template>
  
<script>
import LeftText from './LeftText.vue';
import AimBot from './icons/AimBot.vue';
import MouseIcon from './icons/MouseIcon.vue';
import VisualIcon from './icons/VisualIcon.vue';
import InventIcon from './icons/InventIcon.vue';
import MiscIcon from './icons/MiscIcon.vue';
import ConfIcon from './icons/ConfIcon.vue';
export default {
    props: {
      activeItem: {
        type: String,
        default: null
      }
    },
    components: {
      LeftText,
      AimBot,
      MouseIcon,
      VisualIcon,
      InventIcon,
      MiscIcon,
      ConfIcon,
    },
    methods: {
      setActive(item) {
        this.$emit('updateActive', item);
      }
    }
}
</script>
  
<style>
.left-div{
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: flex-start;
    
    cursor: pointer;
    margin: 10px 0;
}
.left-div svg{
    width: 20px;
    height: 20px;
    margin-right: 10px;
    fill: #17aef5;
}
.left-div .left-item{
    display: flex;
    width: 100%;
    border-radius: 8px;
    align-items: center;
    background-color: transparent;
    padding: 8px 10px;
    margin-top: 4px;
    /* margin-bottom: 2px; */
    transition: background-color 0.4s cubic-bezier(0.5, 1, 0.89, 1);
}
.left-item:hover{
    background-color: #f2fcff30;
}
.left-item.active{
    background-color: #f2fcff60;
}
.left-div > :last-child{
    margin-bottom: 0;
}
</style>