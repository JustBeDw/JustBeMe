## mysql索引为啥用b+树而不用b树

1. b+树的数据都集中在叶子节点。分支节点 只负责索引。 b树的分支节点也有数据 。 b+树的层高 会小于 B树 平均的Io次数会远大于 B+树。
2. b+树更擅长范围查询。叶子节点 数据是按顺序放置的双向链表。 b树范围查询只能中序遍历。
3. 索引节点没有数据。比较小。b树可以吧索引完全加载至内存中。