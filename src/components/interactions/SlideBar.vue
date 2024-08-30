<template>
  <div class="slider-container">
    <input
    type="range"
    v-model="value"
    @input="updateValue"
    class="slider"
    :min="min"
    :max="max"
    :style="sliderStyle"
    />
    <div class="value-display" ref="valueDisplay">{{ formattedValue }}</div>
  </div>
</template>
  
<script>
import gsap from 'gsap';
export default {
    name: 'SliderComponent',
    props: {
      modelValue: {
        type: Number,
        default: 0,
      },
      unit: {
        type: String,
        default: '%',
      },
      min: {
        type: Number,
        default: 0,
      },
      max: {
        type: Number,
        default: 100,
      },
    },
    data() {
      return {
        value: this.modelValue,
      };
    },
    computed: {
      formattedValue() {
        return this.value === 0 ? 'Auto' : `${Math.round(this.value)}${this.unit}`;
      },
      sliderStyle() {
        const percentage = ((this.value - this.min) / (this.max - this.min)) * 100;
        return {
            background: `linear-gradient(to right, #17aef5 ${percentage}%, #ffffff70 ${percentage}%)`,
        };
      },
    },
    watch: {
      value(newValue) {
        // this.animateValue(newValue);
        this.$emit('update:modelValue', newValue);
      },
    },
    methods: {
      updateValue(event) {
        this.animateValue(event.target.valueAsNumber);
      },
      handleMouseDown(event) {
        this.animateValue(event.target.valueAsNumber);
      },
      animateValue(targetValue) {
        gsap.to(this.$data, {
          duration: 0.5, // 动画持续时间
          value: targetValue,
          ease: 'power2.out', // 缓动函数
          onUpdate: () => {
            this.$emit('update:modelValue', this.value);
            console.log(this.value);
          },
        });
      },
    },
};
</script>
  
<style scoped>
.slider-container {
  display: flex;
  align-items: center;
  margin-left: auto;
  gap: 10px;
  position: relative;
  width: 50%;
}
  
.slider {
  width: 70%;
  height: 6px;
  border-radius: 5px;
  -webkit-appearance: none;
  outline: none;
  position: relative;
  /* z-index: 1; */
}
  
.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  background: #17aef5;
  border-radius: 50%;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.3);
  cursor: pointer;
}
  
.value-display {
  width: 45px;
  padding: 2px 4px 2px 4px;
  background: #ffffff70;
  border: #454f582f 1px solid;
  border-radius: 6px;
  font-size: 13px;
  text-align: center;
  color: #333;
}
</style>