# CertChain — منصة التحقق من الشهادات الأكاديمية

منصة موحدة للجامعات الفلسطينية لإصدار والتحقق من الشهادات الأكاديمية عبر البلوكتشين.

---

## المتطلبات

- PHP >= 8.3
- Composer
- Node.js >= 18
- npm
- MySQL

---

## خطوات التشغيل

### 1. استنساخ المشروع

```bash
git clone https://github.com/abdallah-hasan23/CertChain.git
cd CertChain
```

### 2. تثبيت اعتمادات PHP

```bash
composer install
```

### 3. إعداد ملف البيئة

```bash
cp .env.example .env
php artisan key:generate
```

### 4. إعداد قاعدة البيانات

عدّل القيم التالية في ملف `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=certchain
DB_USERNAME=root
DB_PASSWORD=
```

ثم شغّل الـ migrations:

```bash
php artisan migrate
```

### 5. إضافة بيانات تجريبية (اختياري)

```bash
php artisan db:seed
```

### 6. تثبيت أصول الواجهة

```bash
npm install
npm run dev
```

لإنشاء نسخة الإنتاج:

```bash
npm run build
```

### 7. تشغيل الخادم المحلي

```bash
php artisan serve
```

افتح المتصفح على: [http://localhost:8000](http://localhost:8000)

---

## سكربتات مفيدة

- `composer run setup` — تثبيت الاعتمادات وإعداد البيئة وتشغيل الـ migrations وبناء الأصول.
- `npm run dev` — تشغيل Vite في وضع التطوير.
- `npm run build` — بناء الأصول للبيئة الإنتاجية.

---

## الصفحات الرئيسية في التطبيق

| الصفحة | الرابط | الصلاحية المطلوبة |
|--------|--------|-------------------|
| الرئيسية | `/` | عام |
| تسجيل الدخول | `/login` | زوار فقط |
| إنشاء حساب | `/register` | زوار فقط |
| لوحة التحكم | `/dashboard` | مسجّل الدخول |
| إضافة شهادة | `/dashboard/add-certificate` | مسجّل الدخول |
| قائمة الشهادات | `/dashboard/certificates` | مسجّل الدخول |
| تفاصيل شهادة | `/dashboard/certificates/{id}` | مسجّل الدخول |
| رمز QR | `/dashboard/qr-code` | مسجّل الدخول |
| التحقق من الشهادات | `/dashboard/verify` | مسجّل الدخول |
| المستخدمون | `/dashboard/users` | Admin فقط |
| الإعدادات | `/dashboard/settings` | مسجّل الدخول |
| الملف الشخصي | `/dashboard/profile` | مسجّل الدخول |
| البحث | `/dashboard/search` | مسجّل الدخول |
| الإشعارات | `/dashboard/notifications` | مسجّل الدخول |
| طلبات الأدوار (Admin) | `/dashboard/admin/role-requests` | Admin فقط |
| تسجيل الخروج | `/logout` | مسجّل الدخول |

---

## الأدوار والصلاحيات

| الدور | الوصف |
|-------|-------|
| `admin` | صلاحيات كاملة: إدارة المستخدمين، قبول/رفض طلبات الأدوار، الموافقة على الشهادات |
| `verifier` | التحقق من الشهادات وعرضها |

> يستطيع الـ Admin فقط إنشاء مستخدمين جدد وتعيين دور `admin` أو `verifier` لهم.

---

## نظام طلبات الأدوار

يمكن للمستخدمين طلب ترقية دورهم إلى `verifier`. يستقبل الـ Admin هذه الطلبات في صفحة `/dashboard/admin/role-requests` ويمكنه قبولها أو رفضها. يصل إشعار فوري للـ Admin عبر نظام الإشعارات في الـ dashboard.

---

## نماذج قاعدة البيانات (Models)

| النموذج | الجدول | الوصف |
|---------|--------|-------|
| `User` | `users` | المستخدمون مع الأدوار والحالة |
| `Certificate` | `certificates` | الشهادات الأكاديمية |
| `Student` | `students` | بيانات الطلاب |
| `University` | `universities` | الجامعات |
| `BlockchainRecord` | `blockchain_records` | سجلات البلوكتشين |
| `VerificationLog` | `verification_logs` | سجلات التحقق |
| `RoleRequest` | `role_requests` | طلبات ترقية الأدوار |
| `RoleRequestNotification` | `notifications` | إشعارات النظام |

---

## ملاحظات للمطورين

- الملف `.env` يجب إنشاؤه من `.env.example`.
- المشروع يستخدم `SESSION_DRIVER=database` و`CACHE_STORE=database` افتراضياً.
- تأكد من إنشاء قاعدة البيانات في MySQL قبل تشغيل `php artisan migrate`.
- جميع روابط الـ dashboard تبدأ بالبادئة `/dashboard/` وتستوجب تسجيل الدخول.
- نظام الإشعارات يعمل عبر AJAX ويتحدث تلقائياً عند فتح قائمة الإشعارات.
