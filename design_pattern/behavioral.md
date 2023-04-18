
# observer

اگر بخواهیم فانکشن یا کلاسی از تغییرات یا آپدیت شدن کلاسی دیگر مطلع شود از این دیزاین پترن استفاده می کنیم

به تعریف دیگر اگر بخواهیم بعد از آپدیت شدن یک تابع، پیام یا داده ای به دیگر کلاس ها داده شود از ابزرور استفاده می کنیم

مثلا سیگنال در جنگو از این دیزاین پترن استفاده می کند

در اینجا ۲ مفهوم داریم  :

Subject یا پابلیشر

Observer یا سابسکرایبر

در سابجکت لیستی وجود دارد که هر ابزرور که بخواهد تغییرات را مشاهده کند ، در آن اضافه می کنیم

همچنیندر در ابزرور ها باید متدی مانند آپدیت باشد تا هر بار در سابجکت تغییر صورت گرفت ، این متد را صدا بزند و آبزرور متوجه تغییر بشود

https://refactoring.guru/design-patterns/observer/go/example

# strategy 

زمانی که برای انجام کاری ، چند الگوریتم متفاوت داریم و با توجه به ورودی ، باید یکی از الگوریتم ها را انتخاب کنیم ، از دیزاین پترن استراتجی استفاده می کنیم . 

فرض کنیم برای پرداخت الکترونیکی چند درگاه با متد های مختلف داریم و کلاینت می تواند یکی را انتخاب کند . شاید الگوریتم ها متفاوت باشد ولی روند کلی همه یکسان است.

ساختار :

یک اینترفس استراتجی داریم که امضای متد مشترک تمام الگوریتم ها را دارد

تمام متد ها باید اینترفیس را پیاده سازی کنند

کانتکست ، گرفتن اسم متد مشخص از کلاینت و اجرای متد های پیاده سازی شده

کلاینت یا متد اصلی

https://refactoring.guru/design-patterns/strategy/go/example

# iterator

در صورتی که تعدادی داده داشته باشیم و بخواهیم آن را پیمایش کنیم از این دیزاین پترن استفاده می کنیم .

گاهی ساختار کالکشن داده ی ما به سادگی لیست نیست ، مثلا درختی است ، در این صورت می توانیم نحوه ی پیمایش درختی دلخواه خود را پیاده کنیم ( مثلا بر اساس عمق درخت پیمایش کند یا فرزند خوشه )


مثلا مجموعه ای سطر از دیتابیس گرفته ( به صورت پیور) و حال می خواهیم آن را پیمایش کنیم .


https://refactoring.guru/design-patterns/iterator/go/example