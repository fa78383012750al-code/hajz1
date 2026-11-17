# حجز

هذا هو المستودع الموحد لنظام **حجز**. يبقى `index.html` وملفات الويب الموجودة في جذر المستودع مصدر الواجهة الوحيد، بينما يستخدم مشروع Android داخل `android/` Capacitor لتضمين نسخة الويب داخل APK بدل الاعتماد على رابط خارجي فقط. يعمل التطبيق بواجهة عربية من اليمين إلى اليسار، ويدعم التخزين المحلي والمزامنة الاختيارية مع GitHub عند إدخال الرمز من داخل الإعدادات. لا يحتوي المستودع أو التطبيق على رمز GitHub مضمّن.

## نسخة الويب وGitHub Pages

تشغّل نسخة الويب مباشرة بفتح `index.html`، أو عبر خادم محلي:

```bash
npm install
npm run web:serve
```

ينشر الفرع `main` نسخة الويب تلقائيًا عبر [GitHub Pages](https://fa78383012750al-code.github.io/hajz1/). رابط المستودع هو [fa78383012750al-code/hajz1](https://github.com/fa78383012750al-code/hajz1). لتحديث رقم الإصدار، عدّل `version.json` ثم ارفع التعديل إلى الفرع `main`.

## بناء APK لنظام Android

يتطلب البناء Node.js وJava JDK وAndroid SDK متوافقًا مع Capacitor. اسم التطبيق الظاهر هو **حجز**، واسم الحزمة هو `com.fa78383012750alcode.hajz`، ورقم الإصدار الحالي `1.0`، ورقم البناء `1`.

من جذر المستودع، انسخ ملفات الويب الحالية إلى مجلد Capacitor ثم نفّذ المزامنة والبناء:

```bash
npm install
npm run android:copy
cd android
./gradlew assembleDebug
```

ينتج ملف APK في:

```text
android/app/build/outputs/apk/debug/app-debug.apk
```

يحافظ أمر `npm run android:copy` على مصدر الويب في الجذر، ويُنشئ نسخة البناء داخل `www/` ثم ينسخها إلى Android. لذلك، بعد تعديل `index.html` أو `manifest.json` أو `sw.js` أو مجلد `icons/`، يكفي تنفيذ الأوامر السابقة لإعادة بناء APK من التعديل الجديد.

## التحديث المستقبلي من GitHub

```bash
git clone https://github.com/fa78383012750al-code/hajz1.git
cd hajz1
npm install
# عدّل index.html أو ملفات الواجهة
npm run android:copy
cd android
./gradlew assembleDebug
```

لا تُرفع ملفات `build/` أو `local.properties` أو أي رمز وصول. يمكن تثبيت الملف الناتج مباشرة على جهاز Android باستخدام `adb install -r` عند توفر جهاز أو محاكي مصرح به.

## ملاحظات التشغيل والأمان

تُخزّن بيانات الواجهة محليًا عبر `localStorage`. ويُدخل رمز GitHub من إعدادات التطبيق ويُحفظ محليًا فقط عند اختيار المستخدم؛ لا ينبغي وضعه في الكود أو في GitHub أو في رسائل الإصدار. يحتاج الوصول إلى GitHub API والروابط الخارجية إلى اتصال HTTPS.
