# ساختار پایگاه داده (Database Schema)

## 📋 فهرست مطالب
1. [نمای کلی](#نمای-کلی)
2. [جدول Users](#جدول-users)
3. [جدول Logs](#جدول-logs)
4. [جدول AppSettings](#جدول-appsettings)
5. [روابط بین جداول](#روابط-بین-جداول)
6. [کوئری‌های مفید](#کوئری‌های-مفید)

---

## نمای کلی

پایگاه داده شامل **3 جدول اصلی** است:

```
┌─────────────────┐
│     Users       │
├─────────────────┤
│ UserID (PK)     │
│ Username        │
│ Password        │
│ FullName        │
│ Email           │
│ Role            │
│ IsActive        │
└─────────────────┘
         ↓
    (Referenced)
         ↓
┌─────────────────┐
│      Logs       │
├─────────────────┤
│ LogID (PK)      │
│ UserID (FK)     │
│ Username        │
│ ActionType      │
│ Description     │
│ LogDateTime     │
└─────────────────┘

┌──────────────────┐
│  AppSettings     │
├──────────────────┤
│ SettingID (PK)   │
│ SettingName      │
│ SettingValue     │
│ IsEncrypted      │
│ ModifiedBy       │
│ ModifiedDate     │
└──────────────────┘
```

---

## جدول Users

### توضیح
جدول **Users** تمام اطلاعات کاربران سیستم را ذخیره می‌کند.

### ستون‌ها

#### 1. UserID
```
نوع:              AutoNumber
کلید اصلی:       بله (Primary Key)
خالی‌پذیر:       خیر
شرح:             شناسه منحصر برای هر کاربر
مثال:            1, 2, 3, ...
```

#### 2. Username
```
نوع:              Text (100)
یکتا:             بله (Unique)
خالی‌پذیر:       خیر
شرح:             نام‌کاربری برای ورود
مثال:            admin, user1, manager_ali
قیود:            بدون فاصله، کوچک‌نویسی
```

#### 3. Password
```
نوع:              Text (255)
خالی‌پذیر:       خیر
شرح:             رمز عبور رمزگذاری شده
مثال:            123|126|126|144|153|...
نکته:            هرگز به صورت متن ساده ذخیره نشود
```

#### 4. FullName
```
نوع:              Text (255)
خالی‌پذیر:       خیر
شرح:             نام کامل کاربر
مثال:            علی احمدی، فاطمه محمدی
```

#### 5. Email
```
نوع:              Text (255)
خالی‌پذیر:       خیر
شرح:             آدرس ایمیل کاربر
مثال:            ali@example.com, fateme@company.ir
```

#### 6. Role
```
نوع:              Text (50)
خالی‌پذیر:       خیر
شرح:             نقش کاربر
مقادیر ممکن:    "Admin" یا "User"
پیش‌فرض:         "User"
مثال:            Admin, User
```

#### 7. IsActive
```
نوع:              Yes/No (Boolean)
خالی‌پذیر:       خیر
شرح:             وضعیت فعال/غیرفعال کاربر
مقادیر ممکن:    True (فعال) یا False (غیرفعال)
پیش‌فرض:         True
نکته:            کاربر غیرفعال نمی‌تواند ورود کند
```

#### 8. CreatedDate
```
نوع:              DateTime
خالی‌پذیر:       خیر
شرح:             تاریخ و ساعت ایجاد کاربر
مثال:            2026-06-06 14:30:45
```

#### 9. CreatedBy
```
نوع:              Text (255)
خالی‌پذیر:       خیر
شرح:             کاربری که این کاربر را ایجاد کرده
مثال:            admin, System
```

#### 10. LastModifiedDate
```
نوع:              DateTime
خالی‌پذیر:       بلی
شرح:             آخرین تاریخ ویرایش کاربر
مثال:            2026-06-06 15:45:30
نکته:            هنگام ویرایش اطلاعات بروز می‌شود
```

#### 11. LastModifiedBy
```
نوع:              Text (255)
خالی‌پذیر:       بلی
شرح:             کاربری که آخرین ویرایش را انجام داد
مثال:            admin, ali_user
```

#### 12. LastLoginDate
```
نوع:              DateTime
خالی‌پذیر:       بلی
شرح:             تاریخ و ساعت آخرین ورود
مثال:            2026-06-06 10:00:00
نکته:            در هنگام ورود موفق بروز می‌شود
```

#### 13. LoginAttempts
```
نوع:              Integer
خالی‌پذیر:       خیر
شرح:             تعداد تلاش‌های ورود ناموفق متوالی
مثال:            0, 1, 2, 3
پیش‌فرض:         0
نکته:            بعد از ورود موفق صفر می‌شود
```

### مثال داده

```
UserID  | Username    | Password              | FullName      | Email             | Role  | IsActive | ...
--------|-------------|----------------------|---------------|-------------------|-------|----------|
1       | admin       | 123|126|126|144|...  | Administrator | admin@company.ir  | Admin | True     | ...
2       | ali_user    | 145|132|156|123|...  | علی احمدی      | ali@company.ir    | User  | True     | ...
3       | fateme      | 142|155|143|121|...  | فاطمه محمدی    | fateme@company.ir | User  | False    | ...
```

---

## جدول Logs

### توضیح
جدول **Logs** تمام فعالیت‌های سیستم را ثبت می‌کند برای **Audit Trail** و **تتبع مسائل**.

### ستون‌ها

#### 1. LogID
```
نوع:              AutoNumber
کلید اصلی:       بله (Primary Key)
خالی‌پذیر:       خیر
شرح:             شناسه منحصر برای هر لاگ
مثال:            1, 2, 3, ...
```

#### 2. UserID
```
نوع:              Long
کلید خارجی:      بله (References Users)
خالی‌پذیر:       خیر
شرح:             شناسه کاربری که عمل را انجام داد
مثال:            1, 2, 3
```

#### 3. Username
```
نوع:              Text (255)
خالی‌پذیر:       خیر
شرح:             نام‌کاربری برای دسترسی سریع
مثال:            admin, ali_user
نکته:            کپی از جدول Users برای سهولت
```

#### 4. ActionType
```
نوع:              Text (50)
خالی‌پذیر:       خیر
شرح:             نوع عملی که انجام شد
مقادیر ممکن:
  - "Login"
  - "Logout"
  - "UserCreated"
  - "UserUpdated"
  - "UserDeleted"
  - "PasswordChanged"
  - "OwnPasswordChanged"
  - "SettingsChanged"
  - "ConnectionTested"
  - "ConnectionTestFailed"

مثال:            UserCreated, PasswordChanged
```

#### 5. Description
```
نوع:              Text (500)
خالی‌پذیر:       خیر
شرح:             توضیحات تفصیلی عمل
مثال:            "کاربر جدید ایجاد شد: ali_user"
                "تنظیمات تغییر کرد: DatabasePath"
```

#### 6. OldValue
```
نوع:              Text (500)
خالی‌پذیر:       بلی
شرح:             مقدار قدیمی (برای ویرایش)
مثال:            "\\OLD_SERVER\db\Database.accdb"
نکته:            برای عملیات ایجاد خالی است
```

#### 7. NewValue
```
نوع:              Text (500)
خالی‌پذیر:       بلی
شرح:             مقدار جدید
مثال:            "\\NEW_SERVER\db\Database.accdb"
نکته:            برای عملیات حذف خالی است
```

#### 8. LogDateTime
```
نوع:              DateTime
خالی‌پذیر:       خیر
شرح:             تاریخ و ساعت ثبت لاگ
مثال:            2026-06-06 14:30:45
نکته:            خودکار توسط سیستم ثبت می‌شود
```

#### 9. IPAddress
```
نوع:              Text (50)
خالی‌پذیر:       بلی
شرح:             آدرس IP کاربر
مثال:            192.168.1.100, 10.0.0.50
نکته:            برای کاربران داخل‌شبکه
```

### مثال داده

```
LogID | UserID | Username | ActionType      | Description              | LogDateTime         | IPAddress
------|--------|----------|-----------------|--------------------------|---------------------|----------
1     | 1      | admin    | Login           | ورود موفق                | 2026-06-06 10:00:00 | 192.168.1.1
2     | 1      | admin    | UserCreated     | کاربر جدید: ali_user     | 2026-06-06 10:15:00 | 192.168.1.1
3     | 2      | ali_user | OwnPasswordChanged | تغییر رمز شخصی        | 2026-06-06 11:30:00 | 192.168.1.5
4     | 1      | admin    | SettingsChanged | تنظیمات تغییر کرد         | 2026-06-06 12:00:00 | 192.168.1.1
```

---

## جدول AppSettings

### توضیح
جدول **AppSettings** تنظیمات تعریف شده برای کل سیستم را ذخیره می‌کند.

### ستون‌ها

#### 1. SettingID
```
نوع:              AutoNumber
کلید اصلی:       بله (Primary Key)
خالی‌پذیر:       خیر
شرح:             شناسه منحصر برای هر تنظیم
مثال:            1, 2, 3
```

#### 2. SettingName
```
نوع:              Text (100)
خالی‌پذیر:       خیر
شرح:             نام تنظیم
مقادیر ممکن:
  - "DatabasePath"
  - "DatabaseUsername"
  - "DatabasePassword"

مثال:            DatabasePath
```

#### 3. SettingValue
```
نوع:              Text (500)
خالی‌پذیر:       خیر
شرح:             مقدار تنظیم
مثال:            "\\SERVER\shared\Database.accdb"
                "admin_db"
                "123|126|144|..." (رمزگذاری شده)
```

#### 4. IsEncrypted
```
نوع:              Yes/No (Boolean)
خالی‌پذیر:       خیر
شرح:             آیا تنظیم رمزگذاری شده است
مقادیر ممکن:    True (رمزگذاری شده) یا False (متن ساده)
پیش‌فرض:         False
نکته:            برای تنظیمات حساس True باشد
```

#### 5. ModifiedBy
```
نوع:              Text (255)
خالی‌پذیر:       خیر
شرح:             کاربری که تنظیم را ویرایش کرده
مثال:            admin, System
```

#### 6. ModifiedDate
```
نوع:              DateTime
خالی‌پذیر:       خیر
شرح:             تاریخ و ساعت آخرین ویرایش
مثال:            2026-06-06 14:30:45
```

#### 7. Description
```
نوع:              Text (255)
خالی‌پذیر:       بلی
شرح:             توضیحات تنظیم
مثال:            "مسیر پایگاه داده"
                "نام کاربری برای اتصال"
```

### مثال داده

```
SettingID | SettingName           | SettingValue                      | IsEncrypted | ModifiedBy | ModifiedDate
----------|----------------------|-----------------------------------|-------------|------------|------------------
1         | DatabasePath         | \\SERVER\shared\Database.accdb   | False       | System     | 2026-06-06 10:00
2         | DatabaseUsername     | 123|142|155|... (رمزگذاری)      | True        | admin      | 2026-06-06 10:15
3         | DatabasePassword     | 145|133|121|... (رمزگذاری)      | True        | admin      | 2026-06-06 10:15
```

---

## روابط بین جداول

### Relationship: Logs → Users

```
جدول Logs
    ↓
    Logs.UserID → Users.UserID (Foreign Key)
    ↓
جدول Users
```

**معنی:** هر رکورد در جدول Logs متعلق به یک کاربر در جدول Users است.

**نکات:**
- یک کاربر می‌تواند چندین لاگ داشته باشد
- اگر کاربر حذف شود، لاگ‌های آن نیز حذف می‌شود (Cascade)

---

## کوئری‌های مفید

### 1: تمام کاربران فعال

```sql
SELECT UserID, Username, FullName, Email, Role
FROM Users
WHERE IsActive = True
ORDER BY FullName
```

### 2: کاربران ادمین

```sql
SELECT UserID, Username, FullName, Email
FROM Users
WHERE Role = 'Admin'
```

### 3: آخرین ورودهای کاربران

```sql
SELECT Username, FullName, LastLoginDate
FROM Users
WHERE LastLoginDate IS NOT NULL
ORDER BY LastLoginDate DESC
```

### 4: لاگ‌های یک کاربر

```sql
SELECT LogID, ActionType, Description, LogDateTime
FROM Logs
WHERE Username = 'admin'
ORDER BY LogDateTime DESC
```

### 5: تغییرات تنظیمات

```sql
SELECT LogID, Username, Description, LogDateTime
FROM Logs
WHERE ActionType = 'SettingsChanged'
ORDER BY LogDateTime DESC
```

### 6: کاربران با تلاش‌های ناموفق زیاد

```sql
SELECT UserID, Username, FullName, LoginAttempts
FROM Users
WHERE LoginAttempts > 3
```

### 7: آخرین ۱۰ لاگ

```sql
SELECT TOP 10 Username, ActionType, Description, LogDateTime
FROM Logs
ORDER BY LogDateTime DESC
```

### 8: لاگ‌های امروز

```sql
SELECT LogID, Username, ActionType, LogDateTime
FROM Logs
WHERE DATE(LogDateTime) = TODAY()
ORDER BY LogDateTime DESC
```

---

## نکات مهم

### ایندکس‌ها
برای بهتر شدن عملکرد، توصیه می‌شود:
- `Users.Username` ایندکس شود (برای جستجو سریع‌تر)
- `Logs.LogDateTime` ایندکس شود (برای مرتب‌سازی سریع‌تر)

### Backup
- هر روز یک backup از پایگاه داده بگیرید
- لاگ‌های قدیمی را هر ۶ ماه پاک کنید

### امنیت
- هرگز رمز عبور را به صورت متن ساده ذخیره نکنید
- فقط ادمین بتواند تنظیمات را تغییر دهد
- تمام فعالیت‌ها در جدول Logs ثبت شود

---

## ✅ خلاصه

**جدول Users:** اطلاعات کاربران  
**جدول Logs:** ثبت فعالیت‌ها  
**جدول AppSettings:** تنظیمات سیستم  

