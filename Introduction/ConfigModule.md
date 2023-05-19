<div dir="rtl">

## ب- config modules:
این ماژول نیز مانند [log module]() با الهام گیری از `database/sql` برای parse کردن فایل‌های config با فرمت‌های `ini`، `json`، `xml` و `yaml` استفاده میشود. 
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
	
    [بازگشت](https://github.com/NikanV/Beego/blob/introbranch/Introduction/BaseModules.md)

</div>
