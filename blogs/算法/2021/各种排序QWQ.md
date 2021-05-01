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

详细资料可以参考： [《图解排序算法(一)](https://www.cnblogs.com/chengxiao/p/6103002.html)



> 插入排序

直接插入排序基本思想是每一步将一个待排序的记录，插入到前面已经排好序的有序序列中去，直到插完所有元素为止。

插入排序核心--扑克牌思想： 就想着自己在打扑克牌，接起来一张，放哪里无所谓，再接起来一张，比第一张小，放左边， 继续接，可能是中间数，就插在中间....依次

代码实现：

```js
let arr = [5,3,4,1,2];

function insertSort(arr) {
	if(!Array.isArray(arr)) return;
	
	let length = arr.length;
	for(let i=1;i<length;i++) {
		let temp = arr[i];
		let j = i;
		while(j-1>=0 && arr[j-1]>temp) {
			arr[j] = arr[j-1];
			j--;
		}
		
		arr[j] = temp;
	}
	
	return arr;
}

console.log(insertSort(arr))
```

当排序序列为已排序序列时，为最好的时间复杂度 O(n)。

插入排序的平均时间复杂度为 O(n²) ，最坏时间复杂度为 O(n²) ，空间复杂度为 O(1) ，是稳定排序。

详细资料可以参考： [《图解排序算法(一)》](https://www.cnblogs.com/chengxiao/p/6103002.html)



> 快速排序

快速排序的基本思想是通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据 都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，以此达到整个数据变成有序序列。

代码实现：

```js
function quickSort(array) {
	function recursion(arr, left = 0, right = arr.length - 1) {
		if (left >= right) {
			return;
		}
		let i = left;
		let j = right;
		const baseValue = arr[i];
		while (i < j) {
			while (i < j && baseValue <= arr[j]) {
				j--;
			}
			// arr[i] = arr[j];
			while (i < j && baseValue >= arr[i]) {
				i++;
			}
			// arr[j] = arr[i];
			[arr[i], arr[j]] = [arr[j], arr[i]]
		}
		arr[j] = baseValue;
		recursion(arr, left, i - 1);
		recursion(arr, i + 1, right);
	}
	let nowArr = array.slice(0);
	recursion(nowArr);
	return nowArr;
}
console.log(quickSort([4, 8, 1, 9, 2, 7, 6, 4, 5, 2, 14, 12, 4]));
```

这一种方法是填空法，首先将第一个位置的数作为枢纽值，然后 end 指针向前移动，当遇到比枢纽值小的值或者 end 值 等于 start 值的时候停止，然后将这个值填入 start 的位置，然后 start 指针向后移动，当遇到比枢纽值大的值或者 start 值等于 end 值的时候停止，然后将这个值填入 end 的位置。反复循环这个过程，直到 start 的值等于 end 的 值为止。将一开始保留的枢纽值填入这个位置，此时枢纽值左边的值都比枢纽值小，枢纽值右边的值都比枢纽值大。然后在递 归左右两边的的序列。

当每次换分的结果为含 ⌊n/2⌋和 ⌈n/2⌉−1 个元素时，最好情况发生，此时递归的次数为 logn，然后每次划分的时间复杂 度为 O(n)，所以最优的时间复杂度为 O(nlogn)。一般来说只要每次换分都是常比例的划分，时间复杂度都为 O(nlogn)。

当每次换分的结果为 n-1 和 0 个元素时，最坏情况发生。划分操作的时间复杂度为 O(n)，递归的次数为 n-1，所以最坏 的时间复杂度为 O(n²)。所以当排序序列有序的时候，快速排序有可能被转换为冒泡排序。

快速排序的空间复杂度取决于递归的深度，所以最好的时候为 O(logn)，最坏的时候为 O(n)。

快速排序的平均时间复杂度为 O(nlogn) ，最坏时间复杂度为 O(n²) ，空间复杂度为 O(logn) ，不是稳定排序。

详细资料可以参考： [《图解排序算法(五)之快速排序——三数取中法》](https://www.cnblogs.com/chengxiao/p/6262208.html) [《关于快速排序的四种写法》](https://segmentfault.com/a/1190000004410119#articleHeader2) [《快速排序的时间和空间复杂度》](https://harttle.land/2015/09/27/quick-sort.html) [《快速排序最好，最坏，平均复杂度分析》](https://blog.csdn.net/weshjiness/article/details/8660583) [《快速排序算法的递归深度》](https://blog.csdn.net/qq_33758761/article/details/76782610)

> 归并排序

归并排序是利用归并的思想实现的排序方法，该算法采用经典的分治策略。递归的将数组两两分开直到只包含一个元素，然后 将数组排序合并，最终合并为排序好的数组。

```js
/**
 * 归并排序
 */

let arr = [3, 2, 52, 13, 436, 6];

function mergeSort(array) {

	let length = array.length;

	// 如果不是数组或者数组长度小于等于0，直接返回，不需要排序 
	if (!Array.isArray(array)) return;

	if (length === 1) {
		return array;
	}

	let mid = parseInt(length >> 1), // 找到中间索引值
		left = array.slice(0, mid), // 截取左半部分
		right = array.slice(mid, length); // 截取右半部分

	return merge(mergeSort(left), mergeSort(right)); // 递归分解后，进行排序合并
}


function merge(leftArray, rightArray) {

	let result = [],
		leftLength = leftArray.length,
		rightLength = rightArray.length,
		il = 0,
		ir = 0;

	// 左右两个数组的元素依次比较，将较小的元素加入结果数组中，直到其中一个数组的元素全部加入完则停止
	while (il < leftLength && ir < rightLength) {
		if (leftArray[il] < rightArray[ir]) {
			result.push(leftArray[il++]);
		} else {
			result.push(rightArray[ir++]);
		}
	}

	// 如果是左边数组还有剩余，则把剩余的元素全部加入到结果数组中。
	while (il < leftLength) {
		result.push(leftArray[il++]);
	}

	// 如果是右边数组还有剩余，则把剩余的元素全部加入到结果数组中。
	while (ir < rightLength) {
		result.push(rightArray[ir++]);
	}

	return result;
}


console.log(mergeSort(arr))
```

归并排序将整个排序序列看成一个二叉树进行分解，首先将树分解到每一个子节点，树的每一层都是一个归并排序的过程，每 一层归并的时间复杂度为 O(n)，因为整个树的高度为 lgn，所以归并排序的时间复杂度不管在什么情况下都为O(nlogn)。

归并排序的空间复杂度取决于递归的深度和用于归并时的临时数组，所以递归的深度为 logn，临时数组的大小为 n，所以归 并排序的空间复杂度为 O(n)。

归并排序的平均时间复杂度为 O(nlogn) ，最坏时间复杂度为 O(nlogn) ，空间复杂度为 O(n) ，是稳定排序。

详细资料可以参考： [《图解排序算法(四)之归并排序》](https://www.cnblogs.com/chengxiao/p/6194356.html) [《归并排序的空间复杂度？》](https://www.zhihu.com/question/27274006)