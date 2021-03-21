# 复习数据结构(4)--图的Java实现

### Ⅰ. 图的邻接矩阵存储实现

```java
package com.zhou.datastruct.graph;

//邻接矩阵存储结构
public class Graph {
    enum GraphKind {
        UDG,    //无向图
        DG,        //有向图
    }

    //图的顶点类
    class Vertex {
        public char lable;
        public boolean wasvisited;

        public Vertex(char lab) {
            lable = lab;
            wasvisited = false;
        }
    }

    private GraphKind kind;              //图的种类标志
    private final int MAX_VERTS = 20; //顶点最大个数
    private Vertex[] vertexList;      //顶点数组
    private int adjMat[][];           //邻接矩阵
    public int nVerts;                //顶点数

    public Graph() {
        this(GraphKind.UDG);
    }

    public Graph(GraphKind kind) {
        this.kind = kind;
        vertexList = new Vertex[MAX_VERTS];
        adjMat = new int[MAX_VERTS][MAX_VERTS];
        nVerts = 0;
        for (int i = 0; i < MAX_VERTS; i++) {
            for (int j = 0; j < MAX_VERTS; j++) {
                adjMat[i][j] = 0;
            }
        }
    }

    //加入顶点
    public void addVertex(char lab) {
        vertexList[nVerts++] = new Vertex(lab);
    }

    //加入边
    public void addEdage(int start, int end) {
        switch (kind) {
            case UDG:
                adjMat[start][end] = 1;
                adjMat[end][start] = 1;
                break;
            case DG:
                adjMat[start][end] = 1;
                break;
        }
    }

    //取得未被访问过的邻接点
    public int getAdjUnvisitedVertex(int v) {
        for (int i = 0; i < nVerts; i++) {
            if (adjMat[v][i] == 1 && vertexList[i].wasvisited == false) {
                return i;
            }
        }
        return -1;
    }

    //重置顶点状态
    public void resetVexStatus() {
        for (int i = 0; i < nVerts; i++) {
            vertexList[i].wasvisited = false;
        }
    }

}

```

### Ⅱ. 图的邻接表存储实现

```java
package com.zhou.datastruct.graph;

import java.lang.reflect.Array;

//邻接表存储结构
public class TGraph<E> {
    enum mGraphKind {
        UDG,	//无向图
        DG,		//有向图
    }

    // 邻接表中表对应的链表的顶点
    private class ENode<E> {
        private int adjvex;         // 邻接顶点序号
        private int weight;         //弧的权值
        private ENode nextadj;      //下一个邻接表结点
        ENode(int adjvex, int weight) {
            this.adjvex = adjvex;
            this.weight = weight;
        }
    }
    // 邻接表中表的顶点
    private class VNode<E> {
        private E name;        //顶点名称
        private ENode firstadj;      //链接表的第一个结点
    }
    private mGraphKind kind;              //图的种类标志
    private VNode<E>[] vexs;  // 顶点数组
    private final int MAX_VERTS = 20; //顶点最大个数
    private int numOfVexs;    // 顶点的实际数量
    private boolean[] visited;// 判断顶点是否被访问过
    public TGraph() {
        this(mGraphKind.UDG);
    }
    TGraph(mGraphKind kind) {
        this.kind = kind;
        vexs = (VNode<E>[]) Array.newInstance(VNode.class, MAX_VERTS);
    }
    //添加顶点
    public void insertVertex(E vertexName) {
        if (numOfVexs >= MAX_VERTS)
            System.out.println("超出最大个数");
        VNode<E> vex = new VNode<E>();
        vex.name = vertexName;
        vexs[numOfVexs++] = vex;
    }
    //添加边
    public void insertEdge(int v1, int v2, int weight) {
        if (v1 < 0 || v2 < 0 || v1 >= numOfVexs || v2 >= numOfVexs) {
            throw new ArrayIndexOutOfBoundsException();
        }
        ENode vex1 = new ENode(v2, weight);
        // 索引为index1的顶点没有邻接顶点
        if (vexs[v1].firstadj == null) {
            vexs[v1].firstadj = vex1;
        }
        // 索引为index1的顶点有邻接顶点
        else {        //链表插入操作
            vex1.nextadj = vexs[v1].firstadj;
            vexs[v1].firstadj = vex1;
        }
        switch (kind) {
            case UDG:
                ENode vex2 = new ENode(v1, weight);
                // 索引为index2的顶点没有邻接顶点
                if (vexs[v2].firstadj == null) {
                    vexs[v2].firstadj = vex2;
                }
                // 索引为index2的顶点有邻接顶点
                else {
                    vex2.nextadj = vexs[v2].firstadj;
                    vexs[v2].firstadj = vex2;
                }
                break; case DG: break;
        }
    }
    //重置顶点状态
    public void resetVexStatus() {
        for (int i = 0; i < numOfVexs; i++) {
            visited[i] = false;
        }
    }
    }
```

### Ⅲ. 邻接矩阵和邻接表如何实现图的存储
[数据结构-图的实现](https://www.bilibili.com/read/cv7484608)