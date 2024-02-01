缩点，顾名思义，就是把多个点当成一个点看待。典型的用法就是把强联通分量 SCC 缩为点。

老样子上例题


如何判断环？[SCC](https://www.cnblogs.com/2044-space-elevator/articles/17987199) 套一个环的判断即可。当 `dfn[n] == low[n]` 时说明就有一个强连通，具体的请看我以前写的博客：[link](https://www.cnblogs.com/2044-space-elevator/articles/17987199)。

