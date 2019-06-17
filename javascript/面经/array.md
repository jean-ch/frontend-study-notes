##### JavaScript怎么清空数组
1. arrayList = [];
2. arrayList.length = 0;
3. arrayList.splice(0, arrayList.length);

##### reduce 
```
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2]
);
```
答案： [1, 2, 0, 1, 2, 3]   
reduce的第二个参数是initial value, 即第一个参数callback最先调用的元素。如果没有第二个参数，则从array的第一个元素开始调  