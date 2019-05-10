## 求下一个较小的数字

- 前导数不能为 0 
- 如果没有下一个较小的数字 则返回 -1 

example:
```
nextSmaller(21) == 12
nextSmaller(531) == 513
nextSmaller(2071) == 2017

nextSmaller(9) == -1
nextSmaller(111) == -1
nextSmaller(135) == -1
nextSmaller(1027) == -1 // 0721 前导数为0  所以返回 -1
```


---


### 1.1 穷举(性能不好,不推荐)

> 思路 每次 -1 比对位上的数字是否相等 如果相等 则是下一个较小的数字

```
function nextSmaller(n) {
    var x = n
    while(x--) {
        if (getNumerSortStr(n) === getNumerSortStr(x)) {
            break;
        }
    }

    function getNumerSortStr(n) {
        return [...`${n}`].sort().toString()
    }
    return x === n ? -1 : x
}

```

### 1.2 穷举第二种(性能比1.1 略好一点)

> 思路 数字=>字符串=>字符串数组=>数字数组=>从小到大排序=>把前面的0 放到后面 每次用原数字-1 在用这个方法去比对

> 用一些特殊的数字 效率很低 如 `9111111111` 

```
function nextSmaller(n) [
    let min = minify(n);
    while (--n >= min)if (minify(n) === min)return n
    return -1;
  };
  const minify = n => [...`${n}`].sort().join``.replace(/^(0+)([1-9])/, '$2$1');
]

```

~~暂时没有特别好的思路去解决 后续补充~~