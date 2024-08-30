<template>
    <!-- <h1> 这里是RagePanel页面 </h1> -->
    <div class="sub-panel">
      <div class="sub-left">
        <div class="section-text"><h3>MAIN</h3></div>
        <div class="sub-section">
          <div class="section-item">
            <div class="item-title">开启</div>
            <GearSwitch :items="items"/>
          </div>
          <div class="section-item">
            <div class="item-title">静默瞄准</div>
            <GearSwitch :items="silent_items"/>
          </div>
          <div class="section-item">
            <div class="item-title">自动开火</div>
            <SwitchBut />
          </div>
          <div class="section-item">
            <div class="item-title">自动穿墙</div>
            <SwitchBut />
          </div>
          <div class="section-item">
            <div class="item-title">瞄准范围</div>
            <SlideBar v-model="sliderValue" unit="°" max="180"/>
          </div>
          <div class="section-item">
            <div class="item-title">土狗程度</div>
            <GearSlider :items="items"/>
          </div>
        </div>
        <div class="section-text"><h3>SELECTION</h3></div>
        <div class="sub-section">
          <div class="section-item">
            <div class="item-title">目标选择器</div>
            <SingleSmall ref="singleSelect1" :options="target_options"/>
          </div>
          <div class="section-item">
            <div class="item-title">击打部位</div>
            <MultiSmall ref="singleSelect2" :options="hitbox_options"/>
          </div>
          <div class="section-item">
            <div class="item-title">多点射击</div>
            <MultiSmall ref="singleSelect2" :options="hitbox_options"/>
          </div>
          <div class="section-item">
            <div class="item-title">命中率</div>
            <SlideBar unit="%"/>
          </div>
          <div class="section-item">
            <div class="item-title">最低伤害</div>
            <SlideBar unit="" max="130"/>
          </div>
          <div class="section-item">
            <div class="item-title">自动急停</div>
            <GearSwitch :items="stop_items"/>
          </div>
          <div class="section-item">
            <div class="item-title">自动开镜</div>
            <SwitchBut />
          </div>
        </div>
      </div>
      <div class="sub-right">
        <div class="section-text"><h3>OTHER</h3></div>
        <div class="sub-section">
          <div class="section-item">
            <div class="item-title">回溯抓取</div>
            <SingleSmall ref="singleSelect3" :options="history_options"/>
          </div>
          <div class="section-item">
            <div class="item-title">延迟射击</div>
            <SwitchBut />
          </div>
          <div class="section-item">
            <div class="item-title">蹲拉助手</div>
            <SwitchBut />
          </div>
          <div class="section-item">
            <div class="item-title">Peek助手</div>
            <GearSwitch :items="peek_items"/>
          </div>
          <div class="section-item">
            <div class="item-title">Crosshair</div>
            <MultiSmall ref="singleSelect4" :options="options"/>
          </div>
          <div class="section-item">
            <div class="item-title">Crosshair</div>
            <SingleSmall ref="singleSelect5" :options="options"/>
          </div>
        </div>
        <div class="section-text"><h3>ANTI-AIM</h3></div>
        <div class="sub-section">
          <div class="section-item">
            <div class="item-title">开启</div>
            <SwitchBut />
          </div>
          <div class="section-item">
            <div class="item-title">纵向角度</div>
            <SingleSmall ref="singleSelect6" :options="pitch_options"/>
          </div>
          <div class="section-item">
            <div class="item-title">朝向角度</div>
            <SingleSmall ref="singleSelect6" :options="yaw_options"/>
          </div>
          <div class="section-item">
            <div class="item-title">慢走</div>
            <SwitchBut />
          </div>
          <div class="section-item">
            <div class="item-title">自动藏头</div>
            <GearSwitch :items="peek_items"/>
          </div>
        </div>
      </div>
    </div>
</template>

