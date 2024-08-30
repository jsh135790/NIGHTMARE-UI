<template>
  <GearPanel ref="gearPanel" :items="items" />
  <div class="switch-but">
    <input type="checkbox" :id="uniqueId" class="toggle-checkbox" :checked="modelValue" @change="toggleSwitch">
        <label class="toggle-label" :for="uniqueId">
    </label>
  </div>
</template>
<script>
import GearPanel from './GearPanel.vue';
export default {
  name: 'GearSwitch',
  components: {
    GearPanel,
  },
  props: {
    // 接收外部items
    items: {
      type: Array,
      required: true,
    },
    modelValue: {
      type: Boolean,
      default: false,
    },
    id: {
      type: String,
      default: null,
    },
    name: {
      type: String,
      default: 'toggle-switch',
    },
  },
  mounted() {
    document.addEventListener('click', this.handleClickOutside);
  },
  beforeUnmount() {
    document.removeEventListener('click', this.handleClickOutside);
  },
  computed: {
    uniqueId() {
      // 如果没有传入 id，则使用 name 加上一个随机数生成唯一 id
      return this.id || `${this.name}-${Math.random().toString(36).substr(2, 9)}`;
    },
  },
  methods: {
    toggleSwitch(event) {
      this.$emit('update:modelValue', event.target.checked);
    },
    handleClickOutside(event) {
      for (let ref in this.$refs) {
        if (this.$refs[ref] && this.$refs[ref].$el && !this.$refs[ref].$el.contains(event.target) && this.$refs[ref].closePanel) {
            this.$refs[ref].closePanel();
        }
      }
    },
  },
}
</script>
<style scoped>
.switch-but {
    margin-top: 3px;
    /* margin-left: auto; */
}
.toggle-checkbox {
    display: none; /* 隐藏实际的复选框 */
}

.toggle-label {
    display: inline-block;
    width: 34px;
    height: 20px;
    background-color: transparent;
    border: #454f582f 1px solid;
    border-radius: 15px;
    position: relative;
    cursor: pointer;
    transition: background-color 0.3s;
}

.toggle-label::before {
    content: '';
    position: absolute;
    width: 18px;
    height: 18px;
    background-color: white;
    border-radius: 50%;
    left: 1px;
    transition: left 0.6s cubic-bezier(0.22, 1, 0.36, 1);
}

.toggle-checkbox:checked + .toggle-label {
    background-color: #17aef5;
}

.toggle-checkbox:checked + .toggle-label::before {
    left: 14px;
}
</style>