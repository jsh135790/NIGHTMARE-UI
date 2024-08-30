<template>
  <div class="select-container" @click="toggleDropdown">
    <div class="selected-item">
      {{ selectedLabel }}
      <span class="arrow"></span>
    </div>
    <transition name="fade">
      <div ref="dropdown" class="dropdown" v-show="isOpen">
        <!-- <transition-group name="list" tag="div"> -->
        <div 
          v-for="(option, index) in options" 
          :key="option.label" 
          class="dropdown-item"
          :class="{ active: selectedValue === option.value }"
          :data-index="index"
          @click="selectOption(option)"
        >
          <div class="check-style-unequal-width"></div>
          <span>{{ option.label }}</span>
        </div>
        <!-- </transition-group> -->
      </div>
    </transition>
  </div>
</template>

<script>
import gsap from 'gsap'
export default {
  props: {
    options: {
      type: Array,
      required: true
    },
    value: {
      type: [String, Number],
      default: 1
    }
  },
  data() {
    return {
      isOpen: false,
      selectedValue: this.value,
      animationPlayed: false
    };
  },
  computed: {
    selectedLabel() {
      const selectedOption = this.options.find(option => option.value === this.selectedValue);
      return selectedOption ? selectedOption.label : '请选择...';
    }
  },
  methods: {
    toggleDropdown() {
      this.isOpen = !this.isOpen;
      if(!this.isOpen){
        this.animationPlayed = false;
        gsap.killTweensOf(this.$refs.dropdown.querySelectorAll('.dropdown-item'));
      }
    },
    closeDropdown() {
      this.isOpen = false;
      this.animationPlayed = false;
      gsap.killTweensOf(this.$refs.dropdown.querySelectorAll('.dropdown-item'));
    },
    selectOption(option) {
      this.selectedValue = option.value;
      this.$emit('input', this.selectedValue);
      this.isOpen = false;
    },
    animateItems() {
      // 使用 GSAP 的 TweenMax 对象
      const tl = gsap.timeline();
      // 获取所有的 dropdown-item 元素
      const items = this.$refs.dropdown.querySelectorAll('.dropdown-item');
      // 依次对每个 item 应用动画
      [].forEach.call(items, (item, index) => {
        tl.fromTo(
          item,
          {
            autoAlpha: 0, // 初始状态不可见
            x: -20,
            y: -20        // 初始状态向上偏移
          },
          {
            autoAlpha: 1, // 结束状态可见
            x: 0,
            y: 0,          // 结束状态回到原位
            duration: 0.15, // 动画持续时间
            ease: "power2.out",
            delay: index * 0.005 // 每个 item 延迟 0.1 秒
          }
        );
      });
      this.animationPlayed = true;
    }
  },
  watch: {
    isOpen(newVal) {
      if (newVal && !this.animationPlayed) {
        // 当下拉菜单打开时，执行动画
        this.animateItems();
      }
    }
  }
}
</script>

<style scoped>
.select-container {
position: relative;
padding: 0 5px 0 10px;
width: 50%;
/* margin-bottom: 10px;
margin-left: 15px; */
margin-left: auto;
background: #ffffff70;
border-radius: 6px;
border: #454f582f 1px solid;
cursor: pointer;
transition: border-color 0.6s cubic-bezier(0.25, 1, 0.5, 1);
}
.selected-item {
display: flex;
align-items: center;
padding: 2px 2px 2px 0;
}

.arrow {
display: inline-block;
width: 0;
height: 0;
padding: 3px;
border-right: #454f58 2px solid;
border-bottom: #454f58 2px solid;
transform: rotate(45deg);
margin-left: auto;
margin-bottom: 3px;
transition: border-color 0.6s cubic-bezier(0.25, 1, 0.5, 1);
}
.dropdown {
position: absolute;
top: 100%;
left: 0;
right: 0;
margin-top: 1px;
background: rgba(255, 255, 255, 0.8);
backdrop-filter:  blur(10px);
-webkit-backdrop-filter: blur(10px);
border: 1px solid #ccc;
border-radius: 8px;
z-index: 1000;
}

.dropdown-item {
text-align: left;
margin: 6px 6px 6px 6px;
padding: 5px;
cursor: pointer;
transition: background-color 0.2s cubic-bezier(0.25, 1, 0.5, 1);
}
.dropdown-item:hover {
background-color: #31313120;
border-radius: 6px;
}
.dropdown-item span{
color: #000000a0;
padding-left: 5px;
padding-right: 5px;
transition: padding-left 0.5s cubic-bezier(0.33, 1, 0.68, 1);
}
.check-style-unequal-width{
display: inline-block;
width: 8px;
height: 16px;
border-style: solid;
border-width: 0 3px 3px 0;
border-color: #000000a0;
transform: rotate(45deg);
position: absolute;
opacity: 0;
left: 5px;
transition: 0.3s cubic-bezier(0.5, 1, 0.89, 1);
}

.dropdown-item.active span {
color: #000000;
padding-left: 30px;
}
.dropdown-item.active .check-style-unequal-width {
border-color: #000000;
opacity: 1;
left: 15px;
}
/* 添加动画效果 */
.fade-enter-active, .fade-leave-active {
transition: opacity 0.2s cubic-bezier(0.33, 1, 0.68, 1);
}
.fade-enter-from,
.fade-leave-to {
opacity: 0;
}
</style>