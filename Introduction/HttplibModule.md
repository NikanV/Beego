<div dir="rtl">

  ## ب- httplib module:
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

  
  [بازگشت](https://github.com/NikanV/Beego/blob/main/Introduction/README.md?)

</div>
