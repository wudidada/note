## 连接

### MongoDB URI

MongoDB的连接格式

```
mongodb://[username:password@]host1[:port1][,...hostN[:portN]][/[defaultauthdb][?options]]
```

### cli

```
mongo				# 直接登录
mongo -u <username>	# 用户名登陆
```

### python

```python
from pymongo import MongoClient
# default host and port
client = MongoClient()
# explicit host and port
client = MongoClient('localhost', 27017)
# MongoDB URI format
client = MongoClient('mongodb://localhost:27017/')
```