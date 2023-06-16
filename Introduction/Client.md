<div dir="rtl">

## ۲.کلاینت‌ (client):
### 📝فهرست
- [ORM module (object related mapping)](#object-related-mapping-module)
- [httplib-module](#httplib-module)
- [cache module](#cache-module)
- [back](https://github.com/NikanV/Beego/blob/main/Introduction/README.md)


## object related mapping module:
  کتابخانه‌های orm مانند `gorm`، برای ترجمه داده‌ها بین زبان‌های شی گرا و پایگاه‌های داده‌ی relational طراحی شده اند. این کتابخانه‌ها برای کار با انواع پایگاه‌های داده از جمله `mysql`، `postgresql`، `sqlite` و سایر `sql servers` یک unified interface به کاربر ارائه میدهند که بتواند با syntax های object oriented programs برنامه خود را بنویسد نه sql querry ها.
  
  چهارچوب beego با ارائه beeorm برعکس بقیه کتابخانه‌های object related mapping که قصد دارند با یک generic interface با تنوعی از database ها کار کنند، تنها تمرکز خود را برروی `mysql` قرار داده است. این عمل به developer ها کمک میکند که از امکانات unique پایگاه داده mysql استفاده کند که به performance و reliability بالا ختم میشود.
  
  اما از آنجاییکه mysql برای ذخیره داده option خیلی خوبی است، اما برای تحمل ترافیک سنگین طراحی نشده است. برای حل این مشکل beeorm از تکنولوژی `nosql` و سایر message broker ها استفاده کرده است که از بابت ترافیک performance برنامه پایین نیاید.
  
  کتابخانه beeorm با اضافه کردن لایه `cache`، تعداد query هایی را که برای database باید execute شوند را کاهش داده است. این کار با کم کردن load درخواست ها به database، به آن کمک میکند. اما اضافه کردن caching layer باعث پیچیدگی و بوجود آمدن bug ها میشود. مثلا اگر برای login کاربر استفاده شود، از آنجاییکه یک کاربر را برای استفاده ذخیره میکند، میتواند باعث security concerns برای حساب کاربری افراد شود زیرا امکان این بوجود خواهد آمد که کاربر ها وارد حساب کاربری هم شوند.
  
  در کنار اینها، beeorm با ارائه `redis client` خود به developer ها، این امکان را فراهم میسازد که همه‌ی دستورات redis (بهمراه دستورات اضافی که برای beeorm طراحی شده است مانند `shared lock` و `rate limiter`) را اجرا کنند. وجود این client کاربر را از سایر کتابخانه‌ها مانند `go-redis` در برنامه خود بی نیاز میسازد. 
  
  در شرایطی که برنامه نیاز به handle کردن ترافیک زیاد را داشته باشد، `event streaming` بسیار کارآمد خواهد بود. `event streaming` به developer ها این امکان را میدهد که داده‌های برنامه خود را بین سرویس‌ها پخش کنند. برای این عمل راهکارهای متفاوتی مانند `apache kafka` و `rabitmq` وجود دارد ولی اضافه کردن این قابلیت ها امکان پیچیدگی را به ساختار برنامه میدهد. 
  
  اینجاست که `redis` با یک feature بسیار قوی خود بنام `streams` که قابلیت ساخت event streaming system های سریع را دارد وسط می‌آید. در beeorm با استفاده از event broker امکان ساخت event stream ها در برنامه‌ها توسط developers میسر میشود که دستور آن بصورت زیر است:
  
  ```go
broker := engine.GetEventBroker()

// Publish an event to a stream
broker.Publish("$streamname$", event)

// Consume events from a stream using a consumer and consumer group
broker.Consumer("$myconsumer$", "$mygroup$").Consume(...)
 ```
<div dir="rtl">

  ## httplib module:
  مانند `curl` ماژول `httplib` نیز برای simulate کردن http requestهایی است که از سمت client گرفته میشوند. این ماژول مانند `jquery` زنجیر کردن متد‌ها یا method chaning را پشتیبانی میکند.
  
برای install این ماژول میتوان از دستور زیر در ترمینال استفاده کرد:
  
  ```bash
  go get github.com/beego/beego/v2/client/httplib
  ```
  
  پس از install ماژول، کافیست آنرا در پروژه go مورد نظر import کنید که میتوان import آنرا بصورت زیر انجام داد:
  
  ```go
  import (
	"github.com/beego/beego/v2/client/httplib"
)	
  ```
  
  برای initialize کردن و تست آن نیز میتوان از دستور زیر استفاده کرد:
  
  ```go
  //initialize:
  req := httplib.Get("$url$")
  
  //test:
  str, err := req.String()
  if err != nil {
    t.Fatal(err)
  }
  fmt.Println(str)
  ```
  
  
ماژول httplib همه‌ی methodهای http از جمله `get`، `post`، `put`، `delete` و `head` را برای استفاده کاربر فراهم میکند که هر کدام از این متدها در ورودی خود یک `url` بعنوان ورودی دریافت میکنند. 
  
  برای debug کردن این بخش از برنامه نیز httplib یک تابع بنام `debug` دارد که در صورت فعال بودن `method`، `host` و `user-agent` را در report خود برمیگرداند. نحوه استفاده از این تابع بصورت زیر است:
  
  ```go
  httplib.Get("$url$").Debug(true).Response()
  ```
  
  در صورتیکه request scheme بصورت http باشد، نیاز است transport layer security یا همان `tls` برای client تنظیم گردد.
  
  این ماژول امکان تنظیم ‍`timeout` برای فرستادن request و خواندن data را نیز با تابع `setTimeout` بصورت زیر فراهم میسازد:
  
  ```go
  req.SetTimeout(connectTimeout, readWriteTimeout)
  ```
  
  که این تابع را میتوان حنی برای متد get نیز بصورت زیر نوشت:
  
  ```go
  httplib.Get("http://beego.vip/").SetTimeout($amount1$ * time.Second, $amount2$ * time.Second).Response()
  ```
  
  برای request های `put` یا `post` در صورت وجود parameter برای ورودی request، میتوان آن پارامتر ها را بصورت زیر تنظیم کرد:
  
  ```go
  req := httplib.Post("$url$")
  req.Param($p1$,$v1$)
  req.Param($p2$,$v2$)
  ```
  
  برای فرستادن فایل یا داده‌های بزرگ، میتوان از تابع `body` استفاده کرد که نمونه‌ای از آن در فایل زیر آمده است:
  
  ```go
  req := httplib.Post("$url$")
  bt,err:=ioutil.ReadFile("$filename$")
  if err!=nil{
    log.Fatal("read file err:",err)
  }
  req.Body(bt)
  ```
  
  برای تنظیم `header`، میتوان از تابع Header استفاده نمود:
  
  ‍```go
  req := httplib.Post("$url$")
  req.Header("Accept-Encoding","gzip,deflate,sdch")
  ```
  
  برای `file upload` میتوان از تابع `postFile` استفاده کرد. این تابع برای ورودی خود دو پارامتر دریافت میکند. پارامتر اول نام فرم و پارامتر دوم نام یا آدرس فایل است:
	
```go
req.PostFile("$formname$", "$filenameorpath$")
str, err := b.String()
if err != nil {
    t.Fatal(err)
}
```

برای دریافت response پس از فرستادن request توسط get، راه‌های زیر ممکن است:

	
|Method                          |Type                     |توضیح                                                |
|--------------------------------|-------------------------|-----------------------------------------------------------|
|<div>`req.Response()` </div>               |<div>`(*http.Response, error)`</div>|یک httpresponse برمیگرداند که میتوان data از آن خواند|
|<div>`req.Bytes()`</div>                   |<div>`([]byte, error)`</div>        |بدنه خام یک response را بر میگرداند       |
|<div>`req.String()`</div>                  |<div>`(string, error)`</div>        |بدنه خام یک response را بر میگرداند                                 |
|<div>`req.ToFile(filename string)`</div>   |<div>`error`</div>                  |بدنه response را دی فایل ذخیره میکند                           |
|<div>`req.ToJSON(result interface{})`</div>|<div>`error`</div>                  |فایل response که به فرمت `json` است را به یک object تبدیل میکند             |
|<div>`req.ToXml(result interface{})`</div> |<div>`error`</div>                  |فایل response که به فرمت `xml` است را به یک object تبدیل میکند                 |

<div dir="rtl">

# cache module:
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
  

  [بازگشت](https://github.com/NikanV/Beego/blob/main/Introduction/README.md)

</div>
