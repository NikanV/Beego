<div dir="rtl">

## آ- object related mapping module:
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
 
  
  [بازگشت](https://github.com/NikanV/Beego/blob/main/Introduction/README.md)

</div>
