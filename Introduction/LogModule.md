<div dir="rtl">
  
## آ- log modules:
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

	
[بازگشت](https://github.com/NikanV/Beego/blob/main/Introduction/BaseModules.md)

	
</div>
