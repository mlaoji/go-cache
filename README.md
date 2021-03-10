# go-cache

本地K/V缓存, 支持远程更新

### Installation

`go get github.com/mlaoji/go-cache`

### Usage

```go
import (
	"fmt"
	"github.com/mlaoji/go-cache"
	"time"
)

func main() {
    //创建缓存实例，设置全局默认过期时间5分钟，清理时间间隔10分钟
	c := cache.NewCache(5*time.Minute, 10*time.Minute)

    //设置缓存,使用默认过期时间
	c.Set("key", "val")

    //设置缓存,使用指定过期时间
	c.Set("key", "val", 30*time.Minute)

    //设置缓存, 不过期
	c.Set("key", "val", 0)

    //获取缓存，过期时间及是否存在
	val, expiration, found := c.Get("key")
	if found {
		fmt.Println(expiration)
		fmt.Println(val)
	}

    //递增
	newval, err := c.Increment("key", 1)
	if nil == err {
		fmt.Println(newval)
	}

    //递减
	newval, err := c.Decrement("key", 1)
	if nil == err {
		fmt.Println(newval)
	}

    //通过redis广播消息, 远程更新缓存
    cache.RedisConf = map[string]string{
        "host":"",
        "pass":"",
        }

	err := c.FlushCache("key")
	if nil != err {
		fmt.Println(err)
	}

}
```
