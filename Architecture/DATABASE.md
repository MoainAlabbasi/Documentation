# 🗄️ DATABASE - تصميم قاعدة البيانات

> **⚠️ تعليمات:** هذا الملف يوثق تصميم قاعدة البيانات بناءً على مخطط ERD في وثيقة المشروع الأصلية.

---

## نظرة عامة

قاعدة البيانات مصممة لدعم نظام إدارة المحتوى الأكاديمي الذكي (S-ACM). تتضمن جداول لإدارة المستخدمين، المقررات، المحتوى، الإشعارات، والتفاعلات.

---

## الجداول الرئيسية (Main Tables)

### 1. جدول المستخدمين (Users)

| الحقل | النوع | الوصف |
| :--- | :--- | :--- |
| `id` | Integer (PK) | المعرف الفريد |
| `username` | String | اسم المستخدم |
| `email` | String | البريد الإلكتروني |
| `password` | String (Hashed) | كلمة المرور المشفرة |
| `role` | Enum | الدور (طالب، مدرس، مسؤول) |
| `department_id` | Integer (FK) | القسم |
| `level` | Integer | المستوى الدراسي (للطلاب) |
| `created_at` | DateTime | تاريخ الإنشاء |

### 2. جدول الأقسام (Departments)

| الحقل | النوع | الوصف |
| :--- | :--- | :--- |
| `id` | Integer (PK) | المعرف الفريد |
| `name` | String | اسم القسم |

### 3. جدول المقررات (Courses)

| الحقل | النوع | الوصف |
| :--- | :--- | :--- |
| `id` | Integer (PK) | المعرف الفريد |
| `code` | String | رمز المقرر |
| `name` | String | اسم المقرر |
| `department_id` | Integer (FK) | القسم |
| `level` | Integer | المستوى |
| `instructor_id` | Integer (FK) | المدرس المسؤول |
| `hours` | Integer | عدد الساعات |

### 4. جدول المحتوى (Content)

| الحقل | النوع | الوصف |
| :--- | :--- | :--- |
| `id` | Integer (PK) | المعرف الفريد |
| `course_id` | Integer (FK) | المقرر |
| `title` | String | عنوان المحتوى |
| `type` | Enum | نوع المحتوى (محاضرة، ملف، رابط) |
| `file_path` | String | مسار الملف |
| `uploaded_by` | Integer (FK) | المستخدم الذي رفع المحتوى |
| `uploaded_at` | DateTime | تاريخ الرفع |

### 5. جدول الإشعارات (Notifications)

| الحقل | النوع | الوصف |
| :--- | :--- | :--- |
| `id` | Integer (PK) | المعرف الفريد |
| `user_id` | Integer (FK) | المستخدم المستهدف |
| `title` | String | عنوان الإشعار |
| `message` | Text | نص الإشعار |
| `is_read` | Boolean | هل تم قراءته |
| `created_at` | DateTime | تاريخ الإنشاء |

### 6. جدول تسجيل المقررات (Enrollments)

| الحقل | النوع | الوصف |
| :--- | :--- | :--- |
| `id` | Integer (PK) | المعرف الفريد |
| `student_id` | Integer (FK) | الطالب |
| `course_id` | Integer (FK) | المقرر |
| `enrolled_at` | DateTime | تاريخ التسجيل |

### 7. جدول ملخصات الذكاء الاصطناعي (AI_Summaries)

| الحقل | النوع | الوصف |
| :--- | :--- | :--- |
| `id` | Integer (PK) | المعرف الفريد |
| `content_id` | Integer (FK) | المحتوى الأصلي |
| `summary_text` | Text | نص الملخص |
| `generated_at` | DateTime | تاريخ التوليد |

---

## مخطط العلاقات (ERD Summary)

```
Users ─────┬───── Departments
           │
           ├───── Courses ───── Content ───── AI_Summaries
           │
           ├───── Enrollments
           │
           └───── Notifications
```

> **📍 للمخطط التفصيلي:** راجع الصفحات 35-40 من وثيقة المشروع الأصلية في `Original_Docs/`.
