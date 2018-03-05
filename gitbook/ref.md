## 介绍在GitBook中如何进行交叉引用 参考：[这里](https://seadude.gitbooks.io/learn-gitbook/content/)

### 绝对引用与相对引用
所谓的绝对引用就是用URL的形式来进行，这样导出的PDF即使是同一书籍内的引用也不能跳转，而且一旦URL发生变化，带来的工作量是巨大的，这里只是区别一下，以下内容不对绝对引用进行介绍。

### 相对引用
例如我们有如下目录

``` 
ch1/art1.md
    # head1
         text
    # head2
ch1/art2.md
    # head3
    # head4
ch2/art3.md
    # head5
    # head6
ch2/art4.md
art5.md
```

1. 需求1：在text处链接head2 -> `[name](#head2)`
2. 需求2：在text处链接art1.md -> `[name](./art1.md)`
3. 需求3：在text处链接art3.md -> `[name](../ch2/art3.md)`
4. 需求4：在text处链接head3 -> `[name](./art2.md#head3)`
5. 需求5：在text处链接head5 -> `[name](../ch2/art3.md#head5)`


