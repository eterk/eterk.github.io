1. 检查不同的执行计划
`select * from table where value=0.0; => spark cast value to Double`
`select * from table where value=0; => spark cast value to Int `
this maybe cause different result
1. schema prune ,模式裁剪
可以较少不必要的字段的的读取
spark.conf.set("spark,sql.optimizer.nestedSchemaPruning.enabled",true)
1. collapse project 
1. cross session cached
2. A sql query=>mutiple job
3. A job => A directed acyclic graph
4. 优化
   - 找到不平衡的地方
   - 持久化
   - join 优化
     - SortMergeJoins(Standard)  => both sides is big
     - Broadcast Joins(Fastest)  => one side is small( one sided < spark.sql.autoBroadcastJoinThreshold)(default 10M)
     - Sknew Joins
     - Range Joins
     - BroadcastedNestedLoopJoin(BNLJ)
   - 处理数据倾斜
   - 减少耗时的操作
     - repartition :use coalesce or shuffle partition count
     - count :Do you really need it
     - use approxCountDistinct  instead of distinctCount
     - use dropDuplicateds instead distinct before join or group by 
   - udfs
   - 多维度并行
     - spark 包含三个维度的并行度
     - driver 并行度
     - horizontal 
     - executor