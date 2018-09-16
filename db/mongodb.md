# MongoDB

---

## Install
1. https://www.mongodb.com/download-center#community
2. 与bin同级目录创建data/db和data/log
3. 与bin同级目录创建mongodb.cfg

```cfg
systemLog:
	destination: file
	path: c:\WeekSpace\mongodb\data\log\mongodb.log
	logAppend: true
storage:
	dbPath: c:\WeekSpace\mongodb\data\db
net:
	port: 27017
	bindIp: 0.0.0.0
```
4. mongod.exe --config "C:\mongodb\mongod.cfg" --install(可能需要安装VC++：【https://www.microsoft.com/zh-cn/download/details.aspx?id=48145】)
5. net start MongoDB
6. 初始化帐号
```cmd
mongo
>use admin
>show users
>db.createUser({ user: "admin", pwd: "123456", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })
>show users
>exit
```
7. 登录需要密码
```cfg
security:
	authorization: enabled
```
8. 重启服务
```cmd
mongo
>use admin
>db.auth("admin","123456")
>show users
>exit
```

## CSharp Drive

```csharp

private static readonly string _connectionString = ConfigurationManager.ConnectionStrings["MongoConnectionString"].ConnectionString;
private static readonly MongoClient _client = new MongoClient(_connectionString);
private static readonly IMongoDatabase _database = _client.GetDatabase("DB");
IMongoCollection<MsgEntity> collection = _database.GetCollection<MsgEntity>("MsgDB201809");
var sort = Builders<MsgEntity>.Sort.Descending("CreateTime");
var filterBuilder = Builders<MsgEntity>.Filter;
var filter = filterBuilder.Empty;
if (null != dto)
{
    if (false == string.IsNullOrEmpty(dto.AppNo))
        filter &= filterBuilder.Eq(x => x.AppNo, dto.AppNo);
    if (false == string.IsNullOrEmpty(dto.Identification))
        filter &= filterBuilder.Eq(x => x.Identification, dto.Identification);
    if (dto.StartOn.HasValue)
        filter &= filterBuilder.Gte(x => x.CreateTime, dto.StartOn.Value);
    if (dto.EndOn.HasValue)
        filter &= filterBuilder.Lte(x => x.CreateTime, dto.EndOn.Value);
}
return collection
    .Find(filter)
    .Sort(sort)
    .Skip((dto.Index - 1) * dto.Length)
    .Limit(dto.Length)
    .ToListAsync().Result;
```

