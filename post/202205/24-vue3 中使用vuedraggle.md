# vue3 中使用 vuedraggle

第一步安装 vuedraggle

`npm install npm i -S vuedraggable@next`

第二步使用

```html
<template>
    <draggable v-model="myArray" group="people" @start="drag=true" @end="drag=false" item-key="id">
        <template #item="{element}">
            <div>{{element.name}}</div>
        </template>
    </draggable>
</template>

<script lang="ts">
    import { defineComponent, reactive, toRefs } from "vue";
    import draggable from "vuedraggable";

    export default defineComponent({
        components: { draggable },
        setup() {
            const data = reactive({
                drag: false,
                list: [
                    { id: 1, name: "Jenny" },
                    { id: 2, name: "kevin" },
                    { id: 3, name: "lili" },
                ],
            });
            return { ...toRefs(data) };
        },
    });
</script>
```

toRefs 会将 reactive 对象中的每个属性转变成响应式数据。
toRefs(data) 的写法相当于

let drag = ref(false)
let list = reactive([])
