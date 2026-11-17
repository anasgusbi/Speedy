[README.md](https://github.com/user-attachments/files/34961968/README.md)# سبيدي — حضور وانصراف المندوبين

تطبيق للمشرف يعمل على الهاتف والكمبيوتر من رابط واحد. البيانات محفوظة على Firebase ومتزامنة لحظياً بين الأجهزة، ويستمر العمل بدون إنترنت ثم ترفع التسجيلات تلقائياً عند عودة الاتصال.

## محتويات المستودع

| الملف | الوظيفة |
|---|---|
| `index.html` | واجهة البرنامج والتصميم |
| `js/app.js` | منطق البرنامج والمزامنة مع Firebase |
| `js/firebase-config.js` | **بيانات مشروع Firebase — يجب تعبئتها** |
| `firestore.rules` | قواعد الحماية (تُنسخ إلى Firebase Console) |
| `manifest.webmanifest` و `sw.js` | تثبيت البرنامج كتطبيق على الهاتف وعمله بدون إنترنت |
| `icons/` | أيقونات التطبيق |

## 1) إعداد Firebase (مرة واحدة)

1. ادخل إلى https://console.firebase.google.com ← **Add project** ← اكتب اسماً (مثلاً `speedy-attendance`) ← يمكنك إيقاف Google Analytics ← Create.
2. **Build ← Authentication ← Get started ← Sign-in method ← Email/Password ← Enable ← Save**.
3. في **Authentication ← Users ← Add user**: أنشئ حساب المشرف (بريد + كلمة مرور)، وحساباً لك إن أردت.
4. **Build ← Firestore Database ← Create database** ← اختر موقعاً أوروبياً (مثل `eur3` أو `europe-west`) ← **Start in production mode** ← Create.
5. في Firestore افتح تبويب **Rules**، الصق محتوى ملف `firestore.rules` ← **Publish**. (بريد صاحب المنصة مكتوب فيه وفي `js/firebase-config.js`؛ بقية المستخدمين يُضافون من داخل المنصة: الإعدادات ← المستخدمون والصلاحيات.)
6. من ⚙️ **Project settings ← General ← Your apps** اضغط أيقونة الويب `</>` ← اكتب اسماً ← Register app. انسخ قيم `firebaseConfig` وضعها في الملف `js/firebase-config.js`.

> الخطة المجانية (Spark) تكفي ولا تحتاج بطاقة دفع.
> بيانات `firebaseConfig` ليست سرية؛ الحماية تتم عبر قواعد Firestore في الخطوة 5.

## 2) الرفع على GitHub Pages

1. على GitHub: **New repository** ← اسم مثل `speedy-attendance` ← **Public** ← Create.
2. **Add file ← Upload files** ← اسحب **محتويات** هذا المجلد كلها (الملفات ومجلدي `js` و `icons`، وليس المجلد نفسه) ← **Commit changes**.
3. **Settings ← Pages ← Source: Deploy from a branch ← Branch: `main` / `(root)` ← Save**.
4. بعد دقيقة أو دقيقتين يصبح الرابط جاهزاً: `https://اسم-المستخدم.github.io/speedy-attendance/`
5. (احتياطاً) في Firebase: **Authentication ← Settings ← Authorized domains ← Add domain** وأضف `اسم-المستخدم.github.io`.

## 3) التثبيت على الهاتف

- **أندرويد (Chrome):** افتح الرابط ← القائمة ⋮ ← **تثبيت التطبيق / Add to Home screen**.
- **آيفون (Safari):** افتح الرابط ← زر المشاركة ← **Add to Home Screen**.
- على الكمبيوتر: افتح الرابط في Chrome، ويمكن تثبيته أيضاً من أيقونة التثبيت في شريط العنوان.

أول تسجيل دخول يحتاج إنترنت، وبعدها يفتح البرنامج ويسجّل حتى بدون اتصال.

## 4) نقل البيانات من البرنامج القديم

في الملف القديم: **الإعدادات والنسخ ← تنزيل نسخة احتياطية**.
في البرنامج الجديد: **الإعدادات ← استيراد نسخة** واختر الملف. ستُرفع كل البيانات للسحابة.

## التحديثات لاحقاً

عدّل الملف ثم ارفعه من جديد على GitHub (Upload files يستبدل الملف القديم). عند كل تعديل على `index.html` أو `js/app.js`، غيّر رقم `CACHE` في `sw.js` (مثلاً `speedy-att-v2`) حتى تصل النسخة الجديدة للهواتف بسرعة.

## هيكل البيانات في Firestore

- `drivers/{id}` — بيانات المندوب.
- `days/{YYYY-MM-DD}` — مستند واحد لكل يوم يحتوي `recs` (تسجيلات الحضور لكل فترة) و `orders` (عدد الطلبيات لكل مندوب). هذا يقلل القراءات ويبقيك داخل الخطة المجانية.
- `config/settings` — مواعيد الفترات ونظام النقاط.

