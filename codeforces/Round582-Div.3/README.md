#####  [Codeforces Round #582 (Div. 3)](http://codeforces.com/contest/1213)题解

- A :  有n个芯片，可以免费的向左或向右移动2格，也可以花费一个金币向左或向右移动1格，问最后都移动到相同位置时，最少的花费。

  题解: 枚举移动到每个点的花费，再取个std::min即可。

  [AC源码](./A.cc)

- B: 有n个数，问在它的右边是否有比它小的数，有的话，对答案贡献加一。

  题解: 我们逆序遍历，维护最小值即可。

  [AC源码](./B.cc)

  

- C: 一本书有N页，问在1~N中，是m的倍数的数的个位求和，是多少。

  题解: m * 1 % 10 , m * 2 % 10 , . . . , m * 10 % 10, m * 11 % 10, m * 12 % 10, . . .

  我们发现 m * 1 % 10等于m * 11 % 10, m * 2 % 10等于m * 12 % 10, 依次类推。

  即m * 1 % 10 . . . 一直到 m * 10 % 10构成一个循环节。那么 n / m / 10 便是完整的循环节个数，n / m % 10便是最后一个循环节剩下的元素。

  找到循环节后，便可以轻松AC啦。

  [AC源码](./C.cc)

  

- D: 给定n个数，每个数可以除以2，记为一次操作。问最后k个数相等时，最少的操作数。

  题解: 记录每个数除以2后，需要的操作数。最后枚举k个相等的数分别是0~2e5时的操作数，取std::min便是答案。

  

- 

- 

  

  