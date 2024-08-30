<template>
    <GearPanel />
    <div class="select-container">
      <div class="selected-items" @click="toggleDropdown">
        <div class="selected-label">
            {{ selectedLabels }}
        </div>
        <span class="arrow"></span>
      </div>
      <transition name="fade">
        <div ref="dropdown" class="dropdown" v-show="isOpen">
          <div 
            v-for="(option, index) in options" 
            :key="option.label" 
            class="dropdown-item"
            :class="{ active: isSelected(option) }"
            :data-index="index"
            @click="selectOption(option)"
          >
            <div class="check-style-unequal-width"></div>
            <span>{{ option.label }}</span>
          </div>
        </div>
      </transition>
    </div>
</template>
  
<script>
import GearPanel from './GearPanel.vue'
import gsap from 'gsap'
export default {
    components: {
      GearPanel,
    },
    props: {
      options: {
        type: Array,
        required: true
      },
      value: {
        type: Array,
        default: () => []
      }
    },
    data() {
      return {
        isOpen: false,
        selectedValues: this.value
      };
    },
    computed: {
      selectedOptions() {
        if (this.selectedValues.length === 0) {
          return [{ label: 'Select...' }];
        }
        return this.options.filter(option => this.selectedValues.includes(option.value));
      },
      selectedLabels() {
        return this.selectedOptions.map(option => option.label).join(', ');
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
        if (this.isSelected(option)) {
          this.removeOption(option);
        } else {
          this.selectedValues.push(option.value);
          this.$emit('input', this.selectedValues);
        }
      },
      removeOption(option) {
        this.selectedValues = this.selectedValues.filter(value => value !== option.value);
        this.$emit('input', this.selectedValues);
      },
      isSelected(option) {
        return this.selectedValues.includes(option.value);
      },
      animateItems() {
        console.log(1);
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
  display: flex;
  position: relative;
  width: 50%;
  /* padding: 0 5px 0 10px;
  background: #ffffff70;
  border-radius: 6px;
  border: #454f582f 1px solid;
  cursor: pointer;
  transition: border-color 0.6s cubic-bezier(0.25, 1, 0.5, 1); */
}
.selected-items {
  display: flex;
  /* justify-content: space-between; */
  align-items: center;
  min-width: 100%;
  width: 100%;
  justify-self: flex-end;
  /* padding: 2px 2px 2px 0; */
  padding: 0 5px 0 10px;
  background: #ffffff70;
  border-radius: 6px;
  border: #454f582f 1px solid;
  cursor: pointer;
  transition: border-color 0.6s cubic-bezier(0.25, 1, 0.5, 1);
}
.selected-label {
  overflow: hidden; /* 隐藏溢出的部分 */
  white-space: nowrap; /* 防止文本换行 */
  text-overflow: ellipsis;
  padding-right: 5px;
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
  /* border-color: #009933; */
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