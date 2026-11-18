# اتصال رزومه به `cv.meetycoach.ir`

> وضعیت بررسی‌شده: ۱۸ نوامبر ۲۰۲۶ · دامنهٔ اصلی: `meetycoach.ir` · هاست: [Runflare](https://portal.runflare.com) · DNS: ابر آروان

---

## خلاصهٔ یکخطی

خطای اسکرین‌شات به خاطر این است که `cv.ir.meetycoach.ir` قبلاً روی ریپوی **دیگری**
(`mhdyhn/mhdy`) تنظیم شده؛ و چون دامنهٔ اصلیم `meetycoach.ir` است، سابدامینی که انتخاب کردیم
**`cv.meetycoach.ir`** است — این اسم آزاد است و آن خطا دیگر پیش نمیآید.
فقط باید (۱) سمت گیت‌هاب دامنه را ثبت کنی، (۲) در **DNS آروان** یک رکورد `CNAME` بزنی.

---

## ۱. چرا اون خطا اومد؟

> `The custom domain cv.ir.meetycoach.ir is already taken by another repository in your account.`

گیت‌هاب Pages اجازه نمیدهد یک دامنه همزمان روی دو ریپو باشد. با API گیت‌هاب چک کردم:

| ریپو | منبع انتشار | Custom domain | آدرس فعلی |
| --- | --- | --- | --- |
| `mhdyhn/mhdy` | `main` / `/` | `cv.ir.meetycoach.ir` (فایل `CNAME` هم در `main` دارد) | `http://cv.ir.meetycoach.ir/` |
| `mhdyhn/resume` | `main` / `/` | خالی | `https://mhdyhn.github.io/resume/` ✅ سالم |

ضمناً `cv.ir.meetycoach.ir` **اصلاً در DNS وجود ندارد** (NXDOMAIN)، یعنی همان سایت قدیمی هم
عملاً بالا نمیآمده. `cv.meetycoach.ir` و `cv.ir.meetycoach.ir` هم هر دو خالی/آزاداند.

**نتیجه:** اسم درست و ساده برای رزومه `cv.meetycoach.ir` است (یک سطح زیر `meetycoach.ir`)،
نه `cv.ir.meetycoach.ir`. با همین اسم، تداخل ریپوی `mhdy` هم خودبهخود بیاثر میشود.

---

## ۲. نکتهٔ مهم: «هاست» کجاست و «سابدامین» کجا ساخته میشود؟

سه چیز را جدا ببین:

| چی | کجاست | چیکار میکند |
| --- | --- | --- |
| اپ اصلی «میتی» (`meetycoach.ir`) | Runflare | صفحهٔ لاگین اپ، الان کار میکند ✅ |
| فایل‌های رزومه | GitHub Pages (الان) / یا یک Static app در Runflare | `index.html` + `assets/` + `fonts/` |
| **DNS دامنه** | **ابر آروان** | `NS`: `c.ns.arvancdn.ir` , `s.ns.arvancdn.ir` |

چون `NS` دامنه پیش آروان است، **رکوردهای DNS را باید در پنل آروان بسازی** (نه در cPanel،
و نه لزوماً در پنل رانفلر). رکورد `A` دامنهٔ اصلی الان `195.214.235.47` برمیگردد؛ این آدرس
مربوط به اپ فعلی است و نباید برای رزومه استفاده شود.

> منوی **«تنظیم دامنه ← مدیریت زیر دامنهها»** در Runflare فقط وقتی لازم است که بخواهی رزومه را
> **بهعنوان یک سرویس/اپ جدید داخل خود Runflare** میزبانی کنی (مسیر B). اگر رزومه همانجا روی
> GitHub Pages بماند، نیازی به ساختن سابدامین در پنل Runflare نیست؛ یک رکورد CNAME در آروان کافی است.

> ⚠️ تلاش کردم تنظیمات Pages را با API گیت‌هاب خودکار تغییر دهم، ولی توکن این محیط فقط دسترسی
> خواندن به Pages دارد (`403 Resource not accessible by integration`). بهجای آن یک `PR` آماده
> کردم که فایل `CNAME` را اضافه میکند؛ مرج کردن آن (یا یک کلیک در Settings → Pages) سمت گیت‌هاب را کامل میکند.

---

## مسیر A — پیشنهادی: رزومه روی GitHub Pages + رکورد CNAME در آروان

سادهترین و ارزانترین راه؛ سایت الان روی `https://mhdyhn.github.io/resume/` بالاست.

### A-1) سمت گیت‌هاب (یکی از دو روش، نه هر دو)

**روش ۱ (قطعی، پیشنهادی):**
`https://github.com/mhdyhn/resume/settings/pages` → **Custom domain** → بنویس
`cv.meetycoach.ir` → **Save**. گیت‌هاب خودش فایل `CNAME` را به `main` کامیت میکند.

**روش ۲ (همین PR):** فایل `CNAME` موجود در این برنچ را با مرج کردن PR وارد `main` کن.
محتوای فایل فقط یک خط است:

```
cv.meetycoach.ir
```

بعد از هر کدام، در همان صفحه چک کن که جلوی Custom domain نوشته شده `cv.meetycoach.ir`.

> 🔧 پاکسازی پیشنهادی (اختیاری ولی خوب): در `https://github.com/mhdyhn/mhdy/settings/pages`
> روی **Remove** بزن تا `cv.ir.meetycoach.ir` قدیمی و بیاستفاده هم آزاد شود و بعداً دوباره گیجت نکند.
> سایت قدیمی همچنان از `https://mhdyhn.github.io/mhdy/` در دسترس میماند.

### A-2) سمت DNS (پنل ابر آروان)

پنل آروان ← **شبکه توزیع محتوا** ← دامنهٔ `meetycoach.ir` ← **رکوردهای DNS** ← **افزودن رکورد**:

| فیلد | مقدار |
| --- | --- |
| نوع / Type | `CNAME` |
| نام / عنوان | `cv`  ⇐ یعنی `cv.meetycoach.ir` |
| مقدار / مقصد | `mhdyhn.github.io` |
| TTL | `120` یا پیشفرض |
| **نماد ابر / Cloud (CDN)** | **خاموش** (DNS-only) |

قواعدی که حتماً رعایت کن:
- مقصد `mhdyhn.github.io` است؛ **نه** `mhdyhn.github.io/resume` و **نه** IP هاست/آروان.
- نماد ابر را خاموش بگذار. اگر CDN آروان جلوی رکورد باشد، `Host header` عوض میشود و هم Pages
  404 میدهد هم صدور گواهی HTTPS گیر میکند. [مستندات نماد ابر آروان](https://docs.arvancloud.ir/fa/cdn/dns-records/cloud)
- رکورد wildcard (`*.meetycoach.ir`) نساز؛ خطر takeover دامنه دارد.
- اگر رکورد `A` یا `CNAME` دیگری برای `cv` وجود دارد، اول حذفش کن (A بر CNAME اولویت دارد).
- [مستندات افزودن رکورد CNAME در آروان](https://docs.arvancloud.ir/en/cdn/dns-records/adding-records)

### A-3) HTTPS و تأیید مالکیت

1. بعد از اینکه DNS درست شد (چند دقیقه تا ۲۴ ساعت؛ دامنهٔ `.ir` گاهی تا ۴ ساعت)،
   در Pages تیک **Enforce HTTPS** را بزن. صدور گواهی Let's Encrypt تا ~۱ ساعت طول میکشد.
2. **توصیه امنیتی:** Profile (عکس پروفایل) → Settings → **Pages** → **Add a domain** →
   `meetycoach.ir` را verify کن. گیت‌هاب یک رکورد `TXT` به شکل
   `_github-pages-challenge-mhdyhn.meetycoach.ir` میدهد؛ همان را در آروان اضافه کن و نگه دار.
   تأیید دامنهٔ اصلی، `cv.meetycoach.ir` را هم پوشش میدهد و جلوی تصاحب سابدامین را میگیرد.
   [مستندات Verify](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/verifying-your-custom-domain-for-github-pages)

---

## مسیر B — جایگزین: رزومه واقعاً روی هاست Runflare باشد

اگر میخواهی رزومه هم مثل اپ اصلی داخل Runflare میزبانی شود (و GitHub Pages را کنار بگذاری):

1. **بسته آماده:** `resume-static.zip` (کنار همین فایل در ریشهٔ ریپو) را ساختهام؛ شامل `index.html`، `assets/`، `fonts/`
   (بدون پوشهٔ اضافی، فایلها در ریشهٔ zip هستند). مناسب آپلود Drag & Drop.
2. در Runflare: **ایجاد پروژه** (یا همان پروژهٔ فعلی) ← **سرویسها → ایجاد → Static** ←
   نسخهٔ nginx → نام مثلاً `resume` → ایجاد.
   [ایجاد اپ Static](https://docs.runflare.com/static/static-create-app/)
3. همان سرویس ← استقرار با **Drag & Drop** ← آپلود `resume-static.zip`.
   [مستندات Drag & Drop](https://docs.runflare.com/static/static-deploy-dragdrop/)
   (یا از تب **Github** مستقیم این ریپو را وصل کن تا با هر push دیپلوی شود.)
4. **سابدامین در Runflare:** مدیریت پروژه ← **تنظیم دامنه** ← `meetycoach.ir` ← تنظیمات دامنه ←
   **مدیریت زیر دامنهها** ← بنویس `cv` (بدون `http` و بدون `www`) ← افزودن ← سابدامین را به
   سرویس `resume` وصل کن. [مستندات مدیریت دامنه](https://docs.runflare.com/general/domain-managment/)
5. **DNS:** چون `NS` پیش آروان است، در پنل آروان یک رکورد **`A`** بساز:

   | فیلد | مقدار |
   | --- | --- |
   | نوع | `A` |
   | نام | `cv` |
   | مقدار | **IP لودبالانسر سرویس `resume`** در پنل Runflare (بخش سرویس / Public IP). همان `195.214.235.47` دامنهٔ اصلی لزوماً IP این سرویس نیست. |

   همچنین رکورد **TXT**ای که Runflare در «تنظیمات دامنه» برای احراز مالکیت میدهد اضافه کن،
   بعد **بررسی مجدد** بزن تا دامنه Active شود. اگر روزی `NS`ها را به «DNS مستقیم رانفلر» تغییر
   دادی، همان رکوردها را در بخش **«تنظیم رکورد»** خود پنل Runflare بساز.
   [اتصال سابدامین از سرویسدهندهٔ دیگر](https://docs.runflare.com/general/domain-managment/#add_domain_from_other_providor)
6. **SSL:** سرویس `resume` ← تنظیم SSL ← انتخاب `cv.meetycoach.ir` ← «گواهی رایگان رانفلر» ← ذخیره.
   [مستندات SSL](https://docs.runflare.com/general/ssl/)
7. در این مسیر، دامنهٔ سفارشی را از `mhdyhn/resume` → Pages پاک کن (Settings → Pages → Remove)
   تا دو جا همزمان ادعای `cv.meetycoach.ir` نکنند.

> فرق کلیدی: **CNAME → `mhdyhn.github.io`** = مسیر A (GitHub Pages) ·
> **A → IP لودبالانسر Runflare** = مسیر B (هاست خودت). هر دو را همزمان نساز.

---

## ۴. تست و عیبیابی

```bash
dig cv.meetycoach.ir +nostats +nocomments +nocmd
```
خروجی مورد انتظار در مسیر A:
```
cv.meetycoach.ir.     CNAME   mhdyhn.github.io.
mhdyhn.github.io.     A       185.199.108.153   (و ۱۰۹/۱۱۰/۱۱۱)
```
بدون `dig` هم میشود چک کرد:
`https://dns.google/resolve?name=cv.meetycoach.ir&type=CNAME`

| علامت | یعنی | چاره |
| --- | --- | --- |
| `NXDOMAIN` / Status 3 | هنوز رکوردی در آروان نساختهای | A-2 را انجام بده |
| رکورد `A` به `195.214.235.47` برگشت | نماد ابر روشن است یا A قدیمی مانده | ابر را خاموش کن / A را حذف کن |
| 404 گیت‌هاب | CNAME اشتباه است (مثلاً شامل `/resume`) یا Pages دامنه را نگرفته | A-1 + مقصد `mhdyhn.github.io` |
| خطای گواهی / Enforce HTTPS خاکستری | DNS تازه یا CDN جلوی ترافیک است | صبر تا ۲۴h + خاموش کردن نماد ابر |

مستندات گیت‌هاب:
[Managing a custom domain](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site#configuring-a-subdomain) ·
[Troubleshooting / CNAME errors](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/troubleshooting-custom-domains-and-github-pages#cname-errors)

---

## ۵. چکلیست نهایی (مسیر A)

- [ ] PR این برنچ مرج شد **یا** در Settings → Pages دامنه `cv.meetycoach.ir` ذخیره شد
- [ ] (اختیاری) `cv.ir.meetycoach.ir` از ریپوی `mhdy` حذف شد
- [ ] در آروان: `CNAME cv → mhdyhn.github.io`، نماد ابر **خاموش**
- [ ] `dig` درست برگشت و `https://cv.meetycoach.ir` رزومه را نشان داد
- [ ] Enforce HTTPS روشن شد
- [ ] (توصیه) رکورد TXT تأیید دامنه اضافه شد

سایت فعلی بدون دامنه: https://mhdyhn.github.io/resume/
