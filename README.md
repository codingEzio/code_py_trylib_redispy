
### Oh
- You *cannot* write comments in *Redis*.
- Commands in *Redis* are **case-insensitive** (eh?).

### *Get*
  ```bash
  # for macOS
  $ brew update && brew install redis
  $ redis-server
  $ redis-cli
  
  # Or you might wanna install it from source
  # see here: https://realpython.com/python-redis/
  ```

### *Run*
- services
  ```bash
  # for macOS
  brew services start redis
  
  # for any OS
  redis-server
  ```

- run with config
  ```bash
  # config param :: redis-cli
  $ redis-cli -h 127.0.0.1 -p 8414      # hostname, port
  $ redis-cli -h localhost -a PASSWORD
  $ redis-cli --stat
  
  # config param :: redis-server
  $ redis-server /usr/local/etc/redis.conf --loglevel verbose
  $ redis-server /anywhere/is/fine.conf
  $ redis-server --port 6379
  $ redis-server --port 7379 --replicaof 127.0.0.1 6379
  ```

### Find *help*
  ```bash
  127.0.0.1:6379> help
  127.0.0.1:6379> help MSET
  ```

### What *is* Redis?
- Kinda like a *data structure server* (more than `dict` in Python)

### Examples
- All of this were done in `redis-cli`
  ```bash
  SET name jay
  GET name
  
  EXISTS name

  # M: Multiple keys
  MSET name jay age 40
  MGET name age
  
  # H: Hash
  HSET realpython url      "http:example.com/"
  HSET realpython fullname "Real Python"
  HGET realpython url
  HGET realpython url fullname

  # HM: Hash set multiple keys
  HMSET pypa url "https://pypa.io" isitawesome "awesome"
  HMGET pypa url isitawesome
  HGETALL pypa
  ```

### Library :: `redis-py` <small>(`pip3 install redis==3.3.11`)</small>
  ```python
  import redis
  r = redis.Redis()

  r.set("name", "jay")
  r.get("name")                   # b'jay'
  r.get("name").decode("utf-8")   #  'jay'

  r.mset({"name": "jay", "age": 69})
  r.mget(["name", "age"])

  # supported key types: bytes, str, int, float
  import datetime as dt
  today    = dt.date.today().isoformat()
  visitors = ["Jay", "Gloria", "Phil"]

  r.sadd    (today, *visitors)  # now it raises error I don't know why exactly
  r.smembers(today)
  r.scard   (today)
  ```

### Reading materials
- [*Redis CLI* Intro](https://redis.io/topics/rediscli)
- [In case you wanna *start Redis with specific* `.rdb` *file*](https://stackoverflow.com/questions/14497234/how-to-recover-redis-data-from-snapshotrdb-file-copied-from-another-machine)
