#vue setup内实现业务逻辑总结


```javascript
    import {
        reactive,
        ref,
        getCurrentInstance,
        nextTick,
    } from 'vue'

    import {
        useRouter
    } from 'vue-router'



setup() {
    // 路由跳转
    const router = useRouter()
    router.push('/message/list')

    // reactive类型赋值
    const msgFormData = () => {
        return {
            title: '', // 这里写form表单要绑定的数据
        }
    }
    // 生成reactive动态响应数据
    const msgForm = reactive(msgFormData())
    // 重置reactive 数据
    Object.assign(msgForm, msgFormData())

    // setup 中使用 $refs 
    const { proxy } = getCurrentInstance()
    // proxy.$refs 可以访问ref变量对象

    // setup中使用 nextTick
    nextTick(()=>{
        // 这里写业务逻辑
    })    
}
```