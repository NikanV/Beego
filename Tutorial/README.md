<div dir="rtl">

## آموزش راه اندازی و ساخت یک پروژه با فریمورک بیگو

### 📝فهرست
 - [پیش نیاز ها]()
 - [ساختن اولین برنامه]()
 - [ساختن controller و route]()


## پیش نیاز ها
- زبان برنامه نویسی GO

برای نصب آن می توانید از این [وبسایت](https://go.dev/dl/) اقدام کنید.
توجه کنید که حتما Environment Variable مربوط به GOPATH را در کامپیوتر خود تنظیم کنید.

در linux با اجرای دستور زیر:

```bash
export PATH=$PATH:/usr/local/go/bin
```

و در windows با رفتن به تنظیمات Environment Variables و اضافه کردن یک Variable به اسم `GOPATH` و با مقدار `%USERPROFILE%\go`
می توانید این کار را انجام دهید.

- کتابخانه های beego و bee

برای نصب کتابخانه beego می توانید از دستور زیر استفاده کنید:

```bash
go install github.com/beego/beego/v2@latest
```

و برای نصب کتابخانه bee هم می توانید از دستور زیر استفاده کنید:

```bash
go install github.com/beego/bee@latest
```

اگر همه مراحل را درست انجام داده باشید با استفاده از دستور `bee` می توانید توضیحات مربوط به دستورات آن را مشابه عکس زیر ببینید.

<p align=center><img src="./src/images/bee_commands.png" width=500 /></p>

## ساختن اولین برنامه
ابتدا به فولدری که می خواهید در آن پروژه را بسازید بروید. به عنوان مثال اسم فولدر من `demo_project` هست.

سپس با استفاده از دستور زیر می توانید پروژه خود را بسازید.

```bash
bee new your_project_name
```

و به جای `your_project_name` هر اسمی که می خواهید برای پروژه خود انتخاب کنید را قرار دهید.
پس از اجرای دستور بالا پیغامی مشابه زیر دریافت خواهید کرد.

<p align=center><img src="./src/images/bee_new_project.png" width=500 /></p>

اکنون اگر به فولدر ساخته شده با اسمی که انتخاب کردید بروید، فایل های پروژه را می بینید که توسط beego  برای شما ساخته شده اند.

<p align=center><img src="./src/images/bee_list.png" width=500 /></p>

بعد از ساخت پروژه کافی است ابتدا کتابخانه های مربوطه را بریزید که به راحتی با دستور زیر قابل انجام است:

```bash
go mod tidy
```

می توانید در این [لینک](https://go.dev/ref/mod#go-mod-tidy) توضیحات مربوط به دستور بالا را ببینید.

پس از نصب کتابخانه های مربوطه می توانید با استفاده از دستور زیر پروژه خود را اجرا کنید:

```bash
bee run
```

وقتی پروژه اجرا می شود، صفحه ای مانند صفحه زیر خواهید داشت:

<p align=center><img src="./src/images/bee_run.png" width=500 /></p>

پروژه به صورت پیش فرض روی پورت 8080 مربوط به localhost بالا می آید.
پس با رفتن به URL زیر می توانید نمای اولیه سایت خود را ببینید.

```http
http://localhost:8080/
```

<p align=center><img src="./src/images/bee_site.png" width=500 /></p>

در ادامه خواهیم دید که چه طور به پروژه خود controller، view و خیلی قابلیت های دیگر اضافه کنیم.

## ساختن controller و router 

برای اضافه کردن یک controlller کافی است به فولدر مربوط به آن بریم و یک فایل با پسوند `.go` بسازیم.

<p align=center><img src="./src/images/controller_make.png" width=500 /></p>

در فایل controller ابتدا یک struct تعریف می کنیم و بهتر است اسم آن مشابه اسم فایلی که ساختیم باشد و داخل آن `beego.Controller` را به عنوان پدر می نویسیم زیرا که هر controller ی که می سازیم باید فرزند `beego.Controller` باشد. پس در واقع داریم struct embedding انجام می دهیم.

```golang
type DemoController struct {
	beego.Controller
}
```

برای مطالعه بیشتر struct embedding می توانید از این [لینک](https://gobyexample.com/struct-embedding) استفاده کنید.

سپس به عنوان مثال یک struct از کارمندان ساختیم به نام `Employee` که دارای سه تا فیلد نوشته شده هست.
سپس برای controller ی که بالاتر تعریف کردیم یک تابع می نویسیم که برای ما مشخصات کارمندان را با فرمت json بر می گرداند.

```golang
type Employee struct {
	Id        int    `json:"id"`
	FirstName string `json:"firstName"`
	LastName  string `json:"lastName"`
}

func (dc *DemoController) GetEmployees() {
	dc.Ctx.ResponseWriter.WriteHeader(200)
	dc.Data["json"] = employees
	dc.ServeJSON()
}
```

برای مطالعه درباره نحوه نوشتن struct های مناسب با json و کار کردن با json در زبان GO می توانید از این [لینک](https://gobyexample.com/json) و این [لینک](https://go.dev/blog/json) استفاده کنید.

در آخر هم یک تابع به نام init داریم که این تابع هنگام ساخته شدن این controller صدا زده می شود، پس در واقع مقادیری را که لازم است در ابتدا مقدار دهی کنیم می توانیم در این تابع قرار دهیم.

```golang
func init() {
	employees = Employee{Id: 1, FirstName: "De", LastName: "Mo"}
}
```

برای اضافه کردن router هم کافی است به فایل `router.go` در فولدر `routers` رفته و router مربوط به این controller ی که زدیم را اضافه کنیم.

<p align=center><img src="./src/images/route_make.png" width=500 /></p>

تنها خطی که لازم است اضافه کنیم خط زیر می باشد و کاری که انجام می دهد این است که به router می گوید که اگر در url آدرس /employess نوشته شده بود با استفاده از controller ی که به عنوان دومین آرگومان به آن پاس داده شده و تابعی از همین controller که به عنوان سومین آرگومان به آن پاس داده شده برای من یک response برگرداند.

```golang
	beego.Router("/employees", &controllers.DemoController{}, "get:GetEmployees")
```

همچنین در آرگومان سوم نوشتیم `get:GetEmployees` و می دانیم که طرف راست `:` همان اسم تابع است. اما طرف چپ آن در واقع نوع درخواست http ما است که می توانید با مراجعه به این [لینک](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods) بیشتر درمورد آنها مطالعه کنید.

اکنون اگر دوباره پروژه را اجرا کنیم و به URL زیر برویم با همچین صفحه ای که در واقع همان response تابع `GetEmployees()` هست مواجه می شویم.

```http
http://localhost:8080/employees
```

<p align=center><img src="./src/images/bee_site_2.png" width=500 /></p>

به همین صورت می توانیم هر controller ی که می خواهیم بسازیم و با router ها به هر URL ی که می خواهیم یک handler و یا تابعی از یک controller را نسبت بدهیم.

## ساختن view


</div>