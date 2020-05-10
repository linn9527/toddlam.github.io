# TopN问题


<!--more-->

## 问题详情

> N 个数据 如何拿到 前 M 个最大数

## 问题分析

排序可以完全解决这个问题，排序时间复杂度可达 O(NlogN)，最简单的方式就是快排，然后取前 M 个值，或者最简单的冒泡排序也凑合，至少能出结果～～

较好的做法是，使用快排时加退出状态，当确认了第 M 个位置的数据时，后续就不需要递归下去了，这样可以减少一点点时间，最快可以达到 O(N)

根据 It works. 原则 我们已经把问题解决了～～哈哈哈哈。。。

👇 👇 👇 笑完咱继续

![没那么简单](https://gitee.com/toddlam/Colony/raw/master/pic/2020-05-10/ozQlrv.jpg "没那么简单")

Q：当我一个文本文件有 5M 的数据 但是不允许你用超过 5M 的内存，这时候，其实所有数据同时存在内存中是不可能的
  
就是要放弃排序的处理方式「..「

## 解决方式

So，假设内存中已读入了 M 个数据，剩余的数据每一个读取后，替换掉 M 个数据中的最小值，使用的时间大约为 M(N-M)+MlogM (一次 M 个数据排序，加上N-M个数据每次插入比较，大概吧

这种方式至少不需全部数据读取

但是每次读取一个数据后，都要找到当前数据的目标位置，会影响性能

所以，我们可以选择小顶堆这种数据结构

👇 👇 👇 此为小顶堆，大概长这样

![小顶堆](https://gitee.com/toddlam/Colony/raw/master/pic/2020-05-10/TH9op1.png "小顶堆")

利用小顶堆的特性，我们可以每次比较输入数据与堆顶数据，如果替换后不满足小顶堆的性质，则再自上而下进行一次构建

> 上代码

```php
<?php 
/* 变量什么的 Global 最好啦 */
$array = [8, 3, 9, 0, 4, 2, 5, 7, 1, 6];
$n = 5;

topn($array,$n);

/* 入口 */
function topn(&$array,$n){ 
    /* 创建前 n 个数组元素的小顶堆 */
	$m = count($array);
	echo "begin tree",PHP_EOL;
	echo json_encode($array),PHP_EOL;
	createTree($array,$n);
	echo json_encode($array),PHP_EOL;	
	echo "end tree",PHP_EOL;

	for($i = $n;$i < $m;$i++){
		if($array[0] >= $array[$i]) {
			continue;
		}
		$tmp = $array[$i]; $array[$i] = $array[0]; $array[0] = $tmp;
		echo "update tree",PHP_EOL;
		upToDown($array,0,$n);
		echo json_encode($array),PHP_EOL;
	}

    /* 顺便排个序 */
	echo "begin tree sort",PHP_EOL;
	echo json_encode($array),PHP_EOL;
	for($i = $n-1;$i >= 0;$i--){
		$tmp = $array[$i]; $array[$i] = $array[0]; $array[0] = $tmp;
		upToDown($array,0,$i);
	}
	echo json_encode($array),PHP_EOL;	
	echo "end tree sort",PHP_EOL;

}

/* 初始化前N个节点为最小堆 */
function createTree(&$array,$n){ 
    /* 从最小的二叉树从下往上开始构建最小堆 */
	for ($i = round(($n - 1) / 2); $i>=0; $i--){
		upToDown($array,$i,$n);
	}
}

/* 判断节点 $i 是否最小堆，左节点 left 右节点 right */
function upToDown(&$array,$i, $n){ 
	$left = $i * 2 + 1;
    $right = $left + 1;
    /* 没意义的 哈哈哈 */
	$point = $i; 
	echo "left:$left,right$right,i$i",PHP_EOL;
	if($left >= $n){
		return 'happy ending';
	}elseif($right >= $n){
		$point = $left;
	}else{
		$point = $array[$left] < $array[$right] ? $left : $right;
	}	
    /* 当前不是小顶堆 */
	if($array[$point] < $array[$i]){
		$tmp = $array[$point];
		$array[$point] = $array[$i];
		$array[$i] = $tmp;
		return upToDown($array,$point,$n);	
	}else{
		return 'happy ending';
	}
}
?>
```

时间大概为 (N-M)logM+MlogM 对比 M(N-M)+MlogM 当 M > 1 ，前者小于后者 

PS：当 M >= 1/2N 时，可以尝试用大顶堆找出前 N-M 个最小值然后排除掉哟（反正又不用排序「_「 哼）

