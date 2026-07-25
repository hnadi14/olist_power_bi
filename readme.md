**تحلیل عملکرد فروش، لجستیک و رضایت مشتری در پلتفرم  Olist**


**۱. تعریف مسئله بیزینسی**
Situation —   وضعیت موجود
Olist  یک پلتفرم مارکت‌پلیس برزیلی است که فروشندگان کوچک و متوسط را به مشتریان سراسر کشور متصل می‌کند. این پلتفرم بین سال‌های ۲۰۱۶ تا ۲۰۱۸ بیش از ۱۰۰ هزار سفارش را پردازش کرده است.
Complication چالش
مدیریت ارشد داده‌های خام را در قالب فایل‌های پراکنده (سفارش، پرداخت، نظرات، لجستیک) در اختیار دارد، اما دید یکپارچه‌ای از عملکرد فروش، کیفیت تحویل و رضایت مشتری ندارد. تصمیم‌گیری‌ها بدون داشبورد و بر پایه حدس انجام می‌شود.
Business Question  سوال اصلی
کدام عوامل (جغرافیا، زمان تحویل، دسته محصول، روش پرداخت) بیشترین تاثیر را روی رضایت مشتری و درآمد دارند، و چه اقداماتی می‌تواند نرخ رضایت و بازگشت مشتری را افزایش دهد؟
طراحی داشبورد:

**برای مدیر عامل:**
دغدغه اصلی: روند کلی درآمد و رشد
نیاز داشبورد: KPIهای کلان، مقایسه دوره‌ای، خلاصه اجرایی
**برای مدیر لجستیک: **
	دغدغه اصلی: تاخیر تحویل، عملکرد فروشندگان
	نیاز از داشبورد: نقشه جغرافیایی، زمان تحویل، گلوگاه‌ها

**برای CRM:**
	دغدغه اصلی: رفتار و وفاداری مشتری
	داشبورد: تحلیل RFM، Cohort، نرخ بازگشت مشتری

**مدیر محصول: **
	دغدغه اصلی: عملکرد دسته‌بندی‌ها
	داشبورد: فروش و سود به تفکیک دسته محصول

**KPI Dictionary:**

	Total Revenue: مجموع درآمد از آیتم‌های فروخته‌شده
		فرمول:	SUM(price + freight_value)	
هدف: اندازه‌گیری رشد کلی

AOV: میانگین ارزش هر سفارش
	فرمول:  Total Revenue ÷ Total Orders
هدف: سنجش رفتار خرید
	
	Delivery Delay: تاخیر نسبت به تخمین
		فرمول: تاریخ تحویل واقعی – تخمینی
هدف: سنجش کیفیت لجستیک

Review Score Avg: میانگین امتیاز نظرات
	فرمول: AVG(review_score)
هدف: سنجش رضایت مشتری
	
	Repeat Purchase Rate: درصد مشتریان با بیش از یک خرید
		فرمول: مشتریان تکراری ÷ کل مشتریان
هدف: سنجش وفاداری

RFM Segment: سگمنت مشتری بر اساس
فرمول: Recency/Frequency/Monetary	
هدف: هدف‌گذاری بازاریابی

**Solution Architecture:**

● فایل‌های CSV خام Olist (۹ جدول مرتبط)

● پاکسازی و یکپارچه‌سازی داده در PostgreSQL یا Python/Pandas

●  ETL نهایی و تبدیل در Power Query

● مدل‌سازی داده به شکل Star Schema در Power BI

● ساخت لایه محاسباتی با DAX متریک‌های پایه و پیشرفته

● طراحی داشبورد تعاملی چندصفحه‌ای

● انتشار روی Power BI Service

# Data Profiling Report

## Table: `orders`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `order_id` | Text | 0% | 99,441 (Unique) | `e481f51cbdc54678b7cc49136f2d6af` |
| `customer_id` | Text | 0% | 99,441 (Unique) | `9ef432eb6251297304e76186b10a928` |
| `order_status` | Text | 0% | 8 Values | `delivered`, `shipped`, `canceled`, ... |
| `order_purchase_timestamp` | DateTime | 0% | — | `2017-10-02 10:56:33` |
| `order_approved_at` | DateTime | 1% | — | `2017-10-02 11:07:15` |
| `order_delivered_carrier_date` | DateTime | 2% | — | `2017-10-04 19:55:00` |
| `order_delivered_customer_date` | DateTime | 3% | — | `2017-10-10 21:25:13` |
| `order_estimated_delivery_date` | DateTime | 0% | — | `2017-10-18 00:00:00` |

# 📦 Table: `order_items`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `order_id` | Text | 0% | 98,666 | `e481f51cbdc54678b7cc49136f2d6af` |
| `order_item_id` | Integer | 0% | 1–21 | `1` |
| `product_id` | Text | 0% | 32,951 | `4244733e06e7ecb4970a6e2683c13e61` |
| `seller_id` | Text | 0% | 3,095 | `48436dade18ac8b2bce089ec2a041202` |
| `shipping_limit_date` | DateTime | 0% | - | `2017-10-06 11:07:15` |
| `price` | Decimal | 0% | 0.85–6,735 | `58.90` |
| `freight_value` | Decimal | 0% | 0–409.68 | `13.29` |

---

