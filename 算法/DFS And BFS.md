## DFS BFS Dijkstra

[其中部分内容的原制作者链接](https://www.bilibili.com/video/av25761720/?spm_id_from=333.788.videocard.1)

给定一个无向图

![](https://user-images.githubusercontent.com/36192496/75672007-c4951b00-5cba-11ea-9c4b-44e60f55e601.png)

## 预先准备
```
import queue,collections

graph={
    "A":["B","C"],
    "B":["A","C","D"],
    "C":["A","B","D","E"],
    "D":["B","C","E","F"],
    "E":["C","D"],
    "F":["D"],
}

```

## BFS

可以结合树的层序遍历，图其实也是一次性将当前节点相连且未访问的点放入队列，当前节点则是每次从队列出来的点

```


def BFS(graph,s):
    '''
    Queue
    '''
    que=queue.Queue()
    seen=set()
    que.put(s)
    seen.add(s)
    while not que.empty():
        vertex=que.get()
        nodes=graph[vertex]
        for w in nodes:
            if w not in seen:
                que.put(w)
                seen.add(w)
        print(vertex)

```

## DFS

将栈顶节点的相邻节点放入栈中

```
def DFS(graph,s):
    '''
    Stack
    '''
    stack=[s]
    seen=set([s])
    while stack:
        vertix=stack.pop()
        nodes=graph[vertix]
        for w in nodes:
            if w not in seen:
                stack.append(w)
                seen.add(w)
        print(vertix)




```


## Dijkstra

计算graph中各个点距离s的最短距离算法

基本结构是BFS，我们将当前点的相邻节点放入队列中，但是队列每次弹出的值获取最小的那个，因此使用优先队列

每次从队列弹出的点若之前没出现过则其距离s的距离就是所有距离中最短的

```

graph_x={
    "A":{"B":5,"C":1},
    "B":{"A":5,"C":2,"D":1},
    "C":{"A":1,"B":2,"D":4,"E":8},
    "D":{"B":1,"C":4,"E":3,"F":6},
    "E":{"C":8,"D":3},
    "F":{"D":6},
}


def dijkstra(graph,s):


    pqueue=queue.PriorityQueue()
    node=collections.namedtuple("node","val name")
    pqueue.put(node(0,s))
    distence_dic={}
    while not pqueue.empty():
        p=pqueue.get()
        distence_dic.setdefault(p.name,p.val)
        nodes=graph[p.name]
        for k,v in nodes.items():
            if k not in distence_dic.keys():
                pqueue.put(node(v+distence_dic[p.name],k))
    return distence_dic


```

```

if __name__=="__main__":
    BFS(graph,"A")
    DFS(graph,"A")
    res=dijkstra(graph_x,"A")
    for k,v in res.items():
        print(k,v)

```