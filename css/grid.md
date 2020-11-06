
### grid
```
.box {
    display: grid; // inline-grid;
    
    // 固定宽度
    grid-template-columns: 100px 100px 100px;
    grid-template-rows: 100px 100px 100px;
    
    //按比例划分 
    grid-template-columns: 33.33% 33.33% 33.33%;
    grid-template-rows: 33.33% 33.33% 33.33%;
    
    // 重复关键字
    grid-template-columns: repeat(3, 33.33%);
    grid-template-rows: repeat(3, 33.33%);
    
    // 自动填充
    grid-template-columns: repeat(auto-fill, 100px);
    
    // fr关键字
    grid-template-columns: 1fr 1fr;
    
    // minmax 最大最小区间
    grid-template-columns: 1fr 1fr minmax(100px, 1fr);
    
    // 浏览器自动填充
    grid-template-columns: 100px auto 100px;
    
    // 网格线的名称，以便后面引用
    grid-template-columns: [c1] 100px [c2] 100px [c3] auto [c4];
    grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
}
```


