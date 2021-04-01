---
title: 各种排序QAQ
date: 2021-04-01
tags:
 - 算法
categories:
 - 算法
---

在这里，总结一下各种排序啦QAQ

> 冒泡排序

冒泡排序的基本思想是，对相邻的元素进行两两比较，顺序相反则进行交换，这样，每一趟会将最小或最大的元素“浮”到顶端， 最终达到完全有序。

代码实现：

```js
let arr = [2,324,53,34,4,5,46];

/**
 * @param {Object} arr
 * 冒泡排序
 * 直接参考：https://www.cnblogs.com/chengxiao/p/6103002.html
 */
function bubbleSort(arr) {
	if(!Array.isArray(arr)) return;
	
	let lastIndex = arr.length-1;
	// 当最后交换的元素为第一个时，说明后面全部排序完毕
	while(lastIndex>0) {
		let flag = true,k = lastIndex;
		for(let j=0;j<k;j++) {
			if(arr[j] > arr[j+1]) {
				flag = false;
              	lastIndex = j; // 设置最后一次交换元素的位置
                [arr[j], arr[j+1]] = [arr[j+1], arr[j]];
			}
		}
		if(flag) break;
	}
	return arr;
}

console.log(bubbleSort(arr))
```

冒泡排序有两种优化方式。

一种是外层循环的优化，我们可以记录当前循环中是否发生了交换，如果没有发生交换，则说明该序列已经为有序序列了。 因此我们不需要再执行之后的外层循环，此时可以直接结束。

一种是内层循环的优化，我们可以记录当前循环中最后一次元素交换的位置，该位置以后的序列都是已排好的序列，因此下 一轮循环中无需再去比较。

优化后的冒泡排序，当排序序列为已排序序列时，为最好的时间复杂度为 O(n)。

冒泡排序的平均时间复杂度为 O(n²) ，最坏时间复杂度为 O(n²) ，空间复杂度为 O(1) ，是稳定排序。

详细资料可以参考： [《图解排序算法(一)》](https://www.cnblogs.com/chengxiao/p/6103002.html) [《常见排序算法 - 鸡尾酒排序 》](http://bubkoo.com/2014/01/15/sort-algorithm/shaker-sort/) [《前端笔试&面试爬坑系列---算法》](https://juejin.im/post/5b72f0caf265da282809f3b5#heading-1) [《前端面试之道》](https://juejin.im/book/5bdc715fe51d454e755f75ef/section/5bdc724af265da610f632e41)



> 选择排序

选择排序的基本思想为每一趟从待排序的数据元素中选择最小（或最大）的一个元素作为首元素，直到所有元素排完为止。

在算法实现时，每一趟确定最小元素的时候会通过不断地比较交换来使得首位置为当前最小，交换是个比较耗时的操作。其实 我们很容易发现，在还未完全确定当前最小元素之前，这些交换都是无意义的。我们可以通过设置一个变量 min，每一次比较 仅存储较小元素的数组下标，当轮循环结束之后，那这个变量存储的就是当前最小元素的下标，此时再执行交换操作即可。

```js
let arr = [23,231,213,3,324,45,56];

function selectSort(arr) {
	if(!Array.isArray(arr)) return;
	
	let length = arr.length;
	for(let i=0;i<length-1;i++) {
		let minIndex = i;
		for(let j=i+1;j<length;j++) {
			if(arr[minIndex] > arr[j]) {
				minIndex = j;
			}
		}
		[arr[i], arr[minIndex]] = [arr[minIndex], arr[i]];
	}
	
	return arr;
}

console.log(selectSort(arr))
```

当排序序列为已排序序列时，为最好的时间复杂度 O(n)。

插入排序的平均时间复杂度为 O(n²) ，最坏时间复杂度为 O(n²) ，空间复杂度为 O(1) ，是稳定排序。

详细资料可以参考： [《图解排序算法(一)》](https://www.cnblogs.com/chengxiao/p/6103002.html)