    this.refs.aaa.addEventListener('touchmove',
    () => {}, { passive: true })


    capture: false, //捕获
    passive: false, // if true 意味着listener永远不远调用preventDefault方法
    once: false    //只触发一次
    
因为我们在给元素添加touchstart的时候，可以阻止一些默认行为，这时候浏览器不确定我们是否阻止了默认行为，所以需要执行我们写的代码然后才知道是否阻止了默认行为，而我们写的代码可能执行很长时间，就会导致浏览器不会滚动，等待我们写的代码执行完毕后才会执行，我们可以通过设置 passive为true ，这样浏览器就不会检查默认事件是否被阻止，直接执行默认行为（可以理解默认行为为页面滚动事件）
