- x ACM训练 - 第二遍Andrew算法求凸包  
	- 策略：递推  
	- 技术：  
		- 对点集进行双关键字排序后，可得到凸包上的两点  
		- 两点作为首尾，先用单调栈维护边逆时针旋转趋势，求出下凸包，后逆向操作求出上凸包  
	- 技巧：  
		- 双关键字排序  
			- 先重载点结构的小于号运算符  
			-  
			  ``` c++
			  			  bool operator<(Point& a, Point& b) {
			  			      return a.x == b.x ? a.y < b.y : a.x < b.x;
			  			  }
			  ```
			- 后使用库函数 sort 对点集进行排序  
		- 单调栈维护下凸包  
			- 单调要求: i > 1 时 cross(s[top - 1], s[top], p[i]) > 0  
			-  
			  ``` c++
			  			  for (int i = 1; i <= n; ++i) {
			  			      while (top > 1 && cross(s[top - 1], s[top], p[i]) <= 0) --top;
			  			      s[++top] = p[i];
			  			  }
			  ```
		- 单调栈维护上凸包  
			-  
			  ``` c++
			  			  top = t;
			  			  for (int i = n - 1; i >= 1; --i) { // 前面下凸包包含了 1 .. n，这里从 n - 1开始考虑加入
			  			  // i 仍能 = 1 是为了闭合方便 [1] = [n + 1] = 1
			  			      while (top > t && cross(s[top - 1], s[top], p[i]) <= 0) --top;
			  			      s[++top] = p[i];
			  			  }
			  ```