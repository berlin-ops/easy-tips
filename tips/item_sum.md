# 求数组中和为某值的所有成员集合,不能出现重复项

[1, 2, 3, 4, 5, 6] (实际情况乱序)中和为7的成员集合 => [[1, 2, 4], [1, 6]...]

> 这个，真不简单，来自老大的代码

```js
/**
 * @param {number[]} arr
 * @param {number} target
 */
function getSpecItem(arr, target) {
  const stack = [];
  const result = [];
  arr.sort((a, b) => b - a);

  let index = 0, oldIndex;
  
  while (true) {
    const sum = stack.reduce((prev, curr) => prev + curr.value, 0);
    if (sum >= target) {
      if (sum === target) {
        console.log(stack.map(x => x.value));
        result.push(stack.map(x => x.value));
      }
      do {
        index = stack.pop().index + 1;
      } while (index >= arr.length);
    }
    stack.push({ index, value: arr[index] });
    if (index >= arr.length) {
      do {
        if (stack.length < 2) return result;
        stack.length -= 1;
        index = stack.pop().index + 1;
        stack.push({ index, value: arr[index] });
      } while (index + 1 >= arr.length);
    }
    ++index;
  }
}

console.log(getSpecItem([1, 2, 3, 4,5,6,7,8,9], 17).length);

getSpecItem([1, 2, 3, 5, 7], 8);
```

> 自己写

```js
// 看小姐姐跳舞入迷了，忘记这事了，有 BUG，明天看
function getSpecItem(arr, num) {
  const tArr = [];
  const sArr = arr.sort((a, b) => b - a);
  if (sArr[sArr.length - 1] > num) {
    return [];
  }

  void function fn(a, cur, sum) {
    a.forEach((v, i) => {
      if (a.length > 1 && i < a.length - 1) {
        if (sum + v < num) {
          const _a = a.slice(i + 1, a.length);
          fn(_a, cur.concat(v), sum + v);
        } else if (sum + v > num) {
          const _a = a.slice(i + 1, a.length);
          fn(_a, cur, sum);
        } else {
          tArr.push(cur.concat(v));
        }
      } else {
        if (sum + v === num) {
          tArr.push(cur.concat(v));
        }
      }
    });
  }(sArr, [], 0)

  return tArr;
}
```
> 这块的思路不对，我晚点补充，最近刷了 LeetCode 的求 NSum 总结的
