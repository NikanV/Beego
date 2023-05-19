<div dir="rtl">

## ۲. تسک‌ها (tasks):
تسک‌ها بسیار شبیه `cron` در سیستم‌های unix based کارها را در زمان های تنظیم شده خارج از مراحل request و response بصورت regular اجرا میکنند. برای install تسک کافیست خط زیر را execute کنید:
  	
  ```bash
go get github.com/beego/beego/v2/task
```

برای ساخت task میتوان از دستور زیر استفاده کرد:
  
  ```go
tk1 := task.NewTask($name$,$taskformat$, $TaskFunc$) *Task	
```
  
برای implement کردن taskها میتوان آنها را به لیست global taskها اضافه کرد و سپس آنرا start کرد:
  
  ```go
  task.AddTask("tk1", tk1)
  task.StartTask()
  defer task.StopTask()
```
  
برای تست task function نیز میتوان از تابع زیر استفاده نمود:
  
  ```go
  err := tk.Run()
  if err != nil {
    t.Fatal(err)
  }
```
  
برای زمانیکه میخواهیم task جدید را اجرا کنیم، از `spec` استفاده میکنیم که format آن مانند crontab میباشد.
  
  برای debug برنامه، beego از توابع `Display` و `GetDisplayString` استفاده میکند. هر دوی این توابع ورودی‌ها را بصورت key,value دریافت میکنند ولی تابع display نتیجه را در console نمایش میدهد و getdisplaystring نتیجه را بصورت string برمیگرداند.

  
    [بازگشت](https://github.com/NikanV/Beego/blob/introbranch/Introduction/Intro.md)

</div>
