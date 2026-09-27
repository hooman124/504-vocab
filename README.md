# 504 English Vocabulary — اپ فلاتر (آماده انتشار در کافه‌بازار)

این پروژه فایل HTML آموزش ۵۰۴ لغت رو داخل یک اپ فلاتر (با WebView) بسته‌بندی کرده. کل محتوا و منطق اپ همون فایل اصلیه که بدون تغییر در `assets/www/index.html` قرار گرفته و کاملاً آفلاین کار می‌کنه.

مشخصات فعلی اپ:
- نام نمایشی: `504 English Vocabulary`
- Package name / applicationId: `com.vocab504.app`
- آیکون: یه آیکون ساده آبی با نوشته‌ی «۵۰۴» ساخته شده (داخل `android/app/src/main/res/mipmap-*`) — اگه آیکون دلخواه خودتو داری، جایگزینش کن.
- امضا (signing): با یک **کیستور واقعی release** امضا می‌شه (نه debug) — دقیقاً چیزی که کافه‌بازار برای انتشار نیاز داره.

---

## ⚠️ خیلی مهم: فایل کیستور رو گم نکن

توی پوشه‌ی `keystore/` (که عمداً در `.gitignore` هست و **نباید** به گیت‌هاب push بشه) این فایل‌ها رو برات ساختم:

- `keystore/release-keystore.jks` → فایل کیستور واقعی
- `keystore/keystore_creds.txt` → رمزهای کیستور
- `keystore/keystore_base64.txt` → همون کیستور به‌صورت base64 (برای گذاشتن در GitHub Secrets)

**این فایل‌ها رو یه‌جای امن (مثلاً پسورد منیجر یا هارد شخصی) نگه دار.** اگه گمشون کنی، دیگه نمی‌تونی نسخه‌ی جدید اپ رو با همون امضا آپدیت کنی و باید اپ رو به‌عنوان یه اپ کاملاً جدید در کافه‌بازار ثبت کنی.

رمزهای فعلی:
```
storePassword = W1dp9OUqXb186Jz4zruI
keyPassword   = W1dp9OUqXb186Jz4zruI   (در فرمت PKCS12 این دو همیشه یکی هستن)
keyAlias      = vocab504
```

---

## مرحله ۱: ساخت ریپازیتوری و push کردن پروژه

```bash
cd vocab504_flutter
git init
git add .
git commit -m "Flutter wrapper for 504 vocabulary app"
git branch -M main
git remote add origin <آدرس ریپوی خودتون>
git push -u origin main
```

توجه: پوشه‌ی `keystore/` به‌خاطر `.gitignore` push نمی‌شه (و نباید بشه). محتوای اون رو دستی، فقط برای مرحله‌ی بعد لازم داری.

## مرحله ۲: اضافه کردن GitHub Secrets (برای امضای واقعی)

در ریپوی گیت‌هاب برو به: **Settings → Secrets and variables → Actions → New repository secret** و این ۴ مورد رو اضافه کن:

| Secret name | مقدار |
|---|---|
| `KEYSTORE_BASE64` | محتوای فایل `keystore/keystore_base64.txt` (یک رشته‌ی طولانی، همه‌شو کپی کن) |
| `KEYSTORE_PASSWORD` | `W1dp9OUqXb186Jz4zruI` |
| `KEY_ALIAS` | `vocab504` |
| `KEY_PASSWORD` | `W1dp9OUqXb186Jz4zruI` |

بعد از اضافه کردن این ۴ سکرت، هر بار که ورک‌فلو اجرا بشه، APK و AAB با کیستور واقعی امضا می‌شن.

> اگه این سکرت‌ها رو تنظیم نکنی، بیلد بازم خطا نمی‌ده ولی فایل‌ها با کلید debug امضا می‌شن که کافه‌بازار برای انتشار رسمی قبول نمی‌کنه.

## مرحله ۳: اجرای بیلد

- با هر `push` به شاخه‌ی `main`، ورک‌فلوی «Build Flutter APK & AAB» خودکار اجرا می‌شه.
- یا از تب **Actions** روی ورک‌فلو، دکمه‌ی **Run workflow** رو بزن.
- بعد از پایان (حدود ۴-۶ دقیقه)، پایین صفحه‌ی همون Run، بخش **Artifacts** دو فایل داره:
  - `app-release-apk`
  - `app-release-aab`

## مرحله ۴: انتشار در کافه‌بازار

1. وارد پنل توسعه‌دهندگان کافه‌بازار شو: https://pishkhan.cafebazaar.ir (یا developers.cafebazaar.ir بسته به حساب)
2. یک اپ جدید بساز، این اطلاعات رو آماده کن:
   - نام اپ، توضیحات کوتاه/کامل (فارسی)
   - آیکون فروشگاه ۵۱۲×۵۱۲ → فایل آماده‌ست: `store_assets/icon_512.png` (می‌تونی جایگزینش کنی با آیکون بهتر)
   - چند اسکرین‌شات از اپ (با اجرای اپ روی گوشی/شبیه‌ساز بگیر)
   - دسته‌بندی: آموزشی / زبان
3. فایل نصبی رو آپلود کن:
   - اگه پنل کافه‌بازار گزینه‌ی **Android App Bundle (AAB)** رو پشتیبانی می‌کرد، `app-release.aab` رو آپلود کن (حجم کمتر، روش ترجیحی).
   - در غیر این صورت `app-release.apk` رو مستقیم آپلود کن.
4. کافه‌بازار امضای فایل رو بررسی می‌کنه؛ چون با کیستور واقعی امضا شده مشکلی نباید پیش بیاد.
5. برای هر آپدیت بعدی، فقط `versionCode`/`versionName` رو در `android/app/build.gradle` (یا از طریق pubspec.yaml نسخه) بالا ببر و دوباره push کن — تا وقتی همون کیستور رو استفاده می‌کنی، آپدیت‌ها مشکلی نخواهند داشت.

---

## تغییر نام اپ / Package name (اختیاری)

اگه می‌خوای اسم یا Package name رو عوض کنی:
- نام نمایشی: در `android/app/src/main/AndroidManifest.xml`، مقدار `android:label`
- Package name: در `android/app/build.gradle` مقدار `namespace` و `applicationId`، و مسیر پوشه‌ی `android/app/src/main/kotlin/...` و `package` داخل `MainActivity.kt` باید هماهنگ باشن.

## نسخه‌ی اپ (version)

نسخه از `pubspec.yaml` خونده می‌شه:
```yaml
version: 1.0.0+1   # 1.0.0 = versionName, 1 = versionCode
```
برای هر آپدیت جدید در کافه‌بازار، این عدد رو افزایش بده (مثلاً `1.0.1+2`).

---

## ساختار پروژه
```
lib/main.dart                      -> اپ فلاتر که HTML رو در WebView بار می‌کنه
assets/www/index.html              -> فایل ۵۰۴ کلمه (بدون تغییر)
android/                           -> پروژه‌ی native اندروید (کامیت شده، آماده‌ی امضا)
store_assets/icon_512.png          -> آیکون برای صفحه‌ی فروشگاه کافه‌بازار
keystore/                          -> کیستور و رمزها (فقط لوکال، push نمی‌شه)
.github/workflows/build.yml        -> بیلد و امضای خودکار APK/AAB
```

## تست محلی (اختیاری)
اگه خودتون فلاتر نصب دارید:
```bash
flutter pub get
flutter run
```
