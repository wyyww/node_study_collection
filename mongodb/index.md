### mongodb索引详讲






用以下方法解决在重复数据的集合里建立唯一索引
1.首先把集合数据导出
mongoexport -d uxgourmet -c CollectedUrl -o CollectedUrl.json 
2.删除集合数据
db.CollectedUrl.remove({})
3.在集合上创建唯一索引
db.CollectedUrl.ensureIndex({uri:1},{unique:true})
4.把导出数据再次导入集合中
mongoimport -d uxgourmet -c CollectedUrl --upsert D:/CollectedUrl.json   
注意要加上--upsert选项
--upsert   insert or update objects that already exist
