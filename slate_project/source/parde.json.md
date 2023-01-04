---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: CURL
  - plaintext: RAW
  - python: PYTHON

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the پرده API
---

# پرده

پرده یک ای‌پی‌آی بومی برای تشخیص تصاویر نامناسب است.

در روزگار ما دسترسی به فضای مجازی حتی برای کودکان به‌سادگی میسر است. از طرفی می‌دانیم که در این فضا همه نوع محتوایی هست. محتوای نامناسب آسیب‌زاست خصوصاً برای قشر کودک و نوجوان. دولت‌ها تلاش می‌کنند اینترنت را به محیطی سالم و امن تبدیل کنند اما حجم روزافزون داده‌ها در فضای مجازی امکان نظارت انسانی بر این محیط را بسیار سخت کرده است. راه‌حل، استفاده از هوش ماشینی برای پالایش و فیلتر محتواست.

پرده دقیقاً با همین هدف ساخته شده است.

این ابزار، تصاویر و ویدیوهای نامناسب را در چهار دسته شناسایی و گزارش می‌دهد:

از دید پرده، تصویر و ویدیوی نامناسب در یکی از این چهار دسته قرار می‌گیرد:

۱. **برهنه:** تصاویر و ویدیوهایی که به طرز وقیحانه و حیوان‌واری اعضای جنسی انسان را به نمایش می‌گذارند. حتی رسانه‌های غربی نیز در نمایش این نوع محتوا فیلترهای سفت‌وسختی را برای سنین مختلف درنظر گرفته‌اند. همچنین در بسیاری از شبکه‌های اجتماعی نیز انتشار چنین محتوایی خلاف قوانین است.

۲. **نامناسب:** تعریف اسلام از محتوای «نامناسب» بسیار وسیع‌تر از تعریف سطحی غرب است. در اسلام، نامناسب بودن محتوا صرفاً محدود به برهنگی نیست! شاید بتوان ملاک‌های صداوسیما در انتشار محتوا را نزدیک به همان چیزی دانست که شرع مقدس اسلام اجازه داده است. از دید پرده تصویر و ویدیویی «نامناسب» است که با ملاک‌های صداوسیما در تلویزیون ایران قابل پخش نیست.

۳. **وحشتناک:** فقط محتوای غیراخلاقی نیست که بر روان آدمی اثر منفی می‌گذارد. گاهی دیدن صحنه‌ای دلخراش از یک تصادف رانندگی که حاوی آسیب‌های منزجرکننده‌‌ای مثل سوختگی و لهیدگی است می‌تواند همان تأثیر سوء را در پی داشته باشد.

۴. **خشونت:** گاهی تصویر یا ویدیو حاوی صحنه‌های منزجرکننده نیست ولی این موضوع حاکی از وجود یک خشونت است؛ مثلاً آثار خون روی زمین یا دیوار یا فردی که روی زمین افتاده است. این نوع محتوا می‌تواند نشانی از نامناسب‌بودن محتوا برای کودکان باشد.

پرده می‌تواند با یک سخت‌افزار معمولی **هر ثانیه بیش از ۲۰۰ تصویر** را تحلیل و پالایش کند. این رقم را پژوهشکدهٔ ارتباطات و فناوری اطلاعات ایران، بعد از ارزیابی دقیق عملکرد سامانه روی مجموعه‌ای از تصاویر متعدد اعلام کرده است.

برای دسترسی به این ای‌پی‌آی به یک `TOKEN_KEY` نیاز دارید که می‌توانید از طریق ایمیلِ [parde@roshan-ai.ir](mailto:parde@roshan-ai.ir) درخواست دهید

# برچسب‌گذاری تصاویر


## مثال

> Request

```plaintext
{
    "image_urls": [
        "sample.com/1.jpg"
    ]
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary '{
    "image_urls": [
        "sample.com/1.jpg"
    ]
}' \
      http://api.sobhe.ir/parde/api/tag_images
```

```python
try:
   from urllib2 import Request, urlopen
expect ImportError:
   from urllib.request import urlopen, Request
from encodings import utf_8

values = bytes("""
{
    "image_urls": [
        "sample.com/1.jpg"
    ]
}
"""
,'utf-8')
headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('http://api.sobhe.ir/parde/api/tag_images', data=values, headers=headers)

response_body = urlopen(request).read()
print(utf_8.decode(response_body)
```

> Response 

```json
[
    {
        "image_url":"sample.com/1.jpg",
        "tags": 
        [
            {
                "id": 82311,
                "probability": 0.9,
                "title": "نامناسب"
            },
            {
                "id": 82312,
                "probability": 0.67,
                "title": "خشونت"
            }
        ]
    }
]


```

