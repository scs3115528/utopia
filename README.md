# utopia 原理
``` 
Attempting to execute command:
['target_analyzer', '--db',
PosixPath('/root/UTopia/exp/snappy/output/build_db/libsnappy.a.json'), '--extern',
PosixPath('/root/UTopia/helper/external'), '--public',
PosixPath('/root/UTopia/result/test/snappy/api.json'), '--out',
PosixPath('/root/UTopia/result/test/snappy/target/libsnappy.a.json')]

自动生成driver的方法：（保证与源应用程序逻辑相关）
- 推断api的有效序列                
- 直接从使用示例中提取有效的api序列
