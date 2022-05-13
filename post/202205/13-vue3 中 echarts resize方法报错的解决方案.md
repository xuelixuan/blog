# vue3 中 echarts resize 方法报错的解决方案

```html
<div id="card" ref="chartDom" :style="chartStyle"></div>
```

```js
import { shallowRef } from 'vue';
import * as echarts from 'echarts';
import _ from 'lodash';
import { addListener, removeListener } from "resize-detector";

data(){
    return{
      myChart: null,
    },
},
mounted(){
    this.renderChart();
 },
 methods: {
   renderChart() {
      this.myChart = shallowRef(echarts.init(this.$refs.chartDom));
      addListener(this.$refs.chartDom, _.debounce(this.resize, 300));
      this.myChart.setOption(this.chartOption);
    },
    resize() {
      this.myChart.resize();
    },
    disposeChart() {
      if(!this.myChart)
        return;
      this.myChart.dispose();
      this.myChart = null;
    }
 }，
  beforeUnmount() {
    removeListener(this.$refs.chartDom, this.resize);
  },
  unmounted() {
    this.disposeChart();
  }
```

原文地址:https://www.cnblogs.com/hahahakc/p/16144591.html
