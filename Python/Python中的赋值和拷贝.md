#Python中的赋值和拷贝

##赋值

在python中，赋值就是建立一个对象的引用，而不是将对象存储为另一个副本。例如：

    >>> a=[1,2,3]
    >>> b=a
    >>> c=a

对象是[1,2,3]，分别由a、b、c三个变量其建立了对应的引用关系。而三个变量都不独占对象[1,2,3]，或者说，可以通过任何一个变量来修改[1,2,3]这个对象。

    >>> c.append(4)
    >>> c
    [1, 2, 3, 4]
    >>> a
    [1, 2, 3, 4]
    >>> b
    [1, 2, 3, 4]
    >>> b.append("from b")
    >>> b
    [1, 2, 3, 4, 'from b']
    >>> a
    [1, 2, 3, 4, 'from b']
    >>> c
    [1, 2, 3, 4, 'from b']

##拷贝

拷贝有浅拷贝和深拷贝之分。首先看下面的例子

    >>> a=[1,2,3]               #建立a与对象的引用连接
    >>> b=a                     #通过赋值，b也与对象建立引用
    >>> import copy
    >>> c=copy.copy(a)          #浅拷贝，建立了一个[1,2,3]的副本，即一个新的对象
    >>> d=copy.deepcopy(a)      #深拷贝，建立了一个[1,2,3]的副本。

至此，a、b、c、d的关系如下图所示：

a--|
   |-->[1,2,3] 
b--|

c----->[1,2,3]

d----->[1,2,3]

修改a

    >>> a.append('aa')          #通过变量a修改原对象，使它变成[1,2,3,'aa']
    >>> b                       #b也同步改变
    [1, 2, 3, 'aa']
    >>> c                       #c对应的是一个副本，没有受到影响
    [1, 2, 3]
    >>> d                       #d同上道理
    [1, 2, 3]

a、b、c、d的关系如下图所示：

a--|
   |-->[1,2,3,'aa'] 
b--|

c----->[1,2,3]

d----->[1,2,3]

修改c:

    >>> c.append("cc")          #修改c的对象
    >>> a                       #a没有受到影响，因为a、c是指向不同对象
    [1, 2, 3, 'aa']
    >>> b
    [1, 2, 3, 'aa']
    >>> c
    [1, 2, 3, 'cc']
    >>> d
    [1, 2, 3]

a、b、c、d的关系如下图所示：

a--|
   |-->[1,2,3,'aa'] 
b--|

c----->[1,2,3,'cc']

d----->[1,2,3]

在上面的例子中，似乎copy和deepcopy没有什么区别，都是另外建立一个副本。且看如下例子：

    >>> q=[1,2,3,['a','b']]         #注意，这个对象是一个list，里面还有一个元素是list，即q[3]=['a','b']
    >>> w=copy.copy(q)              #w、e分别是浅拷贝和深拷贝的副本对象引用
    >>> e=copy.deepcopy(q)
    
    >>> q.append('4q')              #q所对应的[1,2,3,['a','b']]变成[1,2,3,['a','b'],'4q']
    >>> q
    [1, 2, 3, ['a', 'b'], '4q']
    >>> w                           #w,e如同前面一样，没有受到影响
    [1, 2, 3, ['a', 'b']]
    >>> e
    [1, 2, 3, ['a', 'b']]

修改w的对应列表

    >>> w.append(4)
    >>> w                           #增加了一个整数4
    [1, 2, 3, ['a', 'b'], 4]
    >>> q                           #q和e都没有受到影响
    [1, 2, 3, ['a', 'b']]
    >>> e
    [1, 2, 3, ['a', 'b']]
    >>> q
    [1, 2, 3, ['a', 'b']]

修改w[3]的元素，提示：w是对q进行浅拷贝而得。

    >>> w[3].append('w3c')
    >>> q
    [1, 2, 3, ['a', 'b', 'w3c']]
    >>> w                               
    [1, 2, 3, ['a', 'b', 'w3c'], 4]
    >>> e
    [1, 2, 3, ['a', 'b']]

仔细观察上面的结果，发现：

- q和w，当修改了列表里面的列表元素之后，两个同步修改了。
- e没有受到影响

也就是浅拷贝，只建立了外层的副本，对于内层的副本没有建立；而深拷贝，建立了完整的副本。这就理解了汉语的翻译名称“浅”拷贝之含义了，其“浅”就是拷贝了一层（外层）。

进一步修改e看看效果：

    >>> e.append('5e')
    >>> q
    [1, 2, 3, ['a', 'b', 'w3c']]
    >>> w
    [1, 2, 3, ['a', 'b', 'w3c'], 4]
    >>> e
    [1, 2, 3, ['a', 'b'], '5e']
    >>> e[3].append('e3c')
    >>> q
    [1, 2, 3, ['a', 'b', 'w3c']]
    >>> w
    [1, 2, 3, ['a', 'b', 'w3c'], 4]
    >>> e
    [1, 2, 3, ['a', 'b', 'e3c'], '5e']
 
有思考的程序员，看到这里就会提出一个问题，为什么要有浅拷贝和深拷贝呢？它们各自是如何工作的？在神奇的网络上，对这个问题有回答，请参考以下两个连接内容：

- [http://blog.csdn.net/llg8212/article/details/22782387](http://blog.csdn.net/llg8212/article/details/22782387)
- [http://blog.csdn.net/yueguanghaidao/article/details/25613887](http://blog.csdn.net/yueguanghaidao/article/details/25613887)

