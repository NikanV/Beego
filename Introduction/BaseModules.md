<div dir="rtl">

## ۱. ماژول‌های پایه (base modules):

### 📝فهرست
- [log module](#log-module)
- [config module](#config-module)
- [admin module](#admin-module)
- [back](https://github.com/NikanV/Beego/blob/main/Introduction/README.md)

 

## log module:
این ماژول با الهام گیری از `database/sql` برای پشتیبانی `net`، `consule` ،`file` و `stmp` برای مقصد بصورت پیشفرض تهیه شده است.
برای install کردن این module میتوان از دستور زیر استفاده کرد:
	
  ```bash
go get github.com/beego/beego/v2/core/logs
```
پس از نصب آن تنها لازم است که آنرا در پروژه خود بصورت زیر import کنید:
  ```go
import (
	"github.com/beego/beego/v2/core/logs"
)
```
سپس کافیست یک `log variable` برای برنامه خود تعریف کنید:
  ```go
log := logs.NewLogger($cache size$)
```
  در نهایت پس از اضافه کردن `log variable` میتوان با استفاده از متد `SetLogger`، به آن provider برای خروجی را به یکی از صورت های
	
  `console`، `file`، `multifile`، `conn`، `smtp`، `ElasticSearch` و `SetSecure` 
  
  برای پارامتر اول و config مخصوص آن provider را بصورت string برای پارامتر دوم داد:    

  ```go
log.SetLogger(logs.AdapterFile,{"filename":"$filename$","level":$level$,"maxlines":$maxlines$,"maxsize":$maxsize$,"daily":$boolean$,"maxdays":$maxdays$,"color":$settrueifneededtobecolorfull$})
}
```
### روش log کردن اطلاعات:
برای اینکه شماره خط و نام فایل را در فایل log قرار دهیم، میتوانیم از تابع 'EnableFuncCallDepth' بصورت زیر استفاده کنیم. این تابع بصورت پیشفرض غیرفعال بوده و برای فعال کردن آن میتوان آنرا بصورت زیر صدا زد:
	
	
  ```go
log.EnableFuncCallDepth(true)
```
در صورتی که برنامه برای log کردن نیاز به گذشتن از چند stack frame داشته باشد، میتوان از دستور زیر استفاده کرد:
  ```go
log.SetLogFuncCallDepth($numberofstackframestobeskipped$)
```
### گرفتن log بصورت asynchronously:
برای این کار تنها لازم است از تابع `.Async()` استفاده کرد تا log ها بصورت همزمان گرفته نشوند.

### پیکربندی providerها:
هر provider اعم از `console`، `file`، ‍`multifile`‍، `conn`، `smtp`، ‍`ElasticSearch`، ‍`JianLiao` یا `Slack` تنها یک مجموعه‌ای از configها را پشتیبانی میکند.
	
- console:
	
میتواند output level را مانند کد زیر تنظیم کند. برای حالت default از `os.Stdout` استفاده میکند:
  ```go
log.SetLogger(logs.AdapterConsole, `{"level":$level$}`)
```

- file:
	
میتواند با توجه به پارامتر‌های متفاوت تغییرات لازم را بصورت کد زیر اجرا کند:
  ```go
log.SetLogger(logs.AdapterFile, `{$parameter$:$paraminput$}`)
```
	
در صورتیکه خروجی log برای یک فایل باشد، پارامترهای زیر قابل استفاده میباشند:


`filename`:<div dir="rtl"> برای ذخیره نام فایل</div>
	
`maxlines`:<div dir="rtl"> برای تنظیم حداکثر تعداد خطوط در هر فایل
</div>
	
`maxsize`:<div dir="rtl"> برای تنظیم حداکثر سایز هر فایل	
 </div>
	
`daily`:<div dir="rtl"> برای روزانه log گرفتن
</div>
	
`maxdays`:<div dir="rtl"> برای تنظیم حداکثر روزهایی که فایل log ذخیره میماند
</div>
	
`rotate`:<div dir="rtl"> برای فعال سازی و غیر فعال سازی log rotate
</div>
	
`level`:<div dir="rtl"> برای تنظیم log level که در حالت default برابر trace است
</div>
	
`perm`:<div dir="rtl"> تنظیم permission برای log file
</div>
در صورتیکه provider بصورت multifile باشد، در کنار پارامتر های بالا میتوان از پارامتر `separate` نیز استفاده کرد که log level را درون یک json array مینویسد.

- conn:
	
دارای پارامتر های متفاوت است و کد آن بصورت زیر اجرا میشود:
	
  ```go
 log.SetLogger(logs.AdapterConn, `{"param1":"param1input","param2":"param2input"}`)
```
	
پارامتر های این provider بصورت زیر هستند:
	
`reconnectOnMsg`: <div dir="rtl">
	در صورتیکه این گزینه فعال باشد، پس از هربار فرستادن یا دریافت کردن یک پیام، connection بسته و باز میشود. این option در 
	حالت default </div>غیر فعال است

`reconnect`: <div dir="rtl">
	در صورتیکه `true` باشد مانند autoconnect عمل میکند و زمانیکه disconnect شود، دوباره reconnect میکند
</div>
	
`net`: <div dir="rtl">
	نوع connection را اعم از `tcp`، `udp` یا ... انتخاب میکند
</div>
	
`addr`: <div dir="rtl">
	آدرس اتصال net را مشخص میکند
</div>

`level`: <div dir="rtl">
	مانند `level` در `file`، log level را تنظیم میکند که در حالت default بصورت trace میباشد.
</div>

- stmp:

این provider برای تهیه log توسط email است که دستور آن بصورت زیر میباشد:
	
  ```go
log.SetLogger(logs.AdapterMail, `{"username":"$emailaddress$","password":"$password$","host":"$mailprovider$","sendTos":["$receiveraddress$"]}`)
```
	
پارامتر های این provider بصورت زیر هستند:
	
`username`: sender's email

`password`: sender's password
	
`host`: email host
	
`sendTos`: receiver's email

`subject`

`level`: <div dir="rtl">
	مانند `level` در `file` و `conn`، log level را تنظیم میکند که در حالت default بصورت trace میباشد.
</div>

- ElasticSearch:

این provider کمک میکند تا log مورد نظر به فرمت ElasticSearch تعییر یابد که دستور آن بصورت زیر است:
	
  ```go
log.SetLogger(logs.AdapterEs, `{"dsn":"http://localhost:9200/","level":$requiredlevel$}`)
```
	
- JianLiao:

این provider کمک میکند تا log مورد نظر به فرمت JianLiao تعییر یابد که دستور آن بصورت زیر است:
	
  ```go
log.SetLogger(logs.AdapterJianLiao, `{"authorname":"$authorname$","title":"$title$", "webhookurl":"$url$", "redirecturl":"$redirecturl$","imageurl":"$imageurl$","level":$requiredlevel$}`)
```
	
- Slack:

این provider کمک میکند تا log مورد نظر به فرمت Slack تعییر یابد که دستور آن بصورت زیر است:
	
  ```go
log.SetLogger(logs.AdapterSlack, `{"webhookurl":"$url$","level":$requiredlevel$}`)
```

### سایر format های log گرفتن:
در بیگو، log گرفتن با فرمت‌های متفاوت با override کردن توابع امکان پذیر است و میتوان format یک log را متناسب با adaptor تغییر داد. دستور این عمل بصورت زیر میباشد:
	
  ```go
log.SetLoggerWithOpts("adapterName", []string{'{"key":"value"}'}, utils.KV{Key:"formatter", Value: formatterFunc})
```

## config module:
این ماژول نیز مانند [log module](https://github.com/NikanV/Beego/blob/main/Introduction/LogModule.md) با الهام گیری از `database/sql` برای parse کردن فایل‌های config با فرمت‌های `ini`، `json`، `xml` و `yaml` استفاده میشود. 
برای install کردن این module میتوان از دستور زیر استفاده کرد:
	
  ```go
go get github.com/beego/beego/v2/core/config
```
	
در صورتی که قصد parse کردن فایل‌های `yaml` یا `xml` را دارید، نخست باید `beego/v2/core/config/xml` را نصب کرده و سپس آنرا import کنید. امکان نصب آن با دستور زیر:
	
  ```bash
go get -u github.com/beego/beego/v2/core/config/xml
```
‍ 	
و امکان import آن با دستور زیر میباشد:
	
  ```go
import _ "github.com/beego/beego/v2/core/config/xml"
```

از ورژن 2.x به بعد، beego امکان ساخت یک `global instance` از config module را به کاربران میدهد که بتوانند از آن بصورت مستقیم استفاده کنند. ساخت این instance را میتوان بصورت زیر:
	
  ```go
val, err := config.String("mykey")
``` 
	
یا بصورت زیر ساخت:

  ```go
err := InitGlobalInstance("toml", "$someconfig$")
val, err := config.String("mykey")
```
	
در صورتی که instance بصورت دوم ساخته شود، باید بسته `toml` را نیز import کرد که این کار را میتوان بصورت زیر انجام داد:

	
  ```go
_ import "github.com/beego/beego/v2/core/config/toml"
```
	
بیگو با load کردن فایل config از `config/app.conf` و پیاده سازی `ini` که یک فرمت text-based فایل config است که مقادیر را بصورت `key:value` نگه‌داری میکند، فایل‌های config خود را ذخیره میکند. کاربران قادر به parse کردن فایل config خود در حالت manual بصورت زیر نیز هستند:
	
  ```go
iniconf, err := NewConfig("ini", "$configfilename$")
if err != nil {
	t.Fatal(err)
}
iniconf.String("$appname$")
```
		
که میتوانند data خود را پس از parse شدن از `iniconf.String` دریافت کنند. methodهای parser بصورت زیر هستند:
	
  ```go
// Configer defines how to get and set value from configuration raw data.
type Configer interface {
    // support section::key type in given key when using ini type.
    Set(key, val string) error

    // support section::key type in key string when using ini and json type; Int,Int64,Bool,Float,DIY are same.
    String(key string) (string, error)
    
    // get string slice
    Strings(key string) ([]string, error)
    Int(key string) (int, error)
    Int64(key string) (int64, error)
    Bool(key string) (bool, error)
    Float(key string) (float64, error)
    
    // support section::key type in key string when using ini and json type; Int,Int64,Bool,Float,DIY are same.
    DefaultString(key string, defaultVal string) string
    
    // get string slice
    DefaultStrings(key string, defaultVal []string) []string
    DefaultInt(key string, defaultVal int) int
    DefaultInt64(key string, defaultVal int64) int64
    DefaultBool(key string, defaultVal bool) bool
    DefaultFloat(key string, defaultVal float64) float64

    // DIY return the original value
    DIY(key string) (interface{}, error)

    GetSection(section string) (map[string]string, error)

    Unmarshaler(prefix string, obj interface{}, opt ...DecodeOption) error
    Sub(key string) (Configer, error)
    OnChange(key string, fn func(value string))
    SaveConfigFile(filename string) error
}
```

در صورتی که از متدهای `default*` استفاده شود و `key` ورودی پیدا نشود، `value` خروجی مقدار تعریف شده ‍default میباشد.

برای return کردن `original value`، میتوان از `DIY` استفاده کرد. فقط باید دقت به value type داشت.

برای دریافت موارد متفاوت در هر section، میتوان از `GetSection` استفاده کرد. (section در هر implementation معنی متفاوتی دارد)

برای تبدیل configها به object، میتوان از `Unmarshaler` استفاده کرد که پارامتر `prefix` آن مانند `section` است.

برای اینکه نتیجه را بصورت یک instance از Configer بسته بندی کنیم، میتوانیم از ‍`Sub` استفاده کنیم که مانند GetSection عمل میکند.

برای اینکه از تغییر حالتی مطلع شویم، میتوانیم از `Onchange` استفاده کنیم که برای ما listen میکند. اما باید دقت داشت که اکثر implementationهای بر پایه فایل این method را پشتیبانی نمیکنند.

برای ذخیره config در فایل میتوان از متد `SaveConfigFile` استفاده کرد.
	
همه‌ی implementationها هر ورودی را مانن `x.y.z` بعنوان key پشتیبانی نمیکنند.
	
در فایل های `ini` از آنجاییکه config-section پشتیبانی میشود، میتوان value برای هر section را بوسیله key آن بصورت `secion::key` بدست آورد که دستور آن نیز بصورت زیر میباشد:
	
  ```go
key1 = $firstkey$
key2 = $secondkey$
iniconf.String("demo::key2")
```
	
با توجه به community فعال beego، پس از pull-request از issue #1636، امکان دریافت `environment variable` ها در config file های beego فراهم گردید. فرمت آن در فایل config بصورت `${ENVIRONMENTVARIABLE}` میباشد که به همان معنای ‍`value = os.Getenv('ENVIRONMENTVARIABLE')` است. میتوان به environment variableها یک value نیز نسبت داد که syntax آن بصورت `${ENVVAR||defaultvalue}` میباشد. پس برای اینکه در beego از environment value تعریف شود، کافیست آنرا به فرمت زیر بنویسیم:
	
  ```go
	envvar1 = ${environmentvariable1||val1}
	envvar2 = ${environmentvariable2||val2}
```	

که امکان دریافت value1 از envvar1 بالا بصورت `beego.AppConfig.String("envvar1")` میسر میشود (برای envvar2 نیز به همین صورت است).
	

## admin module:
این ماژول بر پایه [Dropwizard](https://github.com/dropwizard/dropwizard) از Java طراحی شده است که امکان های زیر را میدهد:
	
1- `health check`
2- `profiling`
3- `statistics`

برای install این ماژول تنها کافیست دستور زیر را در terminal خود تایپ کنید:
  ```bash
go get github.com/beego/beego/v2/core/admin
```

### 1- چک کردن سلامت برنامه (health check):
این قابلیت امکان چک کردن health status برنامه را به admin میدهد.
	
برای این بخش میتوان بالا بودن database را با مثال زیر نمایش داد:

نخست تابع check را بصورت زیر تعریف میکنیم:
	
  ```go
type DatabaseCheck struct {
}

func (dc *DatabaseCheck) Check() error {
	if dc.isConnected() {
		return nil
	} else {
		return errors.New("can't connect database")
	}
}
```
	
سپس میتوان آنرا بعنوان یک check item به برنامه اضافه کرد و به آن request برای چک کردن بالا بودن database داد:

  ```go
admin.AddHealthCheck("database",&DatabaseCheck{})
```
	
### 2- ساخت profile:
مانیتور کردن پروسه‌های برنامه یکی از راه‌های مناسب برای اطلاع از عملکرد آن میباشد. بیگو با امکان profiling این قابلیت را به کاربر میدهد که یک entry point به بخش‌های مختلف برنامه برای debug داشته باشد. admin module با استفاده از `ProcessInput` در profiling به عنوان تابع entry، درخواست ها را process میکند. برای مثال میتوان موارد زیر را lookup کرد:
	
`goroutine`, `heap`, `threadcreate`, `block`
	
### 3- مشاهده statistics:
در ماژول ادمین امکان مشاهده statistics برنامه نیز قرار داده شده است. برای این کار نخست لازم هست statistic ها را اضافه کرده و سپس با صدا زدن تابع زیر، از شرایط آنها باخبر شویم:
  ```go
admin.StatisticsMap.GetMap(os.Stdout)
```
	
این دستور به ما field‌های زیر را بعنوان خروجی میدهد:

`requestUrl`, `method`, `times`, `used`, `max used`, `min used`, `avg used`
	
	
	
[بازگشت](https://github.com/NikanV/Beego/blob/main/Introduction/README.md)

</div>
