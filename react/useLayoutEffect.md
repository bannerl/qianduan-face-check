
下述代码在使用useEffect的时候，页面会先渲染成0，然后再渲染成随机数，由于更新很快，所以出现了闪烁。使用useLayoutEffect就不会有这种情况。
```
import React, { useEffect, useState, useLayoutEffect, useRef } from 'react';
import { render } from 'react-dom';

function App() {
  const [count, setCount] = useState(0);
  
  useLayoutEffect(() => {
    if (count === 0) {
      const randomNum = 10 + Math.random()*200
      setCount(10 + Math.random()*200);
    }
  }, [count]);

  return (
      <div onClick={() => setCount(0)}>{count}</div>
  );
}

render(<App />, document.getElementById('root'));
```

- useLayoutEffect 相比 useEffect，通过同步执行状态更新可解决一些特性场景下的页面闪烁问题（例如：页面有更新，更新的会在useLayoutEffect之后完成更新，如果页面更新的内容和useLayoutEffect一致，以后者为准。也就是说，更新一个变量后导致页面刷新，而我不想要这个变量引起的页面刷新，想用一个新的页面来替换当前的刷新，就可以用这个）。
- useEffect 可以满足大部分的场景，而且 useLayoutEffect 会阻塞渲染，因此谨慎使用。


https://juejin.im/post/6859602611901825037