# 👥 Table: `customers`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `customer_id` | Text | 0% | 99,441 (Unique) | `861eff4711a542e4b93843c6dd7febb` |
| `customer_unique_id` | Text | 0% | 96,096 | `8d50f5eadf50201ccdcedfb9e2ac8455` |
| `customer_zip_code_prefix` | Integer | 0% | 14,994 | `14409` |
| `customer_city` | Text | 0% | 4,119 | `Franca` |
| `customer_state` | Text | 0% | 27 | `SP` |

---

# 💳 Table: `order_payments`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `order_id` | Text | 0% | 99,440 | `e481f51cbdc54678b7cc49136f2d6af` |
| `payment_sequential` | Integer | 0% | 1–29 | `1` |
| `payment_type` | Text | 0% | 5 Values | `credit_card`, `boleto`, `voucher`, ... |
| `payment_installments` | Integer | 0% | 0–24 | `2` |
| `payment_value` | Decimal | 0% | 0–13,664 | `99.33` |

# ⭐ Table: `order_reviews`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `review_id` | Text | 0% | ~98,410 | `7bc2406110b926393aa56f80a40eba40` |
| `order_id` | Text | 0% | 98,673 | `e481f51cbdc54678b7cc49136f2d6af` |
| `review_score` | Integer | 0% | 1–5 | `5` |
| `review_comment_title` | Text | ~88% | - | `"Recomendo"` |
| `review_comment_message` | Text | ~59% | - | `"Muito bom o produto."` |
| `review_creation_date` | DateTime | 0% | - | `2018-01-18 00:00:00` |
| `review_answer_timestamp` | DateTime | 0% | - | `2018-01-18 21:46:59` |

---

# 📦 Table: `products`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `product_id` | Text | 0% | 32,951 (Unique) | `1e9e8ef04dbcff4541ed26657ea517e5` |
| `product_category_name` | Text | ~1.85% | 73 | `cool_stuff` |
| `product_name_lenght` | Numeric | ~1.85% | - | `40` |
| `product_description_lenght` | Numeric | ~1.85% | - | `287` |
| `product_photos_qty` | Numeric | ~1.85% | 1–20 | `1` |
| `product_weight_g` | Numeric | ~0.01% | - | `225` |
| `product_length_cm` | Numeric | ~0.01% | - | `16` |
| `product_height_cm` | Numeric | ~0.01% | - | `10` |
| `product_width_cm` | Numeric | ~0.01% | - | `14` |

> **Note:** The original dataset uses the column names `product_name_lenght` and `product_description_lenght` (with the misspelling **lenght**). They are kept unchanged for consistency with the source data.

---

# 🏪 Table: `sellers`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `seller_id` | Text | 0% | 3,095 (Unique) | `3442f8959a84dea7ee197c632cb2df15` |
| `seller_zip_code_prefix` | Integer | 0% | 2,246 | `13023` |
| `seller_city` | Text | 0% | 611 | `Campinas` |
| `seller_state` | Text | 0% | 23 | `SP` |

# 🌍 Table: `geolocation`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `geolocation_zip_code_prefix` | Integer | 0% | 19,015 | `1037` |
| `geolocation_lat` | Decimal | 0% | - | `-23.545621` |
| `geolocation_lng` | Decimal | 0% | - | `-46.639292` |
| `geolocation_city` | Text | 0% | 8,011 | `sao paulo` |
| `geolocation_state` | Text | 0% | 27 | `SP` |

---

# 🔄 Table: `category_name_translation`

| Column Name | Data Type | Null % | Cardinality | Sample Value |
|-------------|-----------|-------:|-------------|--------------|
| `product_category_name` | Text | 0% | 71 (Unique) | `beleza_saude` |
| `product_category_name_english` | Text | 0% | 71 (Unique) | `health_beauty` |

---

# 📊 Data Quality Summary

| Table | Missing Values | Data Quality Notes |
|-------|----------------|--------------------|
| `orders` | Very Low | High-quality transactional data with a few nullable delivery timestamps for canceled/unavailable orders. |
| `order_items` | None | Complete transactional data with no missing values. |
| `customers` | None | Customer identifiers and location fields are fully populated. |
| `order_payments` | None | Payment information is complete. |
| `order_reviews` | High in text fields | Review scores are complete, but optional review titles (~88%) and messages (~59%) are frequently missing. |
| `products` | Low | Approximately 1.85% missing values in category and product attributes; physical dimensions are almost complete. |
| `sellers` | None | Seller information is complete. |
| `geolocation` | None | Geographic reference dataset is complete. |
| `category_name_translation` | None | Translation table is complete, although it contains only 71 translated categories while the products table references 73 categories. |

> **Data Quality Assessment**
>
> - ✅ Most tables contain **0% missing values**, making the dataset suitable for BI and analytical modeling.
> - ⚠️ `order_reviews` contains many missing values in free-text review fields, but these are optional and do not affect quantitative analysis.
> - ⚠️ `products` has a small percentage of missing product metadata (~1.85%), which can be handled through imputation or an "Unknown" category.
> - ⚠️ The translation table contains **71 categories**, whereas the products table includes **73 categories**, meaning two product categories have no English translation and should be identified during ETL.
> - ✅ Primary and foreign keys exhibit high cardinality and are appropriate for building a **Star Schema** in Power BI.