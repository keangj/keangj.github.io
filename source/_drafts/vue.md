vue 

没key，遍历最小的长度对比两个数组一一patch，不相同直接创建，相同就更新。

再判断新旧数组哪个长，旧的长就卸载（unmountChildren）掉，新的长就挂载（mountChildren）



判断

有key

从头遍历，VNode 相同（VNode 的 key 和 type 相同）就 patch 并继续遍历，不同就break。

然后从尾开始遍历（取新旧列表的长度 -1），VNode 相同就 patch 并继续遍历（两个数组的长度都 -1），不同就break。

双端遍历后仍然有新的节点，就创建新的节点（patch），旧的节点多就卸载

