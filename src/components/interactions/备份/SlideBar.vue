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
export default {
    name: 'SliderComponent',
    props: {
      modelValue: {
        type: Number,
        required: true,
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
        return this.value === 0 ? 'Auto' : `${this.value}${this.unit}`;
      },
      sliderStyle() {
        const percentage = ((this.value - this.min) / (this.max - this.min)) * 100;
        return {
            background: `linear-gradient(to right, #007bff ${percentage}%, #ffffff70 ${percentage}%)`,
        };
      },
    },
    watch: {
      value(newValue) {
        this.$emit('update:modelValue', newValue);
      },
    },
    methods: {
      updateValue(event) {
        this.value = event.target.valueAsNumber;
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
  z-index: 1;
}
  
.slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 15px;
  height: 15px;
  background: #007bff;
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