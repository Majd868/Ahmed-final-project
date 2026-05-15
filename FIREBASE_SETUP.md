# 🔥 دليل ربط StorePilot بـ Firebase

## الخطوة 1: إنشاء مشروع Firebase

1. افتح [console.firebase.google.com](https://console.firebase.google.com)
2. اضغط **"Add project"**
3. اسم المشروع: `StorePilot`
4. فعّل Google Analytics (اختياري)
5. اضغط **"Create project"**

---

## الخطوة 2: إضافة تطبيق Android

1. في Firebase Console، اضغط أيقونة **Android** (</> Android)
2. أدخل:
   - **Package name:** `com.storepilot`
   - **App nickname:** StorePilot
3. اضغط **"Register app"**
4. حمّل ملف **`google-services.json`**
5. ضع الملف في: `app/google-services.json` ← **مهم جداً!**

---

## الخطوة 3: تفعيل Firebase Authentication

1. Firebase Console → **Authentication** → **Get started**
2. Sign-in method → **Email/Password** → فعّل (Enable)
3. اضغط **Save**

> 💡 التطبيق يحول اسم المستخدم إلى بريد إلكتروني داخلي:
> `ahmed` → `ahmed@storepilot.app`

---

## الخطوة 4: إنشاء Firestore Database

1. Firebase Console → **Firestore Database** → **Create database**
2. اختر **"Start in production mode"**
3. اختر موقع الخادم (مثلاً: `europe-west1` للشرق الأوسط)
4. اضغط **"Done"**

### تطبيق قواعد الأمان:
1. Firestore → **Rules**
2. انسخ محتوى `firebase/firestore.rules` والصقه
3. اضغط **Publish**

---

## الخطوة 5: إنشاء Realtime Database

1. Firebase Console → **Realtime Database** → **Create Database**
2. اختر **"Start in locked mode"**
3. اختر موقع قريب

### تطبيق القواعد:
1. Realtime Database → **Rules**
2. انسخ محتوى `firebase/database.rules.json` والصقه
3. اضغط **Publish**

---

## الخطوة 6: تفعيل Firebase Storage (لصور المنتجات)

1. Firebase Console → **Storage** → **Get started**
2. اختر **"Start in production mode"**
3. اختر الموقع (نفس Firestore)

---

## الخطوة 7: بناء المشروع

```bash
# تأكد من وجود google-services.json في app/
ls app/google-services.json

# بناء المشروع
./gradlew assembleDebug
```

---

## هيكل Firestore Collections

```
firestore/
├── users/
│   └── {uid}/          ← Firebase Auth UID
│       ├── username
│       ├── email
│       ├── role        ← OWNER | STORE_MANAGER | SHIFT_MANAGER | EMPLOYEE
│       └── createdAt
│
├── products/
│   └── {productId}/
│       ├── name, category, size, color
│       ├── quantity, price, costPrice
│       └── imageUrl    ← Firebase Storage URL
│
├── sales/
│   └── {saleId}/
│       ├── productId, productName
│       ├── quantity, totalPrice
│       ├── soldBy, soldByName
│       └── saleDate
│
├── purchases/
│   └── {purchaseId}/
│
├── tasks/
│   └── {taskId}/
│
├── seasons/
│   └── {seasonId}/
│
└── video_metrics/
    └── {metricId}/
```

## هيكل Realtime Database

```
realtime-db/
├── dashboard/
│   └── daily_sales/
│       └── {YYYY-M-D}: 89.97   ← إجمالي مبيعات اليوم (يتحدث فورياً)
│
├── store_stats/
│   ├── productCount: 5
│   ├── totalUsers: 3
│   └── lastUpdated: timestamp
│
└── online_users/
    └── {uid}: true             ← المستخدمون المتصلون الآن
```

---

## المقارنة: قبل وبعد Firebase

| الميزة | قبل (Room) | بعد (Firebase) |
|--------|-----------|----------------|
| قاعدة البيانات | محلية على الجهاز | سحابية مشتركة |
| المزامنة | لا يوجد | فورية بين الأجهزة |
| المصادقة | PBKDF2 يدوي | Firebase Auth آمن |
| الجلسة | تُفقد عند الإغلاق | تُحفظ تلقائياً |
| الصور | لا دعم | Firebase Storage |
| Dashboard | استعلامات بطيئة | Realtime DB فوري |
| Offline | لا يوجد | Firestore Cache |

---

## بيانات الدخول التجريبية (Demo Mode)

عند تفعيل Demo Mode في الإعداد:
- **Owner:** يُنشأ باسم المستخدم وكلمة المرور التي أدخلتها
- **بيانات تجريبية:** منتجات، مبيعات، مهام، مواسم تُضاف تلقائياً

لإضافة موظفين: استخدم `AuthViewModel.createEmployee(username, password, role)`

---

## ملاحظات مهمة

> ⚠️ **لا تنسَ** وضع `google-services.json` في مجلد `app/`
> هذا الملف سري ولا يُرفع إلى GitHub (مضاف في .gitignore)

> 🔒 **الأمان:** Firebase Auth يدير كلمات المرور بشكل آمن تلقائياً،
> لا حاجة لـ PBKDF2 أو Salt يدوي.

> 📱 **Offline:** Firestore يُخزن نسخة محلية تلقائياً، التطبيق يعمل
> بدون إنترنت ويُزامن عند الاتصال.
