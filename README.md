# Python-Knowledges
Python小知识集合
数据结构
线性表：顺序存放的一个表（数组、列表）
python实现线性表
#!/usr/bin/env python
# -*- coding:utf-8 -*-

class SeqList(object):
    def __init__(self, max=8):
        self.max = max      #创建默认为8
        self.num = 0
        self.date = [None] * self.max
        #list()会默认创建八个元素大小的列表，num=0，并有链接关系
        #用list实现list有些荒谬，全当练习
        #self.last = len(self.date)
        #当列表满时，扩建的方式省略
    def is_empty(self):
        return self.num is 0

    def is_full(self):
        return self.num is self.max

    #获取某个位置的元素
    def __getitem__(self, key):
        if not isinstance(key, int):
            raise TypeError
        if 0<= key < self.num:
            return self.date[key]
        else:
            #表为空或者索引超出范围都会引发索引错误
            raise IndexError

    #设置某个位置的元素
    def __setitem__(self, key, value):
        if not isinstance(key, int):
            raise TypeError
        #只能访问列表里已有的元素,self.num=0时，一个都不能访问,self.num=1时，只能访问0
        if 0<= key < self.num:
            self.date[key] = value   #该位置无元素会发生错误
        else:
            raise IndexError

    def clear(self):
        self.__init__()

    def count(self):
        return self.num

    def __len__(self):
        return self.num

    #加入元素的方法 append()和insert()
    def append(self,value):
        if self.is_full():
            #等下扩建列表
            print("list is full")
            return
        else:
            self.date[self.num] = value
            self.num += 1

    def insert(self,key,value):
        if not isinstance(key, int):
            raise TypeError
        if key<0:  #暂时不考虑负数索引
            raise IndexError
        #当key大于元素个数时，默认尾部插入
        if key>=self.num:
            self.append(value)
        else:
            #移动key后的元素
            for i in range(self.num, key, -1):
                self.date[i] = self.date[i-1]
            #赋值
            self.date[key] = value
            self.num += 1

    #删除元素的操作
    def pop(self,key=-1):
        if not isinstance(key, int):
            raise   TypeError
        if self.num-1 < 0:
            raise IndexError("pop from empty list")
        elif key == -1:
            #原来的数还在，但列表不识别他
            self.num -= 1
        else:
            for i in range(key,self.num-1):
                self.date[i] = self.date[i+1]
            self.num -= 1

    def index(self,value,start=0):
        for i in range(start, self.num):
            if self.date[i] == value:
                return i
        #没找到
        raise ValueError("%d is not in the list" % value)

    #列表反转
    def reverse(self):
        i,j = 0, self.num - 1
        while i<j:
            self.date[i], self.date[j] = self.date[j], self.date[i]
            i,j = i+1, j-1

if __name__=="__main__":
    a = SeqList()
    print(a.date)
    #num == 0
    print(a.is_empty())
    a.append(0)
    a.append(1)
    a.append(2)
    print(a.date)
    print(a.num)
    print(a.max)
    a.insert(1,6)
    print(a.date)
    a[1] = 5
    print(a.date)
    print(a.count())

    print("返回值为2(第一次出现)的索引：", a.index(2, 1))
    print("====")
    t = 1
    if t:
        a.pop(1)
        print(a.date)
        print(a.num)
    else:
        a.pop()
        print(a.date)
        print(a.num)
    print("========")
    print(len(a))

    a.reverse()
    print(a.date)
    """
    print(a.is_full())
    a.clear()
    print(a.date)
    print(a.count())
    """
链表（LinkedList）：由节点组成（适合做插入和删除）

一个节点分为二部分，前半部分存储数据，后半部分存储下一个节点的地址

python实现单链表
# coding=utf-8
# 单链表的实现


class SingleNode:
    """单链表的节点"""
    def __init__(self, item):
        # item存放数据元素
        self.item = item
        # 下一个节点
        self.next = None

    def __str__(self):
        return str(self.item)


class SingleLinkList:
    """单链表"""
    def __init__(self):
        self._head = None

    def is_empty(self):
        """判断链表是否为空"""
        return self._head is None

    def length(self):
        """获取链表长度"""
        cur = self._head
        count = 0
        while cur is not None:
            count += 1
            # 将cur后移，指向下一个节点
            cur = cur.next
        return count

    def travel(self):
        """遍历链表"""
        cur = self._head
        while cur is not None:
            print(cur.item)
            cur = cur.next
        print("")

    def add(self, item):
        """链表头部添加元素"""
        # node表示一个节点
        node = SingleNode(item)
        # 此时链表里面只有一个节点,所以节点的next为None
        node.next = self._head

        self._head = node

    def append(self, item):
        """链表尾部添加元素"""
        node = SingleNode(item)

        if self.is_empty():
            self._head = node
        else:
            cur = self._head
            # 使用循环判断当前节点的下一个节点是否为空
            while cur.next is not None:
                # 如果下一个节点不为空，则将下一个节点赋值给当前节点
                cur = cur.next

            # 此时cur指向链表最后一个节点，即 next = None
            cur.next = node

    def insert(self, pos, item):
        """指定位置添加元素"""
        # 若指定位置pos为第一个元素之前，则执行头部插入
        if pos <= 0:
            self.add(item)

        # 若指定位置超过链表尾部，则执行尾部插入
        elif pos > (self.length() - 1):
            self.append(item)

        # 找到指定位置
        else:
            node = SingleNode(item)
            cur = self._head
            cur_pos = 0
            while cur.next is not None:
                # 获取需要插入位置的上一个节点
                if pos - 1 == cur_pos:
                    node.next = cur.next
                    cur.next = node
                cur = cur.next
                cur_pos += 1

    def remove(self, item):
        """删除节点"""
        cur = self._head
        while cur is not None:
            if cur.next.item == item:
                cur.next = cur.next.next
                break
            cur = cur.next

    def search(self, item):
        """查找节点是否存在"""
        cur = self._head
        count = 0
        while cur is not None:
            if cur.item == item:
                return count
            cur = cur.next
            count += 1

        # 找不到元素
        if count == self.length():
            count = -1
        return count


if __name__ == "__main__":
    ll = SingleLinkList()
    ll.add(1)           # 1
    print(ll)
    ll.add(2)           # 2 1
    ll.append(3)        # 2 1 3
    ll.insert(2, 4)     # 2 1 4 3
    print("length:", ll.length())   # 4
    ll.travel()         # 2 1 4 3
    print("search(3):", ll.search(3))   # 3
    print("search(5):", ll.search(5))   # -1
    ll.remove(1)
    print("length:", ll.length())       # 3
    ll.travel()         # 2 4 3
堆栈（堆、栈）：先进后出

 压栈

弹栈

队列：先进先出

使用先进先出的的存储方式

使用二分查找前必须要先排序
