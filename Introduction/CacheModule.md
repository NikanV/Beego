<div dir="rtl">

# پ- cache module:
  این ماژول در beego با الهام از `database/sql` برای cache کردن داده‌ها استفاده میشود. این ماژول از چهار cache provider استفاده میکند:
  
  1. `file`
  2. `memcache`
  3. `memory`
  4. `redis`
  
  برای install این ماژول میتوان از دستور زیر استفاده کرد:
  
  ```bash
  go get github.com/beego/beego/v2/client/cache
  ```
  
  سپس میتوان با import کردن آن در پروژه خود بصورت زیر از beego cache module در پروژه استفاده نمود:
  
  ```go
  import (
    "github.com/beego/beego/v2/client/cache"
  )
  ```  
  در صورت استفاده از `memcache` یا `redis` نخست باید memcache را با دستورهای زیر نصب و در پروژه import کرد:
  
  ```bash
  go get -u github.com/beego/beego/v2/client/cache/memcache
  ```
  
  ```go
  import _ "github.com/beego/beego/v2/client/cache/memcache"
  ```
  
  سپس با initialize کردن یک global variable میتوان به استفاده از آن پرداخت:
  
  ```go
  bm, err := cache.NewCache("memory", `{"interval":$interval$}`)
  ```
  
  برای config کردن provider ها میتوان از دستورات زیر استفاده کرد:
  
  - `memory`
```go  
{"interval":$timeforgcinsecs$}
```
- `file`
```go
{"CachePath":"./cache","FileSuffix":".cache","DirectoryLevel":2,"EmbedExpiry":120}
```
- `redis`
```go
{"key":"$key$","conn":"$ip$:$port$","dbNum":"$dbnumericindex$","password":"$passwordconnectedredisserverpass$"}
```

- `memcache`
```go
{"conn":"$ip$:$port$"}
```
  
  در صورتی که developer تمایل به ساخت provider خود داشت، میتواند با نوشتن interface آن و سپس register کردن آن provider از آن استفاده کند:
  
 ```go
  type Cache interface {
	// Get a cached value by key.
	Get(ctx context.Context, key string) (interface{}, error)
	// GetMulti is a batch version of Get.
	GetMulti(ctx context.Context, keys []string) ([]interface{}, error)
	// Set a cached value with key and expire time.
	Put(ctx context.Context, key string, val interface{}, timeout time.Duration) error
	// Delete cached value by key.
	Delete(ctx context.Context, key string) error
	// Increment a cached int value by key, as a counter.
	Incr(ctx context.Context, key string) error
	// Decrement a cached int value by key, as a counter.
	Decr(ctx context.Context, key string) error
	// Check if a cached value exists or not.
	IsExist(ctx context.Context, key string) (bool, error)
	// Clear all cache.
	ClearAll(ctx context.Context) error
	// Start gc routine based on config string settings.
	StartAndGC(config string) error
}
  
  func init() {
	cache.Register("$name$", $func$)
}
 ```
  
  
  

  [بازگشت](https://github.com/NikanV/Beego/blob/introbranch/Introduction/Client.md)

</div>


