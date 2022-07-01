- > 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [zhuanlan.zhihu.com](https://zhuanlan.zhihu.com/p/135407927)
- 一、图的概念
- 图是一种比较复杂的非线性数据结构
- 图（Graph）是由顶点的有穷非空集合和顶点之间边的集合组成，通常表示为：G(V,E)，其中，G 表示一个图，V 是图 G 中顶点的集合，E 是图 G 中边的集合。
- 图区分有向图和无向图
- 1、无向图（Undirected graphs）
- 如果图中任意两个顶点之间的边都是无向边，则称该图为无向图。
- 无向图相关概念：顶点、边
- ![](https://pic2.zhimg.com/v2-86b237f02c41991a56e2a8dd45aad035_r.jpg)
- 2、有向图（Directed graphs）
- 如果图中任意两个顶点之间的边都是有向边，则称该图为有向图
- 有向图相关概念：顶点、弧（弧头、弧尾 出度、入度）
- ![](https://pic2.zhimg.com/v2-db80979e6d5bad785082c3e1870dc569_r.jpg)
- 另外，有些图的边或弧具有与它相关的数值，这种与图的边或弧相关的数值叫做权（Weight），这是一个求最小生成树的关键值。
- 二、图的存储
- 图的存储结构要存储图中的各个顶点本身的信息，以及存储顶点与顶点之间的关系，比较复杂。图的存储结构有邻接矩阵、邻接表、十字链表、邻接多重表等。
- 三、图的遍历
- 图的遍历思想是从指定一个顶点出发，开始访问其他顶点，不能重复访问（每个顶点只能访问一次），直至所有点被访问。
- 1. 深度优先遍历（depth_first_search）
- 思想：从指定的第一个点开始，沿着向下的定点不重复的一直遍历下去，若走到尽头，退到上一个顶点，寻找附近有没有顶点，有而且不重复的话，接着遍历，否则退到上一个顶点。
- 2. 广度优先遍历（breadth_first_search）
- 思想：从指定的第一点开始，先寻找跟它直接相连的所有顶点，然后继续这几个顶点再次深入，每次搜寻的都是同一级别的。
- 四、最小生成树
- 1、普利姆（Prim）算法
- 思路：首先就是从图中的一个起点 a 开始，把 a 加入 U 集合，然后，寻找从与 a 有关联的边中，权重最小的那条边并且该边的终点 b 在顶点集合：（V-U）中，我们也把 b 加入到集合 U 中，并且输出边（a，b）的信息，这样我们的集合 U 就有：{a,b}，然后，我们寻找与 a 关联和 b 关联的边中，权重最小的那条边并且该边的终点在集合：（V-U）中，我们把 c 加入到集合 U 中，并且输出对应的那条边的信息，这样我们的集合 U 就有：{a,b,c} 这三个元素了，一次类推，直到所有顶点都加入到了集合 U
- 2、克鲁斯卡尔 (Kruskal) 算法
- 思路：1. 将图中的所有边都去掉；2. 将边按权值从小到大的顺序添加到图中，保证添加的过程中不会形成环；3. 重复上一步直到连接所有顶点，此时就生成了最小生成树。这是一种贪心策略。
- 终于到了最兴奋的代码时候，代码说明一切！奥利给！
- 树的邻接矩阵表示法 C++ 基本实现：
- ```
  #ifndef _CMAP_H_
  #define _CMAP_H_
  - #include <iostream>
  #include <vector>
  #include <assert.h>
  using namespace std;
  - //顶点
  class Node
  {
  public:
  	Node(char data = 0)
  	{
  m_cData = data;
  m_bVisited = false;
  	}
  	Node(const Node& node)
  	{
  if (this == &node)
  	return;
  *this = node;
  	}
  - Node& operator=(const Node& node)
  	{
  if (this == &node)
  	return *this;
  this->m_cData = node.m_cData;
  this->m_bVisited = node.m_bVisited;
  return *this;
  	}
  public:
  	char m_cData; //数据
  	bool m_bVisited; //是否访问
  };
  - //边
  class Edge
  {
  public:
  	Edge(int nodeIndexA = 0, int nodeIndexB = 0, int weightValue = 0) :
  m_iNodeIndexA(nodeIndexA),
  m_iNodeIndexB(nodeIndexB),
  m_iWeightValue(weightValue),
  m_bSelected(false) {}
  	Edge(const Edge& edge)
  	{
  if (this == &edge)
  	return;
  *this = edge;
  	}
  - Edge& operator=(const Edge& edge)
  	{
  if (this == &edge)
  	return *this;
  this->m_iNodeIndexA = edge.m_iNodeIndexA;
  this->m_iNodeIndexB = edge.m_iNodeIndexB;
  this->m_iWeightValue = edge.m_iWeightValue;
  this->m_bSelected = edge.m_bSelected;
  return *this;
  	}
  - public:
  	int m_iNodeIndexA; //头顶点
  	int m_iNodeIndexB; //尾顶点
  	int m_iWeightValue; //权重
  	bool m_bSelected; //是否被选中
  };
  - //图
  class CMap
  {
  private:
  	int m_iCapacity; //顶点总数
  	int m_iNodeCount; //当前顶点数量
  	Node *m_pNodeArray; //顶点集合
  	int *m_pMatrix; //邻接距阵
  	Edge *m_pEdgeArray; //最小生成树边集合
  public:
  	CMap(int iCapacity)
  	{
  m_iCapacity = iCapacity;
  m_iNodeCount = 0;
  m_pNodeArray = new Node[m_iCapacity];
  m_pMatrix = new int[m_iCapacity*m_iCapacity];
  memset(m_pMatrix, 0, m_iCapacity*m_iCapacity * sizeof(int));
  m_pEdgeArray = new Edge[m_iCapacity - 1];
  	}
  	~CMap(void)
  	{
  delete[]m_pNodeArray;
  delete[]m_pMatrix;
  delete[]m_pEdgeArray;
  	}
  - private:
  	//广度遍历具体实现
  	void breadthFirstTraverseImpl(vector<int> preVec)
  	{
  int val = 0;
  vector<int> curVec;
  for (int i = 0; i < (int)preVec.size(); i++)
  {
  	for (int j = 0; j < m_iCapacity; j++)
  	{
  		getValueFromMatrix(preVec[i], j, val);
  		if (/*1 == val*/0 != val)
  		{
  			if (m_pNodeArray[j].m_bVisited)
  				continue;
  			cout << m_pNodeArray[j].m_cData << " ";
  			m_pNodeArray[j].m_bVisited = true;
  			curVec.push_back(j);
  		}
  		else
  			continue;
  	}
  }
  - if (curVec.empty())
  	return;
  else
  	breadthFirstTraverseImpl(curVec);
  	}
  - //取最小边
  	int getMinEdge(const vector<Edge>& edgeVec)
  	{
  int min = 0, minEdge = 0;
  - for (int i = 0; i < (int)edgeVec.size(); i++)
  {
  	if (edgeVec[i].m_bSelected)
  		continue;
  	min = edgeVec[i].m_iWeightValue;
  	minEdge = i;
  }
  - for (int i = 0; i < (int)edgeVec.size(); i++)
  {
  	if (edgeVec[i].m_bSelected)
  		continue;
  	if (min > edgeVec[i].m_iWeightValue)
  	{
  		min = edgeVec[i].m_iWeightValue;
  		minEdge = i;
  	}
  }
  - if (0 == min)
  	return -1;
  - return minEdge;
  	}
  - bool isInSet(const vector<int>& nodeSet, int target)
  	{
  for (int i = 0; i < (int)nodeSet.size(); i++)
  {
  	if (nodeSet[i] == target)
  		return true;
  }
  - return false;
  	}
  - void mergeNodeSet(vector<int>& nodeSetA, const vector<int>& nodeSetB)
  	{
  for (size_t i = 0; i < (int)nodeSetB.size(); i++)
  {
  	nodeSetA.push_back(nodeSetB[i]);
  }
  	}
  public:
  	//添加顶点
  	void addNode(Node *node)
  	{
  assert(node);
  m_pNodeArray[m_iNodeCount].m_cData = node->m_cData;
  m_iNodeCount++;
  	}
  	//将顶点访问设置默认
  	void resetNode()
  	{
  for (int i = 0; i < m_iNodeCount; i++)
  	m_pNodeArray[i].m_bVisited = false;
  	}
  	//设置权重-有向图
  	bool setValueToMatrixForDirectedGraph(int row, int col, int val = 1)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  m_pMatrix[row*m_iCapacity + col] = val;
  return true;
  	}
  - //设置权重-无向图
  	bool setValueToMatrixForUndirectedGraph(int row, int col, int val = 1)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  m_pMatrix[row*m_iCapacity + col] = val;
  m_pMatrix[col*m_iCapacity + row] = val;
  return true;
  	}
  	//获取权重
  	bool getValueFromMatrix(int row, int col, int& val)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  val = m_pMatrix[row*m_iCapacity + col];
  return true;
  	}
  	//打印矩阵
  	void printMatrix()
  	{
  for (int i = 0; i < m_iCapacity; i++)
  {
  	for (int j = 0; j < m_iCapacity; j++)
  		cout << m_pMatrix[i*m_iCapacity + j] << " ";
  	cout << endl;
  }
  	}
  - //深度遍历
  	void depthFirstTraverse(int index)
  	{
  int val = 0;
  cout << m_pNodeArray[index].m_cData << " ";
  m_pNodeArray[index].m_bVisited = true;
  - for (int i = 0; i < m_iCapacity; i++)
  {
  	getValueFromMatrix(index, i, val);
  	if (/*1 == val*/0 != val)
  	{
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		depthFirstTraverse(i);
  	}
  	else
  		continue;
  }
  	}
  - //广度遍历
  	void breadthFirstTraverse(int index)
  	{
  cout << m_pNodeArray[index].m_cData << " ";
  m_pNodeArray[index].m_bVisited = true;
  - vector<int> curVec;
  curVec.push_back(index);
  - breadthFirstTraverseImpl(curVec);
  	}
  - //求最小生成树-普里斯算法
  	void primTree(int index)
  	{
  int val = 0;
  int iEdgeCount = 0;
  vector<Edge> edgeVec;//待选边集合
  - //从传入点开始找
  vector<int> nodeIndexVec;
  nodeIndexVec.push_back(index);
  //结束条件：边数=顶点数-1
  while (iEdgeCount < m_iCapacity - 1)
  {
  	//查找传入点的符合要求（权重不为0且目的点没有被访问）边
  	int row = nodeIndexVec.back();
  	cout << m_pNodeArray[row].m_cData << endl;
  	m_pNodeArray[row].m_bVisited = true;
  - for (int i = 0; i < m_iCapacity; i++)
  	{
  		getValueFromMatrix(row, i, val);
  		if (0 == val)
  			continue;
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		Edge edge(row, i, val);
  		edgeVec.push_back(edge);
  	}
  //取出最小边
  	int retIndex = getMinEdge(edgeVec);
  	if (-1 != retIndex)
  	{
  		//存储选中边
  		edgeVec[retIndex].m_bSelected = true;
  		m_pEdgeArray[iEdgeCount] = edgeVec[retIndex];
  		cout << m_pNodeArray[m_pEdgeArray[iEdgeCount].m_iNodeIndexA].m_cData << " - ";
  		cout << m_pNodeArray[m_pEdgeArray[iEdgeCount].m_iNodeIndexB].m_cData << " (";
  		cout << m_pEdgeArray[iEdgeCount].m_iWeightValue << ") " << endl;
  		iEdgeCount++;
  - int iNodeIndex = edgeVec[retIndex].m_iNodeIndexB;
  		//设置点被访问
  		m_pNodeArray[iNodeIndex].m_bVisited = true;
  		//存入目的点递归查找
  		nodeIndexVec.push_back(iNodeIndex);
  	}
  }
  - }
  - //最小生成树-克鲁斯卡尔算法
  	void kruskalTree()
  	{
  int val = 0;
  int edgeCount = 0;
  - //定义存放节点集合数组
  vector<vector<int> > nodeSets;
  - //第一步、取出所有边
  vector<Edge> edgeVec;
  for (int i = 0; i < m_iCapacity; i++)
  {
  	for (int j = i + 1; j < m_iCapacity; j++)
  	{
  		getValueFromMatrix(i, j, val);
  		if (0 == val)
  			continue;
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		Edge edge(i, j, val);
  		edgeVec.push_back(edge);
  	}
  }
  //第二步、从所有边中取出组成最小生成树的边
  //1、算法结束条件：边数=顶点数-1
  while (edgeCount < m_iCapacity - 1)
  {
  	//2、从边集合中找出最小边
  	int retIndex = getMinEdge(edgeVec);
  	if (-1 != retIndex)
  	{
  		edgeVec[retIndex].m_bSelected = true;
  - //3、找出最小边连接点
  		int nodeAIndex = edgeVec[retIndex].m_iNodeIndexA;
  		int nodeBIndex = edgeVec[retIndex].m_iNodeIndexB;
  - //4、找出点所在集合
  		bool nodeAInSet = false;
  		bool nodeBInSet = false;
  		int nodeAInSetLabel = -1;
  		int nodeBInSetLabel = -1;
  - for (int i = 0; i < (int)nodeSets.size(); i++)
  		{
  			nodeAInSet = isInSet(nodeSets[i], nodeAIndex);
  			if (nodeAInSet)
  				nodeAInSetLabel = i;
  		}
  - for (int i = 0; i < (int)nodeSets.size(); i++)
  		{
  			nodeBInSet = isInSet(nodeSets[i], nodeBIndex);
  			if (nodeBInSet)
  				nodeBInSetLabel = i;
  		}
  //5、根据点集合的不同做不同处理
  		if (nodeAInSetLabel == -1 && nodeBInSetLabel == -1)
  		{
  			vector<int> vec;
  			vec.push_back(nodeAIndex);
  			vec.push_back(nodeBIndex);
  			nodeSets.push_back(vec);
  		}
  		else if (nodeAInSetLabel == -1 && nodeBInSetLabel != -1)
  		{
  			nodeSets[nodeBInSetLabel].push_back(nodeAIndex);
  		}
  		else if (nodeAInSetLabel != -1 && nodeBInSetLabel == -1)
  		{
  			nodeSets[nodeAInSetLabel].push_back(nodeBIndex);
  		}
  		else if (-1 != nodeAInSetLabel && -1 != nodeBInSetLabel && nodeAInSetLabel != nodeBInSetLabel)
  		{
  			//mergeNodeSet(nodeSets[nodeAInSetLabel], nodeSets[nodeBInSetLabel]);
  			nodeSets[nodeAInSetLabel].insert(nodeSets[nodeAInSetLabel].end(),
  				nodeSets[nodeBInSetLabel].begin(),
  				nodeSets[nodeBInSetLabel].end());
  			for (int k = nodeBInSetLabel; k < (int)nodeSets.size() - 1; k++)
  			{
  				nodeSets[k] = nodeSets[k + 1];
  			}
  		}
  		else if (nodeAInSetLabel != -1 && nodeBInSetLabel != -1 && nodeAInSetLabel == nodeBInSetLabel)
  		{
  			continue;
  		}
  - m_pEdgeArray[edgeCount] = edgeVec[retIndex];
  		edgeCount++;
  cout << m_pNodeArray[edgeVec[retIndex].m_iNodeIndexA].m_cData << " - ";
  		cout << m_pNodeArray[edgeVec[retIndex].m_iNodeIndexB].m_cData << " (";
  		cout << edgeVec[retIndex].m_iWeightValue << ") " << endl;
  	}
  }
  	}
  };
  - #endif // !_CMAP_H_
  ```
- 测试树结构：
- ![](https://pic4.zhimg.com/v2-68a54ad2528a4326d7f4f2cfb7065c23_r.jpg)
- 测试函数 main.cpp：
- ```
  #include "CMap.hpp"
  #include <iostream>
  using namespace std;
  int main(int argc, char**argv)
  {
  - CMap *pMap = new CMap(6);
  - Node *pNodeA = new Node('A');
  	Node *pNodeB = new Node('B');
  	Node *pNodeC = new Node('C');
  	Node *pNodeD = new Node('D');
  	Node *pNodeE = new Node('E');
  	Node *pNodeF = new Node('F');
  - pMap->addNode(pNodeA);
  	pMap->addNode(pNodeB);
  	pMap->addNode(pNodeC);
  	pMap->addNode(pNodeD);
  	pMap->addNode(pNodeE);
  	pMap->addNode(pNodeF);
  - pMap->setValueToMatrixForUndirectedGraph(0, 1, 7);
  	pMap->setValueToMatrixForUndirectedGraph(0, 2, 1);
  	pMap->setValueToMatrixForUndirectedGraph(0, 3, 9);
  	pMap->setValueToMatrixForUndirectedGraph(1, 2, 2);
  	pMap->setValueToMatrixForUndirectedGraph(1, 4, 3);
  	pMap->setValueToMatrixForUndirectedGraph(2, 3, 11);
  	pMap->setValueToMatrixForUndirectedGraph(2, 4, 8);
  	pMap->setValueToMatrixForUndirectedGraph(2, 5, 4);
  	pMap->setValueToMatrixForUndirectedGraph(3, 5, 5);
  	pMap->setValueToMatrixForUndirectedGraph(4, 5, 15);
  - cout << "打印矩阵: " << endl;
  	pMap->printMatrix();
  	cout << endl;
  - pMap->resetNode();
  - cout << "深度优先遍历: " << endl;
  	pMap->depthFirstTraverse(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "广度优先遍历: " << endl;
  	pMap->breadthFirstTraverse(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "普里姆算法: " << endl;
  	pMap->primTree(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "克鲁斯卡尔算法: " << endl;
  	pMap->kruskalTree();
  	cout << endl;
  - pMap->resetNode();
  - system("pause");
  	return 0;
  }
  ```
- 执行结果：
- ![](https://pic1.zhimg.com/v2-5c767d7581ee17e91799edfb4a2f8b28_r.jpg)
- 图是一种比较复杂的非线性数据结构
- 图（Graph）是由顶点的有穷非空集合和顶点之间边的集合组成，通常表示为：G(V,E)，其中，G 表示一个图，V 是图 G 中顶点的集合，E 是图 G 中边的集合。
- 图区分有向图和无向图
- 1、无向图（Undirected graphs）
- 如果图中任意两个顶点之间的边都是无向边，则称该图为无向图。
- 无向图相关概念：顶点、边
- ![](https://pic2.zhimg.com/v2-86b237f02c41991a56e2a8dd45aad035_r.jpg){:height 478, :width 638}
- 2、有向图（Directed graphs）
- 如果图中任意两个顶点之间的边都是有向边，则称该图为有向图
- 有向图相关概念：顶点、弧（弧头、弧尾 出度、入度）
- ![](https://pic2.zhimg.com/v2-db80979e6d5bad785082c3e1870dc569_r.jpg)
- 另外，有些图的边或弧具有与它相关的数值，这种与图的边或弧相关的数值叫做权（Weight），这是一个求最小生成树的关键值。
- 二、图的存储
- 图的存储结构要存储图中的各个顶点本身的信息，以及存储顶点与顶点之间的关系，比较复杂。图的存储结构有邻接矩阵、邻接表、十字链表、邻接多重表等。
- 三、图的遍历
- 图的遍历思想是从指定一个顶点出发，开始访问其他顶点，不能重复访问（每个顶点只能访问一次），直至所有点被访问。
- 1. 深度优先遍历（depth_first_search）
- 思想：从指定的第一个点开始，沿着向下的定点不重复的一直遍历下去，若走到尽头，退到上一个顶点，寻找附近有没有顶点，有而且不重复的话，接着遍历，否则退到上一个顶点。
- 2. 广度优先遍历（breadth_first_search）
- 思想：从指定的第一点开始，先寻找跟它直接相连的所有顶点，然后继续这几个顶点再次深入，每次搜寻的都是同一级别的。
- 四、最小生成树
- 1、普利姆（Prim）算法
- 思路：首先就是从图中的一个起点 a 开始，把 a 加入 U 集合，然后，寻找从与 a 有关联的边中，权重最小的那条边并且该边的终点 b 在顶点集合：（V-U）中，我们也把 b 加入到集合 U 中，并且输出边（a，b）的信息，这样我们的集合 U 就有：{a,b}，然后，我们寻找与 a 关联和 b 关联的边中，权重最小的那条边并且该边的终点在集合：（V-U）中，我们把 c 加入到集合 U 中，并且输出对应的那条边的信息，这样我们的集合 U 就有：{a,b,c} 这三个元素了，一次类推，直到所有顶点都加入到了集合 U
- 2、克鲁斯卡尔 (Kruskal) 算法
- 思路：1. 将图中的所有边都去掉；2. 将边按权值从小到大的顺序添加到图中，保证添加的过程中不会形成环；3. 重复上一步直到连接所有顶点，此时就生成了最小生成树。这是一种贪心策略。
- 终于到了最兴奋的代码时候，代码说明一切！奥利给！
- 树的邻接矩阵表示法 C++ 基本实现：
- ```
  #ifndef _CMAP_H_
  #define _CMAP_H_
  - #include <iostream>
  #include <vector>
  #include <assert.h>
  using namespace std;
  - //顶点
  class Node
  {
  public:
  	Node(char data = 0)
  	{
  m_cData = data;
  m_bVisited = false;
  	}
  	Node(const Node& node)
  	{
  if (this == &node)
  	return;
  *this = node;
  	}
  - Node& operator=(const Node& node)
  	{
  if (this == &node)
  	return *this;
  this->m_cData = node.m_cData;
  this->m_bVisited = node.m_bVisited;
  return *this;
  	}
  public:
  	char m_cData; //数据
  	bool m_bVisited; //是否访问
  };
  - //边
  class Edge
  {
  public:
  	Edge(int nodeIndexA = 0, int nodeIndexB = 0, int weightValue = 0) :
  m_iNodeIndexA(nodeIndexA),
  m_iNodeIndexB(nodeIndexB),
  m_iWeightValue(weightValue),
  m_bSelected(false) {}
  	Edge(const Edge& edge)
  	{
  if (this == &edge)
  	return;
  *this = edge;
  	}
  - Edge& operator=(const Edge& edge)
  	{
  if (this == &edge)
  	return *this;
  this->m_iNodeIndexA = edge.m_iNodeIndexA;
  this->m_iNodeIndexB = edge.m_iNodeIndexB;
  this->m_iWeightValue = edge.m_iWeightValue;
  this->m_bSelected = edge.m_bSelected;
  return *this;
  	}
  - public:
  	int m_iNodeIndexA; //头顶点
  	int m_iNodeIndexB; //尾顶点
  	int m_iWeightValue; //权重
  	bool m_bSelected; //是否被选中
  };
  - //图
  class CMap
  {
  private:
  	int m_iCapacity; //顶点总数
  	int m_iNodeCount; //当前顶点数量
  	Node *m_pNodeArray; //顶点集合
  	int *m_pMatrix; //邻接距阵
  	Edge *m_pEdgeArray; //最小生成树边集合
  public:
  	CMap(int iCapacity)
  	{
  m_iCapacity = iCapacity;
  m_iNodeCount = 0;
  m_pNodeArray = new Node[m_iCapacity];
  m_pMatrix = new int[m_iCapacity*m_iCapacity];
  memset(m_pMatrix, 0, m_iCapacity*m_iCapacity * sizeof(int));
  m_pEdgeArray = new Edge[m_iCapacity - 1];
  	}
  	~CMap(void)
  	{
  delete[]m_pNodeArray;
  delete[]m_pMatrix;
  delete[]m_pEdgeArray;
  	}
  - private:
  	//广度遍历具体实现
  	void breadthFirstTraverseImpl(vector<int> preVec)
  	{
  int val = 0;
  vector<int> curVec;
  for (int i = 0; i < (int)preVec.size(); i++)
  {
  	for (int j = 0; j < m_iCapacity; j++)
  	{
  		getValueFromMatrix(preVec[i], j, val);
  		if (/*1 == val*/0 != val)
  		{
  			if (m_pNodeArray[j].m_bVisited)
  				continue;
  			cout << m_pNodeArray[j].m_cData << " ";
  			m_pNodeArray[j].m_bVisited = true;
  			curVec.push_back(j);
  		}
  		else
  			continue;
  	}
  }
  - if (curVec.empty())
  	return;
  else
  	breadthFirstTraverseImpl(curVec);
  	}
  - //取最小边
  	int getMinEdge(const vector<Edge>& edgeVec)
  	{
  int min = 0, minEdge = 0;
  - for (int i = 0; i < (int)edgeVec.size(); i++)
  {
  	if (edgeVec[i].m_bSelected)
  		continue;
  	min = edgeVec[i].m_iWeightValue;
  	minEdge = i;
  }
  - for (int i = 0; i < (int)edgeVec.size(); i++)
  {
  	if (edgeVec[i].m_bSelected)
  		continue;
  	if (min > edgeVec[i].m_iWeightValue)
  	{
  		min = edgeVec[i].m_iWeightValue;
  		minEdge = i;
  	}
  }
  - if (0 == min)
  	return -1;
  - return minEdge;
  	}
  - bool isInSet(const vector<int>& nodeSet, int target)
  	{
  for (int i = 0; i < (int)nodeSet.size(); i++)
  {
  	if (nodeSet[i] == target)
  		return true;
  }
  - return false;
  	}
  - void mergeNodeSet(vector<int>& nodeSetA, const vector<int>& nodeSetB)
  	{
  for (size_t i = 0; i < (int)nodeSetB.size(); i++)
  {
  	nodeSetA.push_back(nodeSetB[i]);
  }
  	}
  public:
  	//添加顶点
  	void addNode(Node *node)
  	{
  assert(node);
  m_pNodeArray[m_iNodeCount].m_cData = node->m_cData;
  m_iNodeCount++;
  	}
  	//将顶点访问设置默认
  	void resetNode()
  	{
  for (int i = 0; i < m_iNodeCount; i++)
  	m_pNodeArray[i].m_bVisited = false;
  	}
  	//设置权重-有向图
  	bool setValueToMatrixForDirectedGraph(int row, int col, int val = 1)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  m_pMatrix[row*m_iCapacity + col] = val;
  return true;
  	}
  - //设置权重-无向图
  	bool setValueToMatrixForUndirectedGraph(int row, int col, int val = 1)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  m_pMatrix[row*m_iCapacity + col] = val;
  m_pMatrix[col*m_iCapacity + row] = val;
  return true;
  	}
  	//获取权重
  	bool getValueFromMatrix(int row, int col, int& val)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  val = m_pMatrix[row*m_iCapacity + col];
  return true;
  	}
  	//打印矩阵
  	void printMatrix()
  	{
  for (int i = 0; i < m_iCapacity; i++)
  {
  	for (int j = 0; j < m_iCapacity; j++)
  		cout << m_pMatrix[i*m_iCapacity + j] << " ";
  	cout << endl;
  }
  	}
  - //深度遍历
  	void depthFirstTraverse(int index)
  	{
  int val = 0;
  cout << m_pNodeArray[index].m_cData << " ";
  m_pNodeArray[index].m_bVisited = true;
  - for (int i = 0; i < m_iCapacity; i++)
  {
  	getValueFromMatrix(index, i, val);
  	if (/*1 == val*/0 != val)
  	{
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		depthFirstTraverse(i);
  	}
  	else
  		continue;
  }
  	}
  - //广度遍历
  	void breadthFirstTraverse(int index)
  	{
  cout << m_pNodeArray[index].m_cData << " ";
  m_pNodeArray[index].m_bVisited = true;
  - vector<int> curVec;
  curVec.push_back(index);
  - breadthFirstTraverseImpl(curVec);
  	}
  - //求最小生成树-普里斯算法
  	void primTree(int index)
  	{
  int val = 0;
  int iEdgeCount = 0;
  vector<Edge> edgeVec;//待选边集合
  - //从传入点开始找
  vector<int> nodeIndexVec;
  nodeIndexVec.push_back(index);
  //结束条件：边数=顶点数-1
  while (iEdgeCount < m_iCapacity - 1)
  {
  	//查找传入点的符合要求（权重不为0且目的点没有被访问）边
  	int row = nodeIndexVec.back();
  	cout << m_pNodeArray[row].m_cData << endl;
  	m_pNodeArray[row].m_bVisited = true;
  - for (int i = 0; i < m_iCapacity; i++)
  	{
  		getValueFromMatrix(row, i, val);
  		if (0 == val)
  			continue;
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		Edge edge(row, i, val);
  		edgeVec.push_back(edge);
  	}
  //取出最小边
  	int retIndex = getMinEdge(edgeVec);
  	if (-1 != retIndex)
  	{
  		//存储选中边
  		edgeVec[retIndex].m_bSelected = true;
  		m_pEdgeArray[iEdgeCount] = edgeVec[retIndex];
  		cout << m_pNodeArray[m_pEdgeArray[iEdgeCount].m_iNodeIndexA].m_cData << " - ";
  		cout << m_pNodeArray[m_pEdgeArray[iEdgeCount].m_iNodeIndexB].m_cData << " (";
  		cout << m_pEdgeArray[iEdgeCount].m_iWeightValue << ") " << endl;
  		iEdgeCount++;
  - int iNodeIndex = edgeVec[retIndex].m_iNodeIndexB;
  		//设置点被访问
  		m_pNodeArray[iNodeIndex].m_bVisited = true;
  		//存入目的点递归查找
  		nodeIndexVec.push_back(iNodeIndex);
  	}
  }
  - }
  - //最小生成树-克鲁斯卡尔算法
  	void kruskalTree()
  	{
  int val = 0;
  int edgeCount = 0;
  - //定义存放节点集合数组
  vector<vector<int> > nodeSets;
  - //第一步、取出所有边
  vector<Edge> edgeVec;
  for (int i = 0; i < m_iCapacity; i++)
  {
  	for (int j = i + 1; j < m_iCapacity; j++)
  	{
  		getValueFromMatrix(i, j, val);
  		if (0 == val)
  			continue;
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		Edge edge(i, j, val);
  		edgeVec.push_back(edge);
  	}
  }
  //第二步、从所有边中取出组成最小生成树的边
  //1、算法结束条件：边数=顶点数-1
  while (edgeCount < m_iCapacity - 1)
  {
  	//2、从边集合中找出最小边
  	int retIndex = getMinEdge(edgeVec);
  	if (-1 != retIndex)
  	{
  		edgeVec[retIndex].m_bSelected = true;
  - //3、找出最小边连接点
  		int nodeAIndex = edgeVec[retIndex].m_iNodeIndexA;
  		int nodeBIndex = edgeVec[retIndex].m_iNodeIndexB;
  - //4、找出点所在集合
  		bool nodeAInSet = false;
  		bool nodeBInSet = false;
  		int nodeAInSetLabel = -1;
  		int nodeBInSetLabel = -1;
  - for (int i = 0; i < (int)nodeSets.size(); i++)
  		{
  			nodeAInSet = isInSet(nodeSets[i], nodeAIndex);
  			if (nodeAInSet)
  				nodeAInSetLabel = i;
  		}
  - for (int i = 0; i < (int)nodeSets.size(); i++)
  		{
  			nodeBInSet = isInSet(nodeSets[i], nodeBIndex);
  			if (nodeBInSet)
  				nodeBInSetLabel = i;
  		}
  //5、根据点集合的不同做不同处理
  		if (nodeAInSetLabel == -1 && nodeBInSetLabel == -1)
  		{
  			vector<int> vec;
  			vec.push_back(nodeAIndex);
  			vec.push_back(nodeBIndex);
  			nodeSets.push_back(vec);
  		}
  		else if (nodeAInSetLabel == -1 && nodeBInSetLabel != -1)
  		{
  			nodeSets[nodeBInSetLabel].push_back(nodeAIndex);
  		}
  		else if (nodeAInSetLabel != -1 && nodeBInSetLabel == -1)
  		{
  			nodeSets[nodeAInSetLabel].push_back(nodeBIndex);
  		}
  		else if (-1 != nodeAInSetLabel && -1 != nodeBInSetLabel && nodeAInSetLabel != nodeBInSetLabel)
  		{
  			//mergeNodeSet(nodeSets[nodeAInSetLabel], nodeSets[nodeBInSetLabel]);
  			nodeSets[nodeAInSetLabel].insert(nodeSets[nodeAInSetLabel].end(),
  				nodeSets[nodeBInSetLabel].begin(),
  				nodeSets[nodeBInSetLabel].end());
  			for (int k = nodeBInSetLabel; k < (int)nodeSets.size() - 1; k++)
  			{
  				nodeSets[k] = nodeSets[k + 1];
  			}
  		}
  		else if (nodeAInSetLabel != -1 && nodeBInSetLabel != -1 && nodeAInSetLabel == nodeBInSetLabel)
  		{
  			continue;
  		}
  - m_pEdgeArray[edgeCount] = edgeVec[retIndex];
  		edgeCount++;
  cout << m_pNodeArray[edgeVec[retIndex].m_iNodeIndexA].m_cData << " - ";
  		cout << m_pNodeArray[edgeVec[retIndex].m_iNodeIndexB].m_cData << " (";
  		cout << edgeVec[retIndex].m_iWeightValue << ") " << endl;
  	}
  }
  	}
  };
  - #endif // !_CMAP_H_
  ```
- 测试树结构：
- ![](https://pic4.zhimg.com/v2-68a54ad2528a4326d7f4f2cfb7065c23_r.jpg)
- 测试函数 main.cpp：
- ```
  #include "CMap.hpp"
  #include <iostream>
  using namespace std;
  int main(int argc, char**argv)
  {
  - CMap *pMap = new CMap(6);
  - Node *pNodeA = new Node('A');
  	Node *pNodeB = new Node('B');
  	Node *pNodeC = new Node('C');
  	Node *pNodeD = new Node('D');
  	Node *pNodeE = new Node('E');
  	Node *pNodeF = new Node('F');
  - pMap->addNode(pNodeA);
  	pMap->addNode(pNodeB);
  	pMap->addNode(pNodeC);
  	pMap->addNode(pNodeD);
  	pMap->addNode(pNodeE);
  	pMap->addNode(pNodeF);
  - pMap->setValueToMatrixForUndirectedGraph(0, 1, 7);
  	pMap->setValueToMatrixForUndirectedGraph(0, 2, 1);
  	pMap->setValueToMatrixForUndirectedGraph(0, 3, 9);
  	pMap->setValueToMatrixForUndirectedGraph(1, 2, 2);
  	pMap->setValueToMatrixForUndirectedGraph(1, 4, 3);
  	pMap->setValueToMatrixForUndirectedGraph(2, 3, 11);
  	pMap->setValueToMatrixForUndirectedGraph(2, 4, 8);
  	pMap->setValueToMatrixForUndirectedGraph(2, 5, 4);
  	pMap->setValueToMatrixForUndirectedGraph(3, 5, 5);
  	pMap->setValueToMatrixForUndirectedGraph(4, 5, 15);
  - cout << "打印矩阵: " << endl;
  	pMap->printMatrix();
  	cout << endl;
  - pMap->resetNode();
  - cout << "深度优先遍历: " << endl;
  	pMap->depthFirstTraverse(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "广度优先遍历: " << endl;
  	pMap->breadthFirstTraverse(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "普里姆算法: " << endl;
  	pMap->primTree(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "克鲁斯卡尔算法: " << endl;
  	pMap->kruskalTree();
  	cout << endl;
  - pMap->resetNode();
  - system("pause");
  	return 0;
  }
  ```
- 执行结果：
- ![](https://pic1.zhimg.com/v2-5c767d7581ee17e91799edfb4a2f8b28_r.jpg)
- --|END|--
- 欢迎搜索个人 WX 公众号 “IT 集装箱” 加关注，获取更多 IT 技术知识分享！
- 1、无向图（Undirected graphs）
- 如果图中任意两个顶点之间的边都是无向边，则称该图为无向图。
- 无向图相关概念：顶点、边
- ![][img-0]
- 2、有向图（Directed graphs）
- 如果图中任意两个顶点之间的边都是有向边，则称该图为有向图
- 有向图相关概念：顶点、弧（弧头、弧尾 出度、入度）
- /link
- 另外，有些图的边或弧具有与它相关的数值，这种与图的边或弧相关的数值叫做权（Weight），这是一个求最小生成树的关键值。
- 二、图的存储
- 图的存储结构要存储图中的各个顶点本身的信息，以及存储顶点与顶点之间的关系，比较复杂。图的存储结构有邻接矩阵、邻接表、十字链表、邻接多重表等。
- 三、图的遍历
- 图的遍历思想是从指定一个顶点出发，开始访问其他顶点，不能重复访问（每个顶点只能访问一次），直至所有点被访问。
- 1. 深度优先遍历（depth_first_search）
- 思想：从指定的第一个点开始，沿着向下的定点不重复的一直遍历下去，若走到尽头，退到上一个顶点，寻找附近有没有顶点，有而且不重复的话，接着遍历，否则退到上一个顶点。
- 2. 广度优先遍历（breadth_first_search）
- 思想：从指定的第一点开始，先寻找跟它直接相连的所有顶点，然后继续这几个顶点再次深入，每次搜寻的都是同一级别的。
- 四、最小生成树
- 1、普利姆（Prim）算法
-
- 2、克鲁斯卡尔 (Kruskal) 算法
- 思路：1. 将图中的所有边都去掉；2. 将边按权值从小到大的顺序添加到图中，保证添加的过程中不会形成环；3. 重复上一步直到连接所有顶点，此时就生成了最小生成树。这是一种贪心策略。
- 终于到了最兴奋的代码时候，代码说明一切！奥利给！
- 树的邻接矩阵表示法 C++ 基本实现：
- ```
  #ifndef _CMAP_H_
  #define _CMAP_H_
  - #include <iostream>
  #include <vector>
  #include <assert.h>
  using namespace std;
  - //顶点
  class Node
  {
  public:
  	Node(char data = 0)
  	{
  m_cData = data;
  m_bVisited = false;
  	}
  	Node(const Node& node)
  	{
  if (this == &node)
  	return;
  *this = node;
  	}
  - Node& operator=(const Node& node)
  	{
  if (this == &node)
  	return *this;
  this->m_cData = node.m_cData;
  this->m_bVisited = node.m_bVisited;
  return *this;
  	}
  public:
  	char m_cData; //数据
  	bool m_bVisited; //是否访问
  };
  - //边
  class Edge
  {
  public:
  	Edge(int nodeIndexA = 0, int nodeIndexB = 0, int weightValue = 0) :
  m_iNodeIndexA(nodeIndexA),
  m_iNodeIndexB(nodeIndexB),
  m_iWeightValue(weightValue),
  m_bSelected(false) {}
  	Edge(const Edge& edge)
  	{
  if (this == &edge)
  	return;
  *this = edge;
  	}
  - Edge& operator=(const Edge& edge)
  	{
  if (this == &edge)
  	return *this;
  this->m_iNodeIndexA = edge.m_iNodeIndexA;
  this->m_iNodeIndexB = edge.m_iNodeIndexB;
  this->m_iWeightValue = edge.m_iWeightValue;
  this->m_bSelected = edge.m_bSelected;
  return *this;
  	}
  - public:
  	int m_iNodeIndexA; //头顶点
  	int m_iNodeIndexB; //尾顶点
  	int m_iWeightValue; //权重
  	bool m_bSelected; //是否被选中
  };
  - //图
  class CMap
  {
  private:
  	int m_iCapacity; //顶点总数
  	int m_iNodeCount; //当前顶点数量
  	Node *m_pNodeArray; //顶点集合
  	int *m_pMatrix; //邻接距阵
  	Edge *m_pEdgeArray; //最小生成树边集合
  public:
  	CMap(int iCapacity)
  	{
  m_iCapacity = iCapacity;
  m_iNodeCount = 0;
  m_pNodeArray = new Node[m_iCapacity];
  m_pMatrix = new int[m_iCapacity*m_iCapacity];
  memset(m_pMatrix, 0, m_iCapacity*m_iCapacity * sizeof(int));
  m_pEdgeArray = new Edge[m_iCapacity - 1];
  	}
  	~CMap(void)
  	{
  delete[]m_pNodeArray;
  delete[]m_pMatrix;
  delete[]m_pEdgeArray;
  	}
  - private:
  	//广度遍历具体实现
  	void breadthFirstTraverseImpl(vector<int> preVec)
  	{
  int val = 0;
  vector<int> curVec;
  for (int i = 0; i < (int)preVec.size(); i++)
  {
  	for (int j = 0; j < m_iCapacity; j++)
  	{
  		getValueFromMatrix(preVec[i], j, val);
  		if (/*1 == val*/0 != val)
  		{
  			if (m_pNodeArray[j].m_bVisited)
  				continue;
  			cout << m_pNodeArray[j].m_cData << " ";
  			m_pNodeArray[j].m_bVisited = true;
  			curVec.push_back(j);
  		}
  		else
  			continue;
  	}
  }
  - if (curVec.empty())
  	return;
  else
  	breadthFirstTraverseImpl(curVec);
  	}
  - //取最小边
  	int getMinEdge(const vector<Edge>& edgeVec)
  	{
  int min = 0, minEdge = 0;
  - for (int i = 0; i < (int)edgeVec.size(); i++)
  {
  	if (edgeVec[i].m_bSelected)
  		continue;
  	min = edgeVec[i].m_iWeightValue;
  	minEdge = i;
  }
  - for (int i = 0; i < (int)edgeVec.size(); i++)
  {
  	if (edgeVec[i].m_bSelected)
  		continue;
  	if (min > edgeVec[i].m_iWeightValue)
  	{
  		min = edgeVec[i].m_iWeightValue;
  		minEdge = i;
  	}
  }
  - if (0 == min)
  	return -1;
  - return minEdge;
  	}
  - bool isInSet(const vector<int>& nodeSet, int target)
  	{
  for (int i = 0; i < (int)nodeSet.size(); i++)
  {
  	if (nodeSet[i] == target)
  		return true;
  }
  - return false;
  	}
  - void mergeNodeSet(vector<int>& nodeSetA, const vector<int>& nodeSetB)
  	{
  for (size_t i = 0; i < (int)nodeSetB.size(); i++)
  {
  	nodeSetA.push_back(nodeSetB[i]);
  }
  	}
  public:
  	//添加顶点
  	void addNode(Node *node)
  	{
  assert(node);
  m_pNodeArray[m_iNodeCount].m_cData = node->m_cData;
  m_iNodeCount++;
  	}
  	//将顶点访问设置默认
  	void resetNode()
  	{
  for (int i = 0; i < m_iNodeCount; i++)
  	m_pNodeArray[i].m_bVisited = false;
  	}
  	//设置权重-有向图
  	bool setValueToMatrixForDirectedGraph(int row, int col, int val = 1)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  m_pMatrix[row*m_iCapacity + col] = val;
  return true;
  	}
  - //设置权重-无向图
  	bool setValueToMatrixForUndirectedGraph(int row, int col, int val = 1)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  m_pMatrix[row*m_iCapacity + col] = val;
  m_pMatrix[col*m_iCapacity + row] = val;
  return true;
  	}
  	//获取权重
  	bool getValueFromMatrix(int row, int col, int& val)
  	{
  if (row < 0 || row >= m_iCapacity)
  	return false;
  if (col < 0 || col >= m_iCapacity)
  	return false;
  val = m_pMatrix[row*m_iCapacity + col];
  return true;
  	}
  	//打印矩阵
  	void printMatrix()
  	{
  for (int i = 0; i < m_iCapacity; i++)
  {
  	for (int j = 0; j < m_iCapacity; j++)
  		cout << m_pMatrix[i*m_iCapacity + j] << " ";
  	cout << endl;
  }
  	}
  - //深度遍历
  	void depthFirstTraverse(int index)
  	{
  int val = 0;
  cout << m_pNodeArray[index].m_cData << " ";
  m_pNodeArray[index].m_bVisited = true;
  - for (int i = 0; i < m_iCapacity; i++)
  {
  	getValueFromMatrix(index, i, val);
  	if (/*1 == val*/0 != val)
  	{
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		depthFirstTraverse(i);
  	}
  	else
  		continue;
  }
  	}
  - //广度遍历
  	void breadthFirstTraverse(int index)
  	{
  cout << m_pNodeArray[index].m_cData << " ";
  m_pNodeArray[index].m_bVisited = true;
  - vector<int> curVec;
  curVec.push_back(index);
  - breadthFirstTraverseImpl(curVec);
  	}
  - //求最小生成树-普里斯算法
  	void primTree(int index)
  	{
  int val = 0;
  int iEdgeCount = 0;
  vector<Edge> edgeVec;//待选边集合
  - //从传入点开始找
  vector<int> nodeIndexVec;
  nodeIndexVec.push_back(index);
  //结束条件：边数=顶点数-1
  while (iEdgeCount < m_iCapacity - 1)
  {
  	//查找传入点的符合要求（权重不为0且目的点没有被访问）边
  	int row = nodeIndexVec.back();
  	cout << m_pNodeArray[row].m_cData << endl;
  	m_pNodeArray[row].m_bVisited = true;
  - for (int i = 0; i < m_iCapacity; i++)
  	{
  		getValueFromMatrix(row, i, val);
  		if (0 == val)
  			continue;
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		Edge edge(row, i, val);
  		edgeVec.push_back(edge);
  	}
  //取出最小边
  	int retIndex = getMinEdge(edgeVec);
  	if (-1 != retIndex)
  	{
  		//存储选中边
  		edgeVec[retIndex].m_bSelected = true;
  		m_pEdgeArray[iEdgeCount] = edgeVec[retIndex];
  		cout << m_pNodeArray[m_pEdgeArray[iEdgeCount].m_iNodeIndexA].m_cData << " - ";
  		cout << m_pNodeArray[m_pEdgeArray[iEdgeCount].m_iNodeIndexB].m_cData << " (";
  		cout << m_pEdgeArray[iEdgeCount].m_iWeightValue << ") " << endl;
  		iEdgeCount++;
  - int iNodeIndex = edgeVec[retIndex].m_iNodeIndexB;
  		//设置点被访问
  		m_pNodeArray[iNodeIndex].m_bVisited = true;
  		//存入目的点递归查找
  		nodeIndexVec.push_back(iNodeIndex);
  	}
  }
  - }
  - //最小生成树-克鲁斯卡尔算法
  	void kruskalTree()
  	{
  int val = 0;
  int edgeCount = 0;
  - //定义存放节点集合数组
  vector<vector<int> > nodeSets;
  - //第一步、取出所有边
  vector<Edge> edgeVec;
  for (int i = 0; i < m_iCapacity; i++)
  {
  	for (int j = i + 1; j < m_iCapacity; j++)
  	{
  		getValueFromMatrix(i, j, val);
  		if (0 == val)
  			continue;
  		if (m_pNodeArray[i].m_bVisited)
  			continue;
  		Edge edge(i, j, val);
  		edgeVec.push_back(edge);
  	}
  }
  //第二步、从所有边中取出组成最小生成树的边
  //1、算法结束条件：边数=顶点数-1
  while (edgeCount < m_iCapacity - 1)
  {
  	//2、从边集合中找出最小边
  	int retIndex = getMinEdge(edgeVec);
  	if (-1 != retIndex)
  	{
  		edgeVec[retIndex].m_bSelected = true;
  - //3、找出最小边连接点
  		int nodeAIndex = edgeVec[retIndex].m_iNodeIndexA;
  		int nodeBIndex = edgeVec[retIndex].m_iNodeIndexB;
  - //4、找出点所在集合
  		bool nodeAInSet = false;
  		bool nodeBInSet = false;
  		int nodeAInSetLabel = -1;
  		int nodeBInSetLabel = -1;
  - for (int i = 0; i < (int)nodeSets.size(); i++)
  		{
  			nodeAInSet = isInSet(nodeSets[i], nodeAIndex);
  			if (nodeAInSet)
  				nodeAInSetLabel = i;
  		}
  - for (int i = 0; i < (int)nodeSets.size(); i++)
  		{
  			nodeBInSet = isInSet(nodeSets[i], nodeBIndex);
  			if (nodeBInSet)
  				nodeBInSetLabel = i;
  		}
  //5、根据点集合的不同做不同处理
  		if (nodeAInSetLabel == -1 && nodeBInSetLabel == -1)
  		{
  			vector<int> vec;
  			vec.push_back(nodeAIndex);
  			vec.push_back(nodeBIndex);
  			nodeSets.push_back(vec);
  		}
  		else if (nodeAInSetLabel == -1 && nodeBInSetLabel != -1)
  		{
  			nodeSets[nodeBInSetLabel].push_back(nodeAIndex);
  		}
  		else if (nodeAInSetLabel != -1 && nodeBInSetLabel == -1)
  		{
  			nodeSets[nodeAInSetLabel].push_back(nodeBIndex);
  		}
  		else if (-1 != nodeAInSetLabel && -1 != nodeBInSetLabel && nodeAInSetLabel != nodeBInSetLabel)
  		{
  			//mergeNodeSet(nodeSets[nodeAInSetLabel], nodeSets[nodeBInSetLabel]);
  			nodeSets[nodeAInSetLabel].insert(nodeSets[nodeAInSetLabel].end(),
  				nodeSets[nodeBInSetLabel].begin(),
  				nodeSets[nodeBInSetLabel].end());
  			for (int k = nodeBInSetLabel; k < (int)nodeSets.size() - 1; k++)
  			{
  				nodeSets[k] = nodeSets[k + 1];
  			}
  		}
  		else if (nodeAInSetLabel != -1 && nodeBInSetLabel != -1 && nodeAInSetLabel == nodeBInSetLabel)
  		{
  			continue;
  		}
  - m_pEdgeArray[edgeCount] = edgeVec[retIndex];
  		edgeCount++;
  cout << m_pNodeArray[edgeVec[retIndex].m_iNodeIndexA].m_cData << " - ";
  		cout << m_pNodeArray[edgeVec[retIndex].m_iNodeIndexB].m_cData << " (";
  		cout << edgeVec[retIndex].m_iWeightValue << ") " << endl;
  	}
  }
  	}
  };
  - #endif // !_CMAP_H_
  ```
- 测试树结构：
- ![][img-2]
- 测试函数 main.cpp：
- ```
  #include "CMap.hpp"
  #include <iostream>
  using namespace std;
  int main(int argc, char**argv)
  {
  - CMap *pMap = new CMap(6);
  - Node *pNodeA = new Node('A');
  	Node *pNodeB = new Node('B');
  	Node *pNodeC = new Node('C');
  	Node *pNodeD = new Node('D');
  	Node *pNodeE = new Node('E');
  	Node *pNodeF = new Node('F');
  - pMap->addNode(pNodeA);
  	pMap->addNode(pNodeB);
  	pMap->addNode(pNodeC);
  	pMap->addNode(pNodeD);
  	pMap->addNode(pNodeE);
  	pMap->addNode(pNodeF);
  - pMap->setValueToMatrixForUndirectedGraph(0, 1, 7);
  	pMap->setValueToMatrixForUndirectedGraph(0, 2, 1);
  	pMap->setValueToMatrixForUndirectedGraph(0, 3, 9);
  	pMap->setValueToMatrixForUndirectedGraph(1, 2, 2);
  	pMap->setValueToMatrixForUndirectedGraph(1, 4, 3);
  	pMap->setValueToMatrixForUndirectedGraph(2, 3, 11);
  	pMap->setValueToMatrixForUndirectedGraph(2, 4, 8);
  	pMap->setValueToMatrixForUndirectedGraph(2, 5, 4);
  	pMap->setValueToMatrixForUndirectedGraph(3, 5, 5);
  	pMap->setValueToMatrixForUndirectedGraph(4, 5, 15);
  - cout << "打印矩阵: " << endl;
  	pMap->printMatrix();
  	cout << endl;
  - pMap->resetNode();
  - cout << "深度优先遍历: " << endl;
  	pMap->depthFirstTraverse(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "广度优先遍历: " << endl;
  	pMap->breadthFirstTraverse(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "普里姆算法: " << endl;
  	pMap->primTree(0);
  	cout << endl;
  - pMap->resetNode();
  - cout << "克鲁斯卡尔算法: " << endl;
  	pMap->kruskalTree();
  	cout << endl;
  - pMap->resetNode();
  - system("pause");
  	return 0;
  }
  ```
- 执行结果：
- ![][img-3]
- --|END|--
- 欢迎搜索个人 WX 公众号 “IT 集装箱” 加关注，获取更多 IT 技术知识分享！
- [img-0]:data:image/png;base64,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
- [img-1]:data:image/png;base64,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
- [img-2]:data:image/png;base64,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
- [img-3]:data:image/png;base64,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2isefPDBeP/739/0uccee2w8+OCDMXPmzKxvg3FGOAcAACaMf/u3f4uf/exnbZdzzTXXxMUXX5z17TCOCOcAAMCEcOWVV8YRRxzRsfL++q//On7wgx9kfVuME8I5AAAw7n3oQx+Kgw8+uOPl7r///nHcccdlfXstO/TQQ2O33XaLXC5X2rb11lvHkUce2ZGx9bNnz45Pf/rTsddee1Xdv9NOO8WnP/3p+OhHP9pUuR/5yEdicHAwttpqq5rHvPWtb40PfvCD8epXv7qnz7RVwjkAADCuDQwMxJlnnjlq+2OPPRbHHXdcXHrppQ3LGBoaig996ENx1113jdr3la98JetbbMnBBx8cN954Y9x7770xderU0varr746rrnmmvjQhz7UdJmTJk0a8fMHPvCBuOCCC+Kv//qvY5ttthl1/K677hoXXHBBfOUrX4ldd9011TV22WWXuOyyy2Lp0qUxffr0EV8slDvttNPie9/7XnzpS1+KLbfcMuvH3ZBwDgAAjGs33HBD1e0HHnhg/Pd//3f867/+a9x99901z3/qqafi+OOPj1tuuSX+7u/+Ll588cVRx9xyyy1Z32ZTJk2aFOeff35ERPzoRz+K3XffvdQK/e1vfzsiIk455ZTYeeedU5e54447xs033xxf//rXY4stthix7zWveU1sv/32Nc/dcsstY4899kh1nRNOOCHy+Xzccccd8apXvapq6/nrXve6eO973xsREffcc0+87nWvy/qRN2SdcwAAYFybNm1a1e2FQqH070ceeSTe9KY3VT1ueHg4BgYGSse/8MILkc+PjFKvec1rOl7vxYsXx9FHHx0//vGP433ve19ERBxzzDFNj3MvP7/oH//xH2O//faLiIgNGzbEoYceWlpObmBgIF544YXYa6+94pxzzok77rijarnXXHNN/M///E/p54GBgVi3bl0sWLAgDjnkkJgzZ07Hn8mkSZPik5/8ZERE7LfffnH99dePOua+++6L73//+6Vu+d/5zndqlnfzzTfHu971ro7XsxXCOQAAMG7VC81f/epX4+STT45ddtklDj/88JrHTZ06Nd7xjnfEzTffHH//938f2267bdXj3vCGN4wIq52y8847x5vf/Oa46667Yu3atfHwww+P6qZdHFe9Zs2aWLdu3Yh9W2yxRen82NyqXN7Nv9547+OOO67mmPrnn38+/vznP8fatWsjIuKJJ56II444Iq666qp43/veF4sWLYq//OUvpeN/+9vf1mzBfs1rXhO//OUvR23/f//v/8Xpp59e+vkTn/hE7LbbbrFixYp4/PHH4+1vf3s89NBD8dvf/rZ0zMDAQHzuc5+LiIif/OQn8frXvz5mzZoVDz/8cPzmN78ZUf7q1atj//33j9/97ncdf9+a1VI4HxoaisHBwZaPS3t+o2ObKaeT1017TLt17+S9AwDARHTqqafW3PfhD384PvzhD6cq57vf/e6obb/97W/j8ssvjz/96U/x3HPPxdNPP931+7n22mvjLW95S0yfPn3E9l/84hcRm5eKu+6662Ljxo0j9q9fvz4iIrbbbrv4r//6r9hpp50iNrfEr1mzZsSxhx9+eHz+85+P66+/Ps4555y69dluu+1K4Tw2B/ajjz46zjvvvBgaGooPfOADpX0//OEPY+rUqSPGpW+//fZxyCGHxNq1a+Omm24aVf6LL74Ye+yxRzz44IMxbdq0OOecc2LDhg3xta99Lf73f/83fvzjH8cOO+wQF110Ufz5z3+OXC4XCxcujO222y5uuOGG+Na3vhVr166Nn/3sZ/HqV786vv/978edd9454hrPP/9819+3NKqG824Hv8HBwbauMTQ01LCu5cc02t6LkCtMAwBA73W6y/Lq1avjgx/8YKxYsSIOOuig+Nu//dvYd999Y4cddohCoRBPPfVU/OpXv4orrrgiHn744a7c0xNPPFG1lbnovvvui9WrV4/avt1228WSJUvizW9+c2nbrFmz4pZbbhkR5osTrO20007x+te/Pv70pz+NKmvZsmWxatWqqtcvFAoxf/782H333ePoo48ubT/rrLNi2223HdHqv+eee8YhhxwSTz/9dJx77rlV671mzZqYNGlSXH755bHtttvGFVdcEQ8++GD85je/iWuvvTb+9m//NubMmROnnnpqfPGLX4x3v/vd8cQTT8R3vvOdWLVqVdxzzz1xxhlnxNe+9rVYsGBB/NVf/dWo3gX9oGo4rwzP1QJt5bZuB89mr1de91Zb74vXLD6Pyn9XHtNs+Y2uXa3MWj8L/gAAMFq1ydta9Z73vCcefvjhuOKKK+KAAw6oesyUKVNin332iU984hNRKBTi0ksvja9+9atZP4Z4+ctfHtdff33sv//+ce+995YmgDvttNNqnvPWt7615njtT33qU3HZZZeVWuTLbbHFFnH00UfH5z//+dh///1H7PvjH/844ufyVvT169fH8uXLq14vn8/Hz372s9h5553jP/7jP2LFihWx1157xbp16+Kuu+6Khx56KF796lfHv//7v8f+++8fS5cujdWrV8f9998fERHnnXdeHHLIIfH73/8+9t5777jtttuyfktG32OagzoRPFsJ+OXXKd9Xq1W8Vt0r65u2/uXHVAb08nLS1qfVLwqK+1u5BwAAmMjuvPPOmD17dltlrFmzJvbbb7/40pe+FJ/4xCdSnzcwMBCf+MQn4oQTTog5c+bEgw8+mNlzeOaZZ+Lee++NrbfeOk477bS48sorY6uttmqrW/uOO+44oqX7Fa94RXziE5+Iz3zmM/Ha1742Vq9eHStWrIjdd9+97fq/+OKL8dWvfjX+4z/+I3baaadYsWJF/P3f/328//3vj0suuSRuv/32eOaZZ+LZZ5+NL37xi3HGGWfEfvvtN2I5uIceeij22GOPmD9/fqxduzZeeOGFmD9/fmbvSaWa4bzVVttqx1UL2JWaCdz1pO3OXq97e6Ox3tV+ruxpUO3fab8oELwBAKAzzjzzzLbC+XPPPRezZs2KoaGhEWuBN2PSpElx0003xRlnnFF35vBu2rhxY3z84x+PfffdNwYGBkrd2Fvt1n7//fePCObbbrttrFixInbYYYe477774swzz4xf/vKX8U//9E+lcL7zzjvHKaecMqKc4tj3HXbYIT73uc/FBz/4wRH7//mf/zleeOGFiIhIkiT+53/+Jx544IFIkqR0zHbbbRd/+ctf4tlnny2V+f73v7/hM1m7dm0sXLgw7rvvvkzek0qpJ4RL22pb7bhGgTRNF+5W6tmOyi8f6oXoakMAanV3b/Rz5flRpSt9rWck0AMAwEiV3aibtc8++8TPf/7zloN5uVNOOSVWrFgRN954YybPYuPGjXHbbbeVlhiLiJa7tZ988slx7rnnliZTe+655+Kcc86J9evXxx133BHPPvtsrFy5ckSA32GHHeLEE0+sWt622247Ynx60aJFi+KOO+6IJEli3rx58c53vrO0b++9946IiHe+853xjW98oxTOf/3rX0dExPLly6u2jL/yla+M733ve5HL5WLKlCn9G86rtYinCYZRJxymHR/eaWkCfprAXa/cegG5UWt9tWdd60uLtOPjAQCAlxQKhXj22Wdju+22a/rcr33ta/GOd7wj9txzzxHbjz766FLIrdZV/fnnny/Npn7ZZZeNaLm/9NJLY9q0aaNmU+/1Mylqp1v7q171qnjooYdKP59xxhmx++67x5o1a+LJJ58cdfwDDzwQO+ywQ3z605+O9evXl5Z2q2ennXYqtfS/853vrDq7/tSpU0d8eVI++/rw8PCIZdYiIl772tdm9uzrGRXOqwXyTkxs1kq38qGhobZnXW+2K329md8rx343u9RZM2PG04xPT/NFAgAATHQHHHBALFu2rOnzLr744li5cuWo7cXJxGoF/x/+8Ielf3/lK18Z1a3+zjvvjDe96U1ZP5aIiHjve98bd99994jAXgyvu+22W7z73e+uOoN6RMTMmTPj0ksvjWeeeaa0bcWKFSOO+frXvx5Lly4tfRnxvve9L7761a/G8uXL46CDDoqnnnoqdV0/8pGPxFlnnRW77rpr7LjjjnHFFVfElltuGVdddVVcdtllpeOKX4y8/OUvj6OOOipmzZo1opwdd9wx68deVepu7fVaZusF2sqW32a7uHdinfRm1JuJvVqX82rd19ME70gZwKs96zTPCAAA2OS5556LK664Io477rjU59x+++3xute9ruq+/fffP373u9/FlltuWbVF/h/+4R/ilFNOiSRJ4rzzzhu1/2Uve1nkcrkR46azcvzxx9fct9dee8UXv/jFuuffcsstcdttt9XsCfDggw/GqlWrYsstt4y1a9fG4sWL41vf+lbMmDEjPvaxj8U3v/nNpup7zz33xD333BPnnXdeaUm2ww47LC699NLScIHilwW77rprnHzyyVk/4tRSh/NoEMIrf25mFvNG12hHqy3ntSZ1iwYTx9UaM16+vzxgV/tyImp0l682azsAANDYaaedFu9+97tjypQpqY4/+eST4+qrr6667wc/+EHdc7fYYouGM7Pvvvvumc7efuutt5bC63PPPVfavs0228SXv/zlWL9+fZx66qkNy9lhhx0in8/Hxo0b495774299tordR3OO++8ql9elFu2bFnss88+I7btsssu8alPfar080477RRXX311zJkzJ+68887S9kZjzvtNU+G8lTHclef3Olh2eix2o3Hj9bqZ1+rq3mhG91rHAgAA6R100EFx/fXXxxvf+MaGxy5fvjx1kG/Fpz71qfiXf/mXzJ7Fe97znnjLW94S+Xw+br/99tJ68K985Stjv/32i3Xr1sXjjz8+Iuw2cu6558a+++5batEut9VWW8VRRx0V22+/faxevTp22WWXWLNmTSxevHhUD4JcLhf5fD42bNgQw8PD8Y53vCN+9atflfYvWLAgtt5661izZk1sv/32cdddd8U+++wTN954Y8yZM2dEWWN6zHk9aVrOG51f2bLezaDZqXXBG83WnuY5NbO90XOu9W+hHQAA6jvssMPi+9//frz97W/PtB7dDP61/PrXv44DDjgg1bGTJ0+OO+64o+Fxv/nNb+LAAw+MiIhLLrkkpkyZEltssUVpfy6Xi/e85z1x6qmnxvbbbx833nhjnH/++TFv3rw4/PDD4/HHH49///d/Lx0/Y8aM+OpXvxpbb711fOpTn4qHHnooNmzYUNq/3377xac//elYtWpVXH/99fGxj30sli1bFtddd1186UtfihtvvDE+85nPRIzHMefVlglrR5rJ1NpVrct4sy3aafalqUe0EZrrBe96k8F16rpZnQ8AAN2UdTCPiPjDH/7Q82suXLgwbr311th6661H7dtmm23ife97X0yePHnE9ueeey5+9KMfjZgBvdyTTz4Zb3/720ut03/+858jNrfAv+9974vPfe5zsffee8eKFSvii1/8Ytxxxx2xevXq+NjHPhb33ntvzJ8/P26++eYYGhqK008/PebPnx9PP/10LFq0KDZu3BirV68uXXv77bePH/zgB7HlllvG5ZdfHjvssEOpHt/5zndit912i2nTppW2j6sx52knZ2tWv8wyXj6GO8065s0+j1buLc35ab90AAAARqrscv2f//mfccstt8QJJ5wQe++9d+RyuYiIOOSQQ+LXv/51qVW40xYuXNjze7/iiivihhtuiK222mrE9gMPPDDOPvvsmDx5cume169fH7fffnsceOCBMTg4GF/4whdK64dXeuGFFyI2TyB31FFHxVFHHRXvfOc7R6ynftZZZ8WyZcvivvvuiyeeeCJi8+zrP/zhD+N73/teDA8Px8477xyXX355/Nd//Vc8/fTT8dBDD5XKzuVy8e1vfzumTZsWv/nNb2JoaCgOOuigEfX4+Mc/Hvvuu29p3Pu4GnPe6vjxtPvSBss066ynaZFvNsDWan2vd3yjFvpG95BmjfU049bbkfX5AADQLcW/VVetWhWHHXZYaWbv//zP/xx17PLly+N3v/tdV+rx9NNPZ3L/q1atKv177733jq997Wtx1FFHxbp16+Ib3/hG/PrXv44f/OAHsXHjxjjllFPi8MMPj8985jNx1VVXxU9/+tM4+eST4/bbbx9V7qRJk+Lb3/52HHzwwbFixYq48sor43e/+12cfPLJseuuu0aSJHHXXXfFs88+Wzrnuuuui/e///1x9dVXx5QpU+KnP/1pLFmyJO6///5YsWJFaRx8bO6i/u53vzsee+yxOOecc+Lxxx8fsYRbbP6S4Le//W3k8y/F3HEx5rxRwGp24rJaXZ3TBPRmwl5ly3balv1a477TjAdPU99W76HR8wMAANI7++yz45Of/GRcf/31VfcPDAxELpeLQqEQjz/+eBQKhREtwJ1w/fXXZ7aM2nbbbRdHH310HH/88aVs8fOf/zwuv/zyWLVqVTz++OOlY5MkiQULFsTixYvjjDPOiMMPPzwOP/zwuPvuu+PKK6+Mq666Kh555JGIiNi4cWN84AMfiEMOOST+8pe/xHPPPRePPfbYiG7lL3vZy+LAAw+MAw44IA444IB485vfHLvttlu8973vjcsuuywOP/zweNvb3hYnnXRSPPLIIyPC+VNPPRUf/ehH4w1veEM8+uijcc8998S73/3uUfeXJEnNZd36XW7KlClJkiSRJEkk826K1afOyLpOAAAAfeF1r3td/OIXv+homXvssUcUCoVUx86aNSte/epXx2OPPRZ33XVX3WOLM5XffffdsXr16hH7dtxxx/jGN74R//AP/xDbbrttvPjii3HjjTfGNddcE/fee288+uijcf/998d2220Xjz/+eKxbty7e+973xk033RSxebb1OXPmxPHHHx+HHXZYvOxlL4skSeLnP/95HHXUUaVx4VtuuWVsueWW8eyzz8bf/M3fxE9+8pOIiHjsscfiVa96VcTmAH3ffffFrbfeGhdffHH88pe/jJ122im+/OUvxz/+4z/GNttsE+vWrYuf/vSn8eMf/ziuuOKK0pcZO+20UxQKhXjqqafiC1/4Qpx99tnxve99L0477bT44x//GLG5m3757O61VN5jWkmSxPDwcFufgekXrIrkwsHI5XKlV1OztQMAAEwkDz/8cHzrW9+K//N//k9HynvXu96VOphHRPz+979PfewNN9xQc99TTz0V2267bTz/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