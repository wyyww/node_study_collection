### BSON
  BSON是JSON的扩展，新增了诸如日期，浮点等JSON不支持的数据类型
  
BSON是一种类json的一种二进制形式的存储格式，简称Binary JSON，它和JSON一样，支持内嵌的文档对象和数组对象，
  但是BSON有JSON没有的一些数据类型，如Date和BinData类型。

BSON可以做为网络数据交换的一种存储形式，这个有点类似于Google的Protocol Buffer，
但是BSON是一种schema-less的存储形式，它的优点是灵活性高，但它的缺点是空间利用率不是很理想，

BSON有三个特点：轻量性、可遍历性、高效性
