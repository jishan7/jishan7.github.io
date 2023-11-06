
# 不用递归的后序遍历python实现

核心点就是使用栈这个数据结构

```python
# coding=utf-8

stack = []

class Node(object):
    data = 0
    lChild = None
    rChild = None
    

def NewNode(_data):    
    p = Node()
    p.data = _data
    return p

def postOrder(root):
    t = root
    start = 1
    if t:
        while (start == 1 or len(stack) > 0):   # start的作用是使大循环至少执行一次。这样栈才能有内容，才能开启下次循环
            start = 0
            while (t):   #  循环，将所有最左结点压栈
                stack.append(t)
                t = t.lChild

            flag = 1   # 表示左子树的节点为空或者都被访问了
            p = None   # 最近一个被访问过的节点
            while (len(stack) > 0 and flag == 1):
                t = stack[-1]   # 注意：这里只是获取栈顶元素，而并没有出栈
                if t.rChild == p:   # 如果当前结点右孩子为空，或者已经被访问过，则访问当前结点
                    print t.data   # 访问
                    stack.pop()   # 当前结点出栈
                    p = t   # 更新：最近一个被访问过的节点为t

                else:   # 如果当前结点右孩子不为空，则先去处理右孩子
                    t = t.rChild   # 处理右孩子
                    flag = 0  #  t的左孩子未被访问，flag置为0

def go():
    '''
    构建树结构如下：
       1
     2   3
    4 5

    '''

    root = NewNode(1)
    root.lChild = NewNode(2)
    root.rChild = NewNode(3)
    root.lChild.lChild = NewNode(4)
    root.lChild.rChild = NewNode(5)

    postOrder(root)  # output：4 5 2 3 1

if __name__ == '__main__':
    go()

```
