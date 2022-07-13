# vue3 setup中使用watch和vuex store

```html
<script setup>
        import {useStore} from 'vuex'
        import {watch} from 'vue'

        const store = useStore()
        console.log({store})

        watch(() => store.state.detailTitle, (val) => {
            detailTitle.value = val
        })
</script>
```