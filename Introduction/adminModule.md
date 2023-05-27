<div dir="rtl">

## پ- admin module:
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
	
	
	
	
[بازگشت](https://github.com/NikanV/Beego/blob/introbranch/Introduction/BaseModules.md)

</div>
