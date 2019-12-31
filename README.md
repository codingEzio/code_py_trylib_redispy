
### Oh
- The documentation ***sucks***.
- You *cannot* write comments in *Redis*.
- Commands in *Redis* are **case-insensitive** (eh?).


### Hey
- Commands that I'm not so familiar with *won't* be place here <small>(I'm a little bit dumb 🤪)</small>.
- Commands start with `H` also works with other commands start with `H` <small>(might not be true in another way)</small>.
  ```bash
  SADD  names John Bob
  SCARD names             # yes

  HSET  website google "google.com" twitter "twitter.com"
  SCARD website           # noooo
  ```


### What is [*Redis*](https://redis.io/documentation)
- Kinda like a *data structure server* (more than `dict` in Python)

### What is [*hiredis*](https://pypi.org/project/hiredis/)
> Library *redis-py* comes with two parsers: `HiredisParser` and `PythonParser`. The former uses the `hiredis` library to parse responses from the redis server, while the latter uses Python. *Hiredis* is a library that uses C, so it is much **faster** than the python parser, but requires installing the library separately <small>(quote from [here](https://django-redis-cache.readthedocs.io/en/latest/advanced_configuration.html#pluggable-parser-classes))</small>.

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
  $ redis-cli -n 1                      # use database '1'
  
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

### Examples
- All of this were done in `redis-cli`
  ```bash
  # HELP!
  help COMMAND

  # Is it 😉?
  EXISTS name                             # defined or not
  SISMEMBER nice_websites "aeon.co"       # is "aeon.co" in SET 'nice_websites'?

  # Bang 🔫!
  DEL name [name2 name3]                  # delete ANYTHING attached to that NAME
  ```

  ```bash
  # Simple key/value
  SET  name jay                           # one at a time
  GET  name      
  MSET name jay age 40                    # multiple at a time
  MGET name age
  
  # Hashes (like dict in Python)
  HSET    realpython url "http:example.com/"
  HGET    realpython url
  HMSET   pypa url "https://pypa.io" isitawesome "awesome"
  HMGET   pypa url isitawesome
  HGETALL pypa                            # all the key/values

  # Set (like {} in Python without keys (it's unsorted as well))
  SADD      nice_websites "aeon.co" "undark.org" "dev.to"
  SREM      nice_websites "aeon.co" "undark.org"
  SMEMBERS  nice_websites
  SMOVE     nice_sites sites "aeon"       # move "aeon" from "nice_sites" to "sites"
  SMOVE     nice_sites sites "bang"

  # Sorted set (sorted set & with a score)
  ZADD      vital_activities 1 "sleep" 2 "eat" 100 "read"
  ZREM      vital_activities 1 "sleep" 2 "eat"
  ZRANGE    vital_activities 0 -1               # asc  (1 -> 100)
  ZREVRANGE vital_activities 0 -1               # desc (100 -> 1)
  ZREVRANGE vital_activities 0 -1 WITHSCORES    # list the members (with scores)
  ```
- More complexed commands
  ```bash
  # Two examples for later use
  ZADD police  100 'Jones'  500 'Pelosi'  300 'Peyton'
  ZADD clerk  -100 'Jones' 1000 'Opal'   1200 'Olwen'

  # Sorted set :: ZUNIONSTORE
  ZUNIONSTORE yay 2 police clerk                              # mixed them all
  ZUNIONSTORE yay 2 police clerk WEIGHTS 3 0.5                # *3 *0.5
  ZUNIONSTORE yay 2 police clerk WEIGHTS 3 0.5 AGGREGATE SUM  # *3 *0.5, SUM 'Jones'
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
- [`redis-cli` Intro](https://redis.io/topics/rediscli)
- [In case you wanna *start Redis with specific* `.rdb` *file*](https://stackoverflow.com/questions/14497234/how-to-recover-redis-data-from-snapshotrdb-file-copied-from-another-machine)

### References
- `ZUNIONSTORE`
  - [Documentation](http://redisdoc.com/sorted_set/zunionstore.html) <small>(in Chinese)</small> <small>(sorry for English readers!)</small>
  - [How to perform union of sorted set values in redis](https://codedestine.com/redis-zunionstore/)