<script>
import SingleSmall from './interactions/SingleSmall.vue';
import MultiSmall from './interactions/MultiSmall.vue';
import SwitchBut from './interactions/SwitchBut.vue';
import SlideBar from './interactions/SlideBar.vue';
import GearSlider from './interactions/GearSlider.vue';
import GearSwitch from './interactions/GearSwitch.vue';
export default {
  components: {
    SingleSmall,
    MultiSmall,
    SwitchBut,
    SlideBar,
    GearSlider,
    GearSwitch,
  },
  mounted() {
    document.addEventListener('click', this.handleClickOutside);
  },
  beforeUnmount() {
    document.removeEventListener('click', this.handleClickOutside);
  },
  data() {
    return {
      items: [
        {
          name: '减少瞄准步骤',
          type: 'switch',
        },
        {
          name: 'slide test',
          type: 'slide',
          min: 50,
          max: 100,
          unit: 'm',
        },
        {
          name: 'Multi test',
          type: 'multi',
          options: [
            { label: 'Option 1', value: 1 },
            { label: 'Option 2', value: 2 },
            { label: 'Option 3', value: 3 },
            { label: 'Option 4', value: 4 },
            { label: 'Option 5', value: 5 }
          ],
        },
        {
          name: 'Single test',
          type: 'single',
          options: [
            { label: 'Option 1', value: 1 },
            { label: 'Option 2', value: 2 },
            { label: 'Option 3', value: 3 },
            { label: 'Option 4', value: 4 },
            { label: 'Option 5', value: 5 }
          ],
        },
      ],
      silent_items: [
        {
          name: '完美静默',
          type: 'switch',
        },
        {
          name: '无扩散射击',
          type: 'switch',
        },
        {
          name: '快速射击',
          type: 'switch',
        },
        {
          name: 'Multi test',
          type: 'multi',
          options: [
            { label: 'Option 1', value: 1 },
            { label: 'Option 2', value: 2 },
          ]
        }
      ],
      stop_items: [
        {
          name: '急停条件',
          type: 'multi',
          options: [
            { label: '提前急停', value: 1 },
            { label: '开火间隙', value: 2 },
            { label: '蹲伏急停', value: 3 },
          ]
        }
      ],
      peek_items: [
        {
          name: '返回条件',
          type: 'multi',
          options: [
            { label: '射击结束时', value: 1 },
            { label: '按键释放时', value: 2 },
          ]
        },
        // {
        //   name: 'Peek距离',
        //   type: 'slider',
        //   unit: 'm',
        //   min: 0,
        //   max: 150,
        // }
      ],
      target_options: [
        { label: '距离最近', value: 1 },
        { label: '血量最低', value: 2 },
        { label: '命中率最高', value: 3 },
      ],
      hitbox_options: [
        { label: '头部', value: 1 },
        { label: '胸部', value: 2 },
        { label: '胃部', value: 3 },
        { label: '手臂', value: 4 },
        { label: '腿部', value: 5 },
        { label: '脚部', value: 6 },
      ],
      history_options: [
        { label: '无回溯', value: 1 },
        { label: '低回溯', value: 2 },
        { label: '中回溯', value: 3 },
        { label: '高回溯', value: 4 },
        { label: '最高回溯', value: 5 },
      ],
      pitch_options: [
        { label: '禁用', value: 1 },
        { label: '抬头', value: 2 },
        { label: '89°', value: 3 },
      ],
      yaw_options: [
        { label: '禁用', value: 1 },
        { label: '180°', value: 2 },
        { label: '鼠标指向', value: 3 },
        { label: '朝向目标', value: 4 },
      ],
      options: [
        { label: 'Option 1', value: 1 },
        { label: 'Option 2', value: 2 },
        { label: 'Option 3', value: 3 },
        { label: 'Option 4', value: 4 },
        { label: 'Option 5', value: 5 }
      ],
      sliderValue: 50
    }
  },
  methods: {
    handleClickOutside(event) {
      console.log(this.$refs.gearMulti1);
      for (let ref in this.$refs) {
        if (!this.$refs[ref].$el.contains(event.target) && this.$refs[ref].closeDropdown) {
          this.$refs[ref].closeDropdown();
        }
      }
    }
  }
}
</script>