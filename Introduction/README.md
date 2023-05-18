<div dir="rtl">

## آشنایی با فریمورک بیگو
  چهارچوب‌های وب مجموعه‌ای از ابزارها و کتابخانه‌ها برای توسعه راحت‌تر و سریعتر برنامه‌های وب هستند. این `framework` ها با استفاده از الگوها و ساختارها در قالب معماری‌های متفاوت به کاربر کمک میکنند که `application` خود را سازماندهی کرده و با برنامه‌ی خود در لایه‌های مجزا کار کند. از آنجاییکه این چهارچوب‌ها دارای بخش‌های مشترکی از توسعه و `test` برای برنامه‌ی وب هستند، این امکان به توسعه‌دهنده داده میشود که برنامه خود را کارآمد تر بنویسد.
  
  چهارچوب `beego` یک `opensource web framework` در زبان `go` بر پایه معماری `mvc (models, views & controllers)` میباشد. این `framework` برای توسعه برنامه‌ها در مقیاس صنعتی نوشته شده است. این `framework` از ۴ بخش تهیه شده است:

### ۱. ماژول‌های پایه (`base modules`):
  #### آ- `session modules`:
  این ماژول برای مدیریت داده `session` کاربر در برنامه استفاده میشود و این امکان را فراهم میسازد که اطلاعات مربوط به `session` را به کاربر میدهد.
  برای اضافه کردن این `module` میتوان از دستور زیر استفاده کرد:
  ```bash
go get github.com/beego/beego/v2/server/web/session
```
  پس از نصب آن تنها لازم است که آنرا در پروژه خود بصورت زیر `import` کنید:
  ```bash
import (
	"github.com/beego/beego/v2/server/web/session"
)
```
  سپس کافیست یک `global variable` برای `manager` آن `session` تعریف کنید:
  ```bash
  var globalSessions *session.Manager
```
  در نهایت پس از `init` کردن `session` بصورت زیر، میتوان از `method` های آن نظیر
  
  `SessionStart`، `SessionDestroy`، `SessionRegenerateId`، `GetActiveSession`، `SetHashFunc`و `SetSecure` 
  
  استفاده کرد:
  ```bash
  func init() {
	globalSessions, _ = session.NewManager("memory", `{"cookieName":"gosessionid", "enableSetCookie,omitempty": true, "gclifetime":$lifetime$, "maxLifetime": $maxlifetime$, "secure": false, "sessionIDHashFunc": $hashfuncid$, "sessionIDHashKey": "$hashkeyid$", "cookieLifeTime": $cookielifetime$, "providerConfig": ""}`)
	go globalSessions.GC()
}
```
 ##### الف) `sessionStart`: این `method` یک `session object` با توجه به درخواست کنونی بر میگرداند.
  ##### ب) `sessionDestroy`: این `method` برای نابود کردن `session object` کنونی است.
  ##### ج) `SessionRegenerateId`: این `method` برای `regenerate‍` کردن `id` برای `session` است.
  ##### د) `GetActiveSession`: کاربر `session` فعال را برمیگرداند.
  ##### ه) `SetHashFunc`: تابع `generate` کننده `id` برای `session` را تنظیم میکند.
  ##### و) `SetSecure`: برای فعال و غیرفعال سازی `secure cookie` میباشد.
  
  

</div>
