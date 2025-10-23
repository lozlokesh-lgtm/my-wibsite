README - نشر المشروع على Firebase Hosting (خطوات)
1) قم بتثبيت Firebase CLI محليًا (تحتاج Node.js):
   npm install -g firebase-tools

2) سجل دخولك إلى حساب Google:
   firebase login

3) أنشئ مشروع Firebase من console.firebase.google.com
   - فعّل Firestore (Mode: Native) ضمن Database
   - من Settings > Project settings > احصل على إعدادات التكوين (apiKey, projectId, ...)

4) جهّز مجلد المشروع محليًا:
   - ضع المحتوى في مجلد public/ (index.html داخل public/)

5) تهيئة firebase في المجلد:
   firebase init
   - اختر Hosting و Firestore (إذا أردت)
   - اختر المشروع الذي أنشأته
   - عند السؤال عن public directory اكتب: public
   - اختر عدم تحويل app إلى single-page إذا رغبت (لكن نستخدم rewrite في firebase.json)

6) انسخ قيم firebaseConfig من إعدادات المشروع واستبدلها في أعلى ملف index.html.

7) تأكد من قواعد Firestore (Security Rules):
   // قواعد بسيطة للسماح بالكتابة للجميع وقراءة للجميع
   // **ملاحظة أمان:** هذه القواعد ضعيفة؛ للأمان الحقيقي استخدم Authentication & roles.
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /{document=**} {
         allow read, write: if true;
       }
     }
   }

   أو لتقييد القراءة/الكتابة حسب متطلباتك.

8) نشر الموقع:
   firebase deploy --only hosting

9) بعد النشر ستحصل على رابط مثل: https://<PROJECT_ID>.web.app
   - يمكنك استخدام Firebase Dynamic Links أو خدمة اختصار روابط مثل bit.ly للحصول على رابط قصير.
   - أو ربط دومين مجاني (freenom) إلى Firebase Hosting (يتطلب إعداد DNS).

ملاحظات أمان مهمة:
- لا تحفظ كلمات سر حقيقية كنص عادي في الإنتاج.
- لحماية السجلات، أنشئ Authentication واستخدم Cloud Functions للتحقق من PIN على الخادم بدلاً من العميل.
- قواعد Firestore أعلاه تسمح بالوصول المفتوح؛ عدّلها قبل الإنتاج.
