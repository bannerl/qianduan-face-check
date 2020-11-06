我们给localStorage封装一个上级方法set，在set方法中添加一个有效时间
```
import Vue from 'vue'
Vue._expirseCache = []
const storage = {
    set (key, value, time) {
        let tempValue = {
            value,
            time
        }
        Vue._expirseCache.push(key)
        window.localstorage.setItem(key, JSON.stringify(tempValue))
    },
    get (key) {
        
    }
}
```
然后写一个定时器对storage进行定时检测
```
setInterval(function(){
    Vue._expirseCache.forEach(item => {
        let cache = window.localstorage.getItem(item)
        let time = JSON.parse(cache).time
        // 校验是否过期
        if (time < 1000) {
             // 过期抛出事件
            observer.emit('remove' + key)
            window.localstorage.removeItem(item)
        }
       
    })
}, 5000)
```
同时为了监听指定字段失效，可以采用观察者模式在移除的时候发布一个移除事件
```
observer.on('remove' + key, function () {
    console.log('已移除key')
})

```