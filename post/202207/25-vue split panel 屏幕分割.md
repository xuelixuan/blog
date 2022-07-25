# vue split panel 屏幕分割

```html
npm install splitpanes 

<!-- js -->
// https://antoniandre.github.io/splitpanes/
import { Splitpanes, Pane } from 'splitpanes'
import 'splitpanes/dist/splitpanes.css'


<!-- html -->
<splitpanes class="default-theme" @resized="log('resized', $event)">
    <pane size="15" min-size="5" max-size="15">
        <span>1</span>
    </pane>
        <pane size="15">
        <span>2</span>
        </pane>
        <pane max-size="70">
        <span>3</span>
        </pane>
</splitpanes>
```

可以通过localStorage 把相关数值存储到本地,做到每次刷新页面,分割的尺寸保持调整好的尺寸.