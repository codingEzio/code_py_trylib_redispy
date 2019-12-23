
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

### Reading materials
- [*Redis CLI*](https://redis.io/topics/rediscli)