ورودی این تابع لیستی از آدرس تصاویر است و خروجی آن برچسب‌های هر تصویر. آنچه در پاسخ برگردانده می‌شود آرایه‌ای از آیتم‌هاست. هر آیتم متشکل از فیلد `image_url` حاوی لینک تصویر و آرایهٔ `tags` حاوی برچسب‌های تصویر است. شناسهٔ هر برچسب در فیلد `id`، عنوان برچسب در فیلد `title`و میزان اطمینان به آن برچسب در فیلد `probability`قرار می‌گیرد.

<dl style="background-color:transparent;"><code>POST /parde/api/tag_images</code></dl>

<dl>
<strong>image_urls(required)</strong>
<br>
<br>
Value: [URL, ]
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  لیست آدرس تصاویر.</p>
<br><br>
# برچسب‌گذاری فریم‌های ویدیو

ورودی این تابع لیستی از آدرس ویدیوها است و خروجی آن برچسب‌های هر ویدیو. آنچه در پاسخ برگردانده می‌شود آرایه‌ای از آیتم‌هاست. هر آیتم متشکل از یک فیلد `video_url` حاوی لینک تصویر و آرایهٔ `frames` حاوی فریم‌های پردازش‌شده است. در هر فریم، شمارهٔ آن در فیلد `frame`، موقعیت زمانی آن در فیلد `time` و برچسب‌های شناسایی‌شده برای آن در فیلد `tags` قرار می‌گیرد.


## مثال

> Request

```plaintext
{
    "video_urls": [
        "sample.com/1.mp4"
    ],
    "every_ms": 200,
    "duration": 36000,
    "min_frame_diff": 0.5
}
```

```shell
curl  --request POST \ 
      --header "Content-Type: application/json" --header "Authorization: Token TOKEN_KEY" \
      --data-binary '{
    "video_urls": [
        "sample.com/1.mp4"
    ],
    "every_ms": 200,
    "duration": 36000,
    "min_frame_diff": 0.5
}' \
      http://api.sobhe.ir/parde/api/tag_video_frames
```

```python
try:
   from urllib2 import Request, urlopen
expect ImportError:
   from urllib.request import urlopen, Request
from encodings import utf_8

values = bytes("""
{
    "video_urls": [
        "sample.com/1.mp4"
    ],
    "every_ms": 200,
    "duration": 36000,
    "min_frame_diff": 0.5
}
"""
,'utf-8')
headers = {
  'Content-Type': 'application/json','Authorization': 'Token TOKEN_KEY',
}
request = Request('http://api.sobhe.ir/parde/api/tag_video_frames', data=values, headers=headers)

response_body = urlopen(request).read()
print(utf_8.decode(response_body)
```

> Response 

```json
[
    {
        "video_url": "sample.com/1.mp4",
        "frames": 
        [
            {
                "frame": 6,
                "time": "0:00:00",
                "tags": []
            },
            {
                "frame": 31,
                "time": "0:00:01",
                "tags": []
            },
            {
                "frame": 36,
                "time": "0:00:01",
                "tags": []
            },
            {
                "frame": 475,
                "time": "0:00:18",
                "tags": 
                [
                    {
                        "id": 86141,
                        "probability": 0.32,
                        "title": "وحشتناک"
                    },
                    {
                        "id": 86142,
                        "probability": 0.49,
                        "title": "برهنه"
                    }
                ]
            },
            {
                "frame": 560,
                "time": "0:00:22",
                "tags": []
            }
        ]
    }
]


```

<dl style="background-color:transparent;"><code>POST /parde/api/tag_video_frames</code></dl>

<dl>
<strong>video_urls(required)</strong>
<br>
<br>
Value: [URL, ]
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  آدرس ویدیوهایی که می‌خواهید فریم‌های آن برچسب‌گذاری شود.</p>
<br><br>
<dl>
<strong>every_ms</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    100
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  هر چند میلی‌ثانیه یک فریم استخراج شود؟</p>
<br><br>
<dl>
<strong>min_frame_diff</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    0.4
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  کمترین تفاوتِ دو فریم که از آن به بعد یک فریم را متفاوت از قبلی بداند. عددی بین ۰ تا ۱.</p>
<br><br>
<dl>
<strong>duration</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    25
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  چندثانیه از ابتدای ویدیو پردازش شود؟ اگر `null` باشد کل ویدیو پردازش می‌شود.</p>
<br><br>
<dl>
<strong>wait</strong>
<br>
<br>
Value: <span style="background-color: #00A693;
                    border-color: #00A693;
                    border-width: 3px;
                    border-radius: 2px">
                    true
                    </span>
</dl>

<p style="direction:rtl;font-weight:300;">
<img src="./images/vector.svg" alt="vector">  اگر `true` باشد (مقدار پیش‌فرض)، تا پایان فرایند پردازش باید منتظر بمانید. اگر `false` باشد، بلافاصله بعد از ارسال درخواست، پاسخی با اعلام وضعیتِ `pending` یا `started` ارسال می‌شود و در درخواست‌های بعدی اگر نتیجه آماده بود برگردانده می‌شود و اگر نه همچنان روی `pending` خواهد بود.</p>
<br><br>
