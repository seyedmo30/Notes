### concurrency vs parallelism 

در کانکارنت می توان یک سرویس را شتکست و به چند زیر سرویس تبدیل کرد و هر کدام را همزمان انجام داد اما در پارالل یک سرویس کامل را همزمان اجرا میکینم . در پارالل هر سرویس بر روی یک ترد پراسس می شود در حالی که در کانکارنت چندین تسک می توان بر روی یک ترد پراسس شود همچنین در کانکارنت ترتیب و نظم اهمیتی ندارد


mutex

در بیشتر مثال ها داده درون یک استراکت ، همراه mutex است 


type SafeCounter struct {
	mu sync.Mutex
	v  map[string]int
}

همچنین getter setter آن ، ریسیور هایی هست که دسترسی به داده دارند . همچنین می توان از ... برای این قابلیت که همزمان بتوان داده را چند گوروتین خواند ولی تنها یک گوروتین بتوان آن را تغییر داد ، استفاده کرد .

# انواع چنل ها
## buff
### unbuffer 
+ زمانی که بخواهیم داده سینک دریافت کنیم توجه شود هیچ ظرفیتی ندارد یعنی حتی یک هم نیست و تنها زمانی می تونیم توش بزاریم که قبلش یکی رسیو کرده باشه

+ زمانی که مطمعنیم پاسخ گرفته شده از چنل ، مخصوص در خواست است ( توجه شپد در چنل بافر استیت لس است و نمی دانیم مسیج مخصوص کدام درخواست است، مگر این مه متا دیتا داشته باشد) 

+ می توان به عنوان سیگنال بین گوروتین ها استفاده کرد

+ در کل مخصوص زمانی است که می خواهیم داده ای مشخص را بگیریم ، اما زمان دریافت را نمی دانیم ، به جای انتظار برای یم متغییر از یک فانکشن ، منتظر چنل می مانیم و می توانیم سیاست هایی مانند تایم اوت یا ریپلای گذاشت


  توجه: چنل آنبافر  یعنی طول صفر است ! یعنی اگر به چنل آنبافر بعد از ساختن ، مقدار دهیم ددلاک می شود! 
  
  تنها در صورتی می توان استفاده کرد که در گوروتین جدا ابتدا منتظر برداشتن از چنل باشیم و به محض ریختن در چنل آنبافر، آن برداشته شود . در نتیحه ابتدا باید منتظر باشیم سپس داده در آن بریزیم .


### buffered 

+ نیاز به گرفتن داده به صورت همزمان نباشد - آسینک

+ زمانی استفاده میشود که که حجم زیادی مسیج که ترتیب آن ها برای ما اهمییتی ندارد استفاده می شود

### Directional
+ send-only - chan<- - از دید کلاینت ، فقط می توان به چنل داده فرستاد
+ recevie-only - <-chan - از دید کلاینت ، فقط می توان از چنل داده دریافت کرد

### receive channel ? return ? or pass as parameter
می توانیم چنل خروجی تابع  را یا در ریترن بزاریم ، یا در پارامتر های ورودی

+ Return the channel directly as a return value
در این حالت مدیریت چنل دست فانکشن است ، احتمالن فانکشن عمری طولانی خواهد داشت و در پایان برنامه تابعد قبل از اتمام می تواند چنلی که ساخته را کلوز کند .

+ Pass the channel as a parameter:
در این حالت مدیریت چنل بر عهده ی سوپر فانکشن است ، احتمالن هر بار فانکشن ساخته شده و کارش به پایان می رسد ، بیشتر برای زمانی که می خوهیم برای هر در خواست یک فانکشن بسازیم

فقط توجه کنید در این صورت اگر ارتباط بین فانکشن و سوپر فانکشن قطع شود ، مدیریت آن خیلی سخت می شود ، بهتر است یک چنل برای ارتباط با سوپر فانکشن بگذاریم

### نمی توان گوروتین رو kill کرد

توجه شود ، هیچ راهی برای کیل کردن گوروتین نیست ، تنها راه این است که آن را مدیریت کرد و بهش اطلاع داد که باید تمام شود 

بهتر است داده هایی که مشخص نیست کی بدست میاد رو داخل چنل بریزیم و در این حالت منتظر بمونیم کانتکس دان میشه یا جواب میاد

بهترین راه ارسال done - contect است - هر جا که احساس کردیم برنامه متوقف می شود یعنی یا منتظر سرویس بیرونی است یا .... بهتر است از راه زیر استفاده کنیم :

```

		select {
		case <-ctx.Done():
			fmt.Println("Worker: Context canceled, exiting...")
			return
		case <-ch1:
		...
```


### receive data from chan
به طور کلی به ۳ روش می توان از چنل داده برداشت کرد :
+ ساده
  

		 x := <-ch 
		 fmt.Println(x)
	  
+ انتخاب بین چند چنل

	      for {
	        select {
	        case x, ok := <-ch1:
	            if ok {
	                fmt.Println("Received from ch1:", x)
	            } else {
	                fmt.Println("ch1 closed")
	                ch1 = nil // set ch1 to nil to stop receiving from it
	            }
	        case x, ok := <-ch2:
	  

+ در حلقه ی 

		for i := range ch {
		    fmt.Println(i)
		}

سوالات خوب مصاحبه :

https://www.tutorialspoint.com/articles/category/go-programming


### نکات

تقریبا یه هفته توی مانی یه مشکلی پیش اومده بود و این نتیجه یه هفته زیر و رو کردن کده:

نیاز داشتم چنل رو پیک کنم یعنی چنل رو بخونم بدون اینکه از تو چنل بردارم و به این راه حل غلط رسیدم :

```
			
 res := <-w.workQueue
			
 w.workQueue <- res

```		
در این حال از چنل می گیریم و بعد از گرفتن کار انجام میدیم و دوباره در چنل میریزیم ، ایراد اول : اینکه داده بعد از خواندن ته صف می ره و احتمال داره داده تا سال ها خوانده نشه ، یعنی به نوبتش که رسید بره ته صف . ایراد دولم اینه در صورتی که چنل بخواد به سرعت پر شه ، همون لحظه که خونده می شه تا دوباره تو چنل ریخته شه ، یه داده دیگه توش میشینه و تو همون مرحله قفل میشه

پس این کار رو نکنید