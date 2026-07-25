# 📊 Olist Sales Analytics Dashboard Documentation

## 📑 Table of Contents

-   Project Overview
-   Business Problem
-   KPI Dictionary
-   Solution Architecture
-   Data Profiling
-   Data Cleaning
-   Data Modeling
-   DAX Measures

> \[!NOTE\] This document was generated from the supplied project notes
> and formatted for GitHub.

------------------------------------------------------------------------

تحلیل عملکرد فروش، لجستیک و رضایت مشتری در پلتفرم Olist ۱. تعریف مسئله
بیزینسی Situation --- وضعیت موجود Olist یک پلتفرم مارکت‌پلیس برزیلی است
که فروشندگان کوچک و متوسط را به مشتریان سراسر کشور متصل می‌کند. این
پلتفرم بین سال‌های ۲۰۱۶ تا ۲۰۱۸ بیش از ۱۰۰ هزار سفارش را پردازش کرده است.
Complication چالش مدیریت ارشد داده‌های خام را در قالب فایل‌های پراکنده
(سفارش، پرداخت، نظرات، لجستیک) در اختیار دارد، اما دید یکپارچه‌ای از
عملکرد فروش، کیفیت تحویل و رضایت مشتری ندارد. تصمیم‌گیری‌ها بدون داشبورد و
بر پایه حدس انجام می‌شود. Business Question سوال اصلی کدام عوامل
(جغرافیا، زمان تحویل، دسته محصول، روش پرداخت) بیشترین تاثیر را روی رضایت
مشتری و درآمد دارند، و چه اقداماتی می‌تواند نرخ رضایت و بازگشت مشتری را
افزایش دهد؟ طراحی داشبورد: برای مدیر عامل: دغدغه اصلی: روند کلی درآمد و
رشد نیاز داشبورد: KPIهای کلان، مقایسه دوره‌ای، خلاصه اجرایی برای مدیر
لجستیک: دغدغه اصلی: تاخیر تحویل، عملکرد فروشندگان نیاز از داشبورد: نقشه
جغرافیایی، زمان تحویل، گلوگاه‌ها برای CRM: دغدغه اصلی: رفتار و وفاداری
مشتری داشبورد: تحلیل RFM، Cohort، نرخ بازگشت مشتری مدیر محصول: دغدغه
اصلی: عملکرد دسته‌بندی‌ها داشبورد: فروش و سود به تفکیک دسته محصول

KPI Dictionary: Total Revenue: مجموع درآمد از آیتم‌های فروخته‌شده فرمول:
SUM(price + freight_value)\
هدف: اندازه‌گیری رشد کلی AOV: میانگین ارزش هر سفارش فرمول: Total Revenue
÷ Total Orders هدف: سنجش رفتار خرید Delivery Delay: تاخیر نسبت به تخمین
فرمول: تاریخ تحویل واقعی -- تخمینی هدف: سنجش کیفیت لجستیک Review Score
Avg: میانگین امتیاز نظرات فرمول: AVG(review_score) هدف: سنجش رضایت مشتری
Repeat Purchase Rate: درصد مشتریان با بیش از یک خرید فرمول: مشتریان
تکراری ÷ کل مشتریان هدف: سنجش وفاداری RFM Segment: سگمنت مشتری بر اساس
فرمول: Recency/Frequency/Monetary\
هدف: هدف‌گذاری بازاریابی

Solution Architecture: ● فایل‌های CSV خام Olist (۹ جدول مرتبط) ● پاکسازی
و یکپارچه‌سازی داده در PostgreSQL یا Python/Pandas ● ETL نهایی و تبدیل در
Power Query ● مدل‌سازی داده به شکل Star Schema در Power BI ● ساخت لایه
محاسباتی با DAX متریک‌های پایه و پیشرفته ● طراحی داشبورد تعاملی چندصفحه‌ای
● انتشار روی Power BI Service

Data Profiling Report جدول: orders نام ستون نوع داده Null % Cardinality
نمونه مقدار order_id Text 0 ۹۹,۴۴۱ (یکتا)
e481f51cbdc54678b7cc49136f2d6af customer_id Text 0 ۹۹,۴۴۱ (یکتا)
9ef432eb6251297304e76186b10a928 order_status Text 0 ۸ مقدار delivered /
shipped / canceled... order_purchase_timestamp DateTime 0 - 2017-10-02
10:56:33 order_approved_at DateTime 1 - 2017-10-02 11:07:15
order_delivered_carrier_date DateTime 2 - 2017-10-04 19:55:00
order_delivered_customer_date DateTime 3 - 2017-10-10 21:25:13
order_estimated_delivery_date DateTime 0 - 2017-10-18 00:00:00

جدول order_items: نام ستون نوع داده Null % Cardinality نمونه مقدار
order_id Text ۰٪ ۹۸,۶۶۶ e481f51cbdc54678b7cc49136f2d6af order_item_id
Integer ۰٪ 1 الی 21 1 product_id Text ۰٪ ۳۲,۹۵۱
4244733e06e7ecb4970a6e2683c13e61 seller_id Text ۰٪ ۳,۰۹۵
48436dade18ac8b2bce089ec2a041202 shipping_limit_date DateTime ۰٪ -
2017-10-06 11:07:15 price Decimal ۰٪ ۰.۸۵ الی ۶,۷۳۵ 58.90 freight_value
Decimal ۰٪ 0 الی ۴۰۹.۶۸ 13.29

جدول Customers: نام ستون نوع داده Null % Cardinality نمونه مقدار
customer_id Text ۰٪ ۹۹,۴۴۱ (یکتا) 861eff4711a542e4b93843c6dd7febb
customer_unique_id Text ۰٪ ۹۶,۰۹۶ 8d50f5eadf50201ccdcedfb9e2ac8455
customer_zip_code_prefix Integer ۰٪ ۱۴,۹۹۴ 14409 customer_city Text ۰٪
۴,۱۱۹ Franca customer_state Text ۰٪ ۲۷ SP

جدول order_payments: نام ستون نوع داده Null % Cardinality نمونه مقدار
order_id Text ۰٪ ۹۹,۴۴۰ e481f51cbdc54678b7cc49136f2d6af
payment_sequential Integer ۰٪ 1 تا ۲۹ 1 payment_type Text ۰٪ ۵ مقدار
credit_card / boleto / voucher... payment_installments Integer ۰٪ 0 تا
۲۴ 2 payment_value Decimal ۰٪ 0 تا ۱۳,۶۶۴ 99.33

جدول order_reviews: نام ستون نوع داده Null % Cardinality نمونه مقدار
review_id Text ۰٪ \~۹۸,۴۱۰ 7bc2406110b926393aa56f80a40eba40 order_id
Text ۰٪ ۹۸,۶۷۳ e481f51cbdc54678b7cc49136f2d6af review_score Integer ۰٪ 1
تا ۵ 5 review_comment_title Text \~۸۸٪ - "Recomendo"
review_comment_message Text \~۵۹٪ - "Muito bom o produto."
review_creation_date DateTime ۰٪ - 2018-01-18 00:00:00
review_answer_timestamp DateTime ۰٪ - 2018-01-18 21:46:59

جدول products: نام ستون نوع داده Null % Cardinality نمونه مقدار
product_id Text ۰٪ ۳۲,۹۵۱ (یکتا) 1e9e8ef04dbcff4541ed26657ea517e5
product_category_name Text \~۱.۸۵٪ ۷۳ cool_stuff product_name_lenght
Numeric \~۱.۸۵٪ - 40 product_description_lenght Numeric \~۱.۸۵٪ - 287
product_photos_qty Numeric \~۱.۸۵٪ ۱ تا ۲۰ 1 product_weight_g Numeric
\~۰.۰۱٪ - 225 product_length_cm Numeric \~۰.۰۱٪ - 16 product_height_cm
Numeric \~۰.۰۱٪ - 10 product_width_cm Numeric \~۰.۰۱٪ - 14

جدول sellers: نام ستون نوع داده Null % Cardinality نمونه مقدار seller_id
Text ۰٪ ۳,۰۹۵ (یکتا) 3442f8959a84dea7ee197c632cb2df15
seller_zip_code_prefix Integer ۰٪ ۲,۲۴۶ 13023 seller_city Text ۰٪ ۶۱۱
Campinas seller_state Text ۰٪ ۲۳ SP

جدول geolocation: نام ستون نوع داده Null % Cardinality نمونه مقدار
geolocation_zip_code_prefix Integer ۰٪ ۱۹,۰۱۵ 1037 geolocation_lat
Decimal ۰٪ - -23.545621 geolocation_lng Decimal ۰٪ - -46.639292
geolocation_city Text ۰٪ ۸,۰۱۱ sao paulo geolocation_state Text ۰٪ ۲۷ SP

جدول category_name_translation: نام ستون نوع داده Null % Cardinality
نمونه مقدار product_category_name Text ۰٪ ۷۱ (یکتا) beleza_saude
product_category_name_english Text ۰٪ ۷۱ (یکتا) health_beauty

کیفیت داده ها:

پاکسازی جدول Orders: تمامی فیلدهای تاریخ باید نوع Date/Time باشد. ایجاد
کلید تاریخ: order_delivered_date_Key =
orders\[order_estimated_delivery_date\].\[Year\] \* 10000 +
orders\[order_estimated_delivery_date\].\[MonthNo\] \* 100 +
orders\[order_estimated_delivery_date\].\[Day\] // ایجاد ستون کلید تاریخ
برای ارتباط چون در فیلدهای تاریخ ساعت هم داریم که با هم یکسان نیست.

ساخت ستون Is Delivere: = Table.AddColumn( \#"Changed Type",
"is_delivered", each if \[order_status\] = "delivered" then 1 else 0 )
/// ایجاد ستون برای اینکه مشخص بشه کدوم سفارش ها تحویل داده شده اند.

پاکسازی جدول order_items: کنترل نوع داده ها

ایجاد ستون total_item_value: = Table.AddColumn(#"Changed Type",
"total_item_value", each \[price\]+\[freight_value\]) /// جهت محاسبه
Revenue

پاکسازی جدول Products: • دسته‌بندی‌ها پرتغالی هستند پس باید ترجمه بشن با
جدول category_name_translation و ستون product_category_name_english (
Merge Queries) • اگر مقدار null پیدا شد با Other جایگزین می کنیم.

پاکسازی Reviews: • ساخت ستون has_comment: = Table.AddColumn(#"Changed
Type", "has_comment", each if \[review_comment_message\] = null then
false else true) // جهت بررسی داشتن نظر یا عدم نظردهی مشتری

پاکسازی Geolocatio: • یک Zip Code چندین بار تکرار شده: Group By o بر
اساس: geolocation_zip_code_prefix o محاسباتAverage Latitude, Average
Longitude First City, First State, ساخت Payments Summary • یک سفارش ممکن
است: Credit Card ,Voucher, Debit Card را همزمان داشته باشد. یک جدول با
رفرنس ایجاد می کنیم و Group By بر اساس: order_id و محاسبات
SUM(payment_value) ,MAX(payment_installments) را انجام می‌دهیم.

طبقه‌بندی جداول Fact و Dimension:

ارتباط بین جداول: از (۱) به (N) کلید اتصال کاردینالیتی جهت فیلتر وضعیت
orders fact_order_items order_id ۱ به چند Single ✅ فعال customers
orders customer_id ۱ به چند Single ✅ فعال products fact_order_items
product_id ۱ به چند Single ✅ فعال sellers fact_order_items seller_id ۱
به چند Single ✅ فعال date orders (purchase_date) Date ۱ به چند Single
✅ فعال date orders (delivery_date) Date ۱ به چند Single ❌ غیرفعال
orders fact_payments_summary order_id ۱ به چند Single ✅ فعال Orders
fact_reviews order_id ۱ به چند Single ✅ فعال geolocation customers
zip_code_prefix ۱ به چند Single ✅ فعال geolocation sellers
zip_code_prefix ۱ به چند Single ❌ غیرفعال

دیاگرام مدل داده (Star Schema):

فاز 4: متریک‌های پایه محاسبه کل درآمد حاصل از فروش Total Revenue =
SUM(order_items\[total_item_value\])

تعداد سفارش‌های ثبت‌شده Total Orders = DISTINCTCOUNT(orders\[order_id\])

تعداد کالاهای فروخته‌شده Total Units Sold = COUNTROWS(order_items)

میانگین ارزش هر سفارش AOV (Average Order Value) = DIVIDE(\[Total
Revenue\], \[Total Orders\])

میانگین امتیاز مشتریان Avg Review Score =
AVERAGE(reviews\[review_score\])

میانگین مدت زمان تحویل سفارش Avg Delivery Time (Days) = AVERAGEX(
FILTER(orders, orders\[order_status\] = "delivered"),
DATEDIFF(orders\[purchase_timestamp\],
orders\[delivered_customer_date\], DAY) )

اختلاف بین تاریخ وعده داده شده و تاریخ واقعی تحویل Delivery Delay (Days)
= AVERAGEX( FILTER(orders, NOT
ISBLANK(orders\[order_delivered_customer_date\])),
DATEDIFF(orders\[order_estimated_delivery_date\],
orders\[order_delivered_customer_date\], DAY) )

درصد سفارش‌هایی که دیر تحویل شده‌اند. Late Delivery Rate = DIVIDE(
CALCULATE(COUNTROWS(orders), orders\[order_delivered_customer_date\] \>
orders\[order_estimated_delivery_date\]), CALCULATE(COUNTROWS(orders),
NOT ISBLANK(orders\[order_delivered_customer_date\])) )

تحلیل RFM (Recency, Frequency, Monetary) \_RFM_Base = SUMMARIZE(
customers, customers\[customer_unique_id\], "Last Purchase Date",
CALCULATE(MAX(orders\[order_purchase_timestamp\])), "Frequency",
CALCULATE(DISTINCTCOUNT(orders\[order_id\])), "Monetary",
CALCULATE(SUM(order_items\[total_item_value\])) ) // محاسبه RFM برای هر
مشتری

Revenue YTD = TOTALYTD(\[Total Revenue\], dim_date\[Date\])

Revenue PY = CALCULATE(\[Total Revenue\],
SAMEPERIODLASTYEAR(date\[Date\]))

Revenue YoY % = DIVIDE(\[Total Revenue\] - \[Revenue PY\], \[Revenue
PY\])

Revenue MTD = TOTALMTD(\[Total Revenue\], date\[Date\])

Rolling 3-Month Avg Revenue = AVERAGEX( DATESINPERIOD(date\[Date\],
LASTDATE(date\[Date\]), -3, MONTH), \[Total Revenue\] )

ایجاد جدول RFM در سطح مشتری: \_RFM_Base = SUMMARIZE( dim_customers,
dim_customers\[customer_unique_id\], "Last Purchase Date",
CALCULATE(MAX(dim_orders\[order_purchase_timestamp\])), "Frequency",
CALCULATE(DISTINCTCOUNT(dim_orders\[order_id\])), "Monetary",
CALCULATE(SUM(fact_order_items\[total_item_value\])) ) تعیین سگمنت هر
مشتری: RFM Segment = VAR R = \[R_Score\] VAR F = \[F_Score\] RETURN
SWITCH( TRUE(), R \>= 4 && F \>= 4, "Champions", R \>= 3 && F \>= 3,
"Loyal Customers", R \>= 4 && F \<= 2, "New Customers", R \<= 2 && F \>=
4, "At Risk", R \<= 2 && F \<= 2, "Lost", "Need Attention" ) تحلیل
Cohort (Cohort Retention Analysis) تعیین ماه Cohort هر مشتری (Calculated
Column در dim_customers): Cohort Month = CALCULATE(
MIN(dim_orders\[order_purchase_timestamp\]), ALLEXCEPT(dim_customers,
dim_customers\[customer_unique_id\]) ) شاخص Cohort (چند ماه از اولین
خرید گذشته): Cohort Index = DATEDIFF( RELATED(dim_customers\[Cohort
Month\]), dim_orders\[order_purchase_timestamp\], MONTH ) + 1 Measure
تعداد مشتریان فعال در هر Cohort: dax Active Customers in Cohort =
DISTINCTCOUNT(dim_orders\[customer_id\])

تخمین ارزش طول عمر مشتری (Customer Lifetime Value) CLV (Historical) =
CALCULATE( SUM(fact_order_items\[total_item_value\]),
ALLEXCEPT(dim_customers, dim_customers\[customer_unique_id\]) )

CLV (Estimated) = VAR AvgOrderValue = \[AOV\] VAR AvgPurchaseFrequency =
DIVIDE(\[Total Orders\],
DISTINCTCOUNT(dim_customers\[customer_unique_id\])) VAR
EstimatedLifespanYears = 2 RETURN AvgOrderValue \* AvgPurchaseFrequency
\* EstimatedLifespanYears
