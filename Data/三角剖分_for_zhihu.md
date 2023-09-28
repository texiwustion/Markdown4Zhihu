- x ACM训练 - 第二遍三角剖分求面积交  
	- 策略：分解  
	- 技术1：三角形与圆的面积交  
		- 三角形与圆的面积交由 有向扇形面积 和 有向三角形面积组成（继续分解）  
		- 重要的点有：圆心 O，线段AB的两个端点，线段AB与圆的可能交点，O到AB的垂心  
		- 通过几个点，能求出所需的面积（继续分解）  
	- 技术2：多边形剖分为三角形  
		- 每次迭代一边，多个三角形的有向面积相加，为多边形的有向面积  
	- 技巧1：圆心O到AB的垂心  
		- 得到AB rotate 90度之后的向量，与圆心O表示一条通过O的关于AB的垂线  
		- 求这条垂线与直线AB的交点  
	- 技巧2：圆心O到AB的最短距离  
		- 如果垂心E在AB上，那么最短距离为|OE|  
		- 如果垂心E不在AB上，那么最短距离为 min(|OA|, |OB|)  
	- 技巧3：圆与线段AB是否有交点  
		- 求出圆心到AB的最短距离d  
		- 如果 R< d，那么AB和圆没有交点  
		- 如果 R =d，那么AB和圆只有一个交点  
		- 如果AB的端点都在圆外 且 R>d，AB和圆有两个交点  
	- 技巧4：当AB和圆一定有两个交点时，求这两个交点  
		- 根据某个性质，可知交点pa, pb 距离垂心E的距离是相同的  
		- 在 R, |OE|, len 组成的直角三角形中，用勾股定理求出 len  
		- 求出 (a-b) 和 (b-a) 的单位向量，用 E + 单位向量 * len 得到两个交点  
	- 技巧5：求旋转向量  
		-  <img src="https://www.zhihu.com/equation?tex=\pi = \arccos (-1)" alt="\pi = \arccos (-1)" class="ee_img tr_noresize" eeimg="1">   
		-  <img src="https://www.zhihu.com/equation?tex=(a, b)" alt="(a, b)" class="ee_img tr_noresize" eeimg="1">  旋转  <img src="https://www.zhihu.com/equation?tex=\theta" alt="\theta" class="ee_img tr_noresize" eeimg="1">  之后变成  <img src="https://www.zhihu.com/equation?tex=(a\cos\theta-b\sin\theta, a\sin\theta + b\cos\theta)" alt="(a\cos\theta-b\sin\theta, a\sin\theta + b\cos\theta)" class="ee_img tr_noresize" eeimg="1">   
		-  
		  ``` c++
		  		  Point rotate(Point a, double b) {
		  		      return Poitn(a.x * cos(b) - a.y * sin(b), a.y * sin(b) + a.x * cos(b));
		  		  }
		  ```
	- 技巧6：判断点p是否在线段ab上  
		- pa 与 pb 叉积为0，且pa与pb点积小于0 （共线且在ab之间）  
		-  
		  ``` c++
		  		  bool exGetD(Point p, Point a, Point b) {
		  		      return fabs(cross(a - p, b - p)) < eps && dot(a - p, b - p) <= 0;
		  		  }
		  ```
	- 技巧7：获取直线ab与cd的交点  
		- 以a为基准，求关于方向向量u的缩量  
		-  
		  ``` c++
		  		  Point getNode(Point a, Point u, Point b, Point v) {
		  		      double t = (a - b) * v / (u * v);
		  		      return a + u * t;
		  		  }
		  ```
	- 技巧8：当三角形和圆一定只有扇形交的时候，求扇形面积  
		- 求出AB关于O的角度，如果b在a的逆时针方向取正，否则取负  
		- 然后套公式  <img src="https://www.zhihu.com/equation?tex=S={1\over 2} R^2 \theta" alt="S={1\over 2} R^2 \theta" class="ee_img tr_noresize" eeimg="1">   
		-  
		  ``` c++
		  		  double S_sector(Point a, Point b) {
		  		      double angle = acos(dot(a, b) / len(a) / len(b));
		  		      if (cross(a, b) < 0) angle = -angle;
		  		      return R * R * angle / 2;
		  		  }
		  ```
	- 技巧9：求解环境优化——圆心平移  
		- 输入圆心之后，对每一个点进行 `- o` 的操作  
		- 随后令 o = Point(0, 0)  
	- 技巧10：圆心三角形与圆面积交  
		- 假定AB是逆时针输入，之后用 fabs(S) 来输出面积即可  
		- 如果 o,a,b 共线，直接返回0  
		- 求出 |OA| 和 |OB| ，存储在 da，db  
		- 如果da和db都<=R，那么求一个三角形的面积，方向 a*b/2  
		- 否则，用 exGetD 求出最短距离d，与pa，pb  
		- 如果 R<=d，那么求一个扇形面积 sector(a, b)  
		- 否则，至少有一个点在圆内，判断 R 与 da，db，分类计算。计算扇形面积与三角形面积和  