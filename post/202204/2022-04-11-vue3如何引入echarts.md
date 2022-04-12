## 如何在 vue3.0 中引入 echarts?

根目录下新建 lib 文件夹,然后创建文件(可以视柱状图,饼状图,折线图)
https://echarts.apache.org/examples/zh/editor.html?c=line-simple
完整代码选项卡下,再选中按需引入,复制按需引入组件

```js
// 堆积图
import * as echarts from 'echarts/core'
import {
	TitleComponent,
	ToolboxComponent,
	TooltipComponent,
	GridComponent,
	LegendComponent,
} from 'echarts/components'
import { LineChart } from 'echarts/charts'
import { UniversalTransition } from 'echarts/features'
import { CanvasRenderer } from 'echarts/renderers'

echarts.use([
	TitleComponent,
	ToolboxComponent,
	TooltipComponent,
	GridComponent,
	LegendComponent,
	LineChart,
	CanvasRenderer,
	UniversalTransition,
])
export default echarts
```

在需要使用图标的组件内直接 import 进来,直接使用.

```js
onMounted(() => {
	let disChart = echarts.init(document.getElementById('disChart'))
	let disOpt = {}
	disChart.setOption(disOpt)
	// resize
	window.addEventListener('resize', () => disChart.resize())
})
```
