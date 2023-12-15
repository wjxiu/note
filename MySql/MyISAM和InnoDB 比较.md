# MyISAM和InnoDB 比较(以及其他)

|          | InnoDB           | MyISAM                                                       |
| -------- | ---------------- | ------------------------------------------------------------ |
| 事务     | 支持             | 不支持                                                       |
| 外键     | 支持             | 不支持                                                       |
| 存储结构 | 存储在一个文件里 | 分为三个文件存储<br />frm文件存储表定义。<br />数据文件的扩展名为.MYD (MYData)。<br />索引文件的扩展名是.MYI (MYIndex) |
| 锁       | 支持表锁，行锁   | 只支持表锁                                                   |
| 总行数   | 遍历获取         | 读取变量                                                     |
| 底层     | b+树             | b树                                                          |

## 以下是mysql官方文档的对比,包含了其他引擎

| Feature                                | MyISAM       | Memory           | InnoDB       | Archive      | NDB          |
| :------------------------------------- | :----------- | :--------------- | :----------- | :----------- | :----------- |
| B-tree indexes                         | Yes          | Yes              | Yes          | No           | No           |
| Backup/point-in-time recovery (note 1) | Yes          | Yes              | Yes          | Yes          | Yes          |
| Cluster database support               | No           | No               | No           | No           | Yes          |
| Clustered indexes                      | No           | No               | Yes          | No           | No           |
| Compressed data                        | Yes (note 2) | No               | Yes          | Yes          | No           |
| Data caches                            | No           | N/A              | Yes          | No           | Yes          |
| Encrypted data                         | Yes (note 3) | Yes (note 3)     | Yes (note 4) | Yes (note 3) | Yes (note 5) |
| Foreign key support                    | No           | No               | Yes          | No           | Yes          |
| Full-text search indexes               | Yes          | No               | Yes (note 6) | No           | No           |
| Geospatial data type support           | Yes          | No               | Yes          | Yes          | Yes          |
| Geospatial indexing support            | Yes          | No               | Yes (note 7) | No           | No           |
| Hash indexes                           | No           | Yes              | No (note 8)  | No           | Yes          |
| Index caches                           | Yes          | N/A              | Yes          | No           | Yes          |
| Locking granularity                    | Table        | Table            | Row          | Row          | Row          |
| MVCC                                   | No           | No               | Yes          | No           | No           |
| Replication support (note 1)           | Yes          | Limited (note 9) | Yes          | Yes          | Yes          |
| Storage limits                         | 256TB        | RAM              | 64TB         | None         | 384EB        |
| T-tree indexes                         | No           | No               | No           | No           | Yes          |
| Transactions                           | No           | No               | Yes          | No           | Yes          |
| Update statistics for data dictionary  | Yes          | Yes              | Yes          | Yes          | Yes          |
