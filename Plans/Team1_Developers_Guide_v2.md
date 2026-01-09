# 📖 دليل العمل التفصيلي للفريق الأول (المبرمجين) - الإصدار 2.0

## مقدمة

هذا الدليل المحدّث يقدم خطوات تفصيلية (Step-by-Step) للفريق الأول (أنت ومهند) لتطوير مشروع S-ACM. تم إثراء هذا الإصدار بتوضيحات إضافية، روابط لفيديوهات تعليمية، وعبارات بحث جاهزة لتسهيل عملية التعلم وحل المشاكل.

---

## 🛠️ الأدوات والتقنيات المستخدمة

| الأداة/التقنية | الوصف |
|---|---|
| **نظام التشغيل** | Windows (مع WSL2) |
| **بيئة التطوير المتكاملة (IDE)** | VS Code (موصى به) |
| **إدارة الحاويات** | Docker و Docker Compose |
| **لغة البرمجة** | Python 3.11+ |
| **إطار العمل** | Django 5.x |
| **قاعدة البيانات** | PostgreSQL |
| **نظام التحكم بالإصدارات** | Git و GitHub |
| **واجهة برمجة تطبيقات الذكاء الاصطناعي** | Google Gemini API |

---

## 🚀 الخطوات التفصيلية (Step-by-Step)

### المرحلة 1: الإعداد الأولي لبيئة التطوير (اليوم 1-2)

**الهدف:** توحيد بيئة العمل بشكل كامل باستخدام Docker و WSL2 لضمان عدم وجود أي اختلافات بين أجهزة المطورين.

#### 1.1. تثبيت المتطلبات الأساسية (على نظام Windows)

| الخطوة | الشرح | مصادر مساعدة |
|---|---|---|
| **1. تفعيل WSL2** | نظام Windows الفرعي لـ Linux يسمح بتشغيل بيئة Linux حقيقية داخل Windows، وهو ضروري لتشغيل Docker بكفاءة. | - **فيديو:** [شرح WSL2 وتثبيت Ubuntu](https://www.youtube.com/watch?v=4z-0xJgB_gA)<br>- **بحث:** `"how to install wsl2 on windows 11"` |
| **2. تثبيت Docker Desktop** | الأداة الرئيسية لإدارة الحاويات. تأكد من تفعيل خيار "Use WSL 2 based engine" أثناء التثبيت. | - **فيديو:** [شرح Docker Desktop للمبتدئين](https://www.youtube.com/watch?v=ieHB004jARI)<br>- **بحث:** `"install docker desktop on windows wsl2"` |
| **3. تثبيت VS Code + إضافة WSL** | محرر الأكواد الموصى به. إضافة WSL تسمح لك بالعمل على ملفات المشروع الموجودة داخل بيئة Linux مباشرة من VS Code. | - **فيديو:** [شرح VS Code مع WSL2](https://www.youtube.com/watch?v=N7qWNQxFq90)<br>- **بحث:** `"vs code wsl2 integration tutorial"` |
| **4. تثبيت Git** | نظام التحكم بالإصدارات. سيتم استخدامه داخل بيئة WSL. | - **فيديو:** [كورس تعلم Git و GitHub في ساعة](https://www.youtube.com/watch?v=fDkR0TDR9dI)<br>- **بحث:** `"how to install git on ubuntu wsl2"` |

#### 1.2. إعداد المشروع داخل WSL

1.  **أنت (معين):** قم بإنشاء مستودع المشروع الرئيسي (`S-ACM-Project`) على GitHub.
2.  **افتح Ubuntu Terminal** (من قائمة ابدأ).
3.  **استنسخ المستودع:**
    ```bash
    git clone https://github.com/MoainAlabbasi/S-ACM-Project.git
    cd S-ACM-Project
    ```
4.  **افتح المشروع في VS Code (داخل WSL):**
    ```bash
    code .
    ```
    (هذا الأمر سيفتح VS Code متصلاً ببيئة WSL تلقائياً)

#### 1.3. إعداد Docker Compose للمشروع

- **الهدف:** تعريف الخدمات التي سيعمل عليها المشروع (تطبيق الويب وقاعدة البيانات) وكيفية تواصلها مع بعضها.
- **المصادر:**
    - **فيديو:** [شرح Docker Compose مع Django و Postgres](https://www.youtube.com/watch?v=LEy-Y_A-iFw)
    - **بحث:** `"docker compose django postgresql tutorial"`

- **أنشئ الملفات التالية في جذر المشروع:**

  - **`docker-compose.yml`:** (نفس المحتوى الموجود في الدليل السابق)
  - **`Dockerfile`:** (نفس المحتوى الموجود في الدليل السابق)
  - **`requirements.txt`:**
    ```
    Django==5.0.1
    psycopg2-binary
    python-dotenv
    google-generativeai
    gunicorn # لخادم الإنتاج لاحقاً
    ```
  - **`.env`:** (لا ترفعه إلى GitHub!)
    ```
    DB_NAME=acm_db
    DB_USER=acm_user
    DB_PASSWORD=your_strong_password
    SECRET_KEY=your_django_secret_key_here
    DEBUG=True
    GEMINI_API_KEY=
    ```

#### 1.4. بناء وتشغيل المشروع لأول مرة

- افتح Terminal جديد **داخل VS Code** (سيفتح تلقائياً في WSL).

```bash
# 1. بناء الحاويات (يُنفذ مرة واحدة أو عند تغيير Dockerfile)
docker-compose build

# 2. تشغيل الحاويات في الخلفية
docker-compose up -d

# 3. إنشاء مشروع Django داخل الحاوية (يُنفذ مرة واحدة فقط)
docker-compose exec web django-admin startproject acm_project .

# 4. إنشاء تطبيق Django (يُنفذ مرة واحدة فقط)
docker-compose exec web python manage.py startapp core
```

- **تعديل `acm_project/settings.py`:** (نفس التعديلات المذكورة في الدليل السابق لربط قاعدة البيانات والمتغيرات).
- **تطبيق الترحيلات (Migrations) وإنشاء مستخدم مدير:**

```bash
docker-compose exec web python manage.py migrate
docker-compose exec web python manage.py createsuperuser
```

- **التحقق:** زر `http://localhost:8000` في متصفحك. يجب أن ترى صفحة Django الترحيبية.

---

### المرحلة 2: بناء نماذج البيانات ولوحة التحكم (اليوم 3-4)

| الخطوة | الشرح | مصادر مساعدة |
|---|---|---|
| **1. كتابة النماذج** | ترجمة مخطط ERD إلى كود في `core/models.py`. | - **فيديو:** [شرح Django Models بالتفصيل](https://www.youtube.com/watch?v=RbJOmgTX63M)<br>- **بحث:** `"django models tutorial for beginners"` |
| **2. تطبيق الترحيلات** | `makemigrations` لإنشاء ملفات الترحيل، و `migrate` لتطبيقها على قاعدة البيانات. | - **فيديو:** [شرح Django Migrations](https://www.youtube.com/watch?v=h9m1z2b-p4k)<br>- **بحث:** `"django migrations explained"` |
| **3. تسجيل النماذج في لوحة التحكم** | تعديل `core/admin.py` لجعل النماذج قابلة للإدارة من لوحة تحكم Django. | - **فيديو:** [تخصيص Django Admin](https://www.youtube.com/watch?v=cZJA302s2D4)<br>- **بحث:** `"django admin customization tutorial"` |

---

### المرحلة 3: بناء نظام المستخدمين والصلاحيات (اليوم 5-6)

| الخطوة | الشرح | مصادر مساعدة |
|---|---|---|
| **1. نظام المصادقة** | بناء صفحات ووظائف تسجيل الدخول، تسجيل الخروج، وإنشاء حساب جديد. | - **فيديو:** [كورس كامل لنظام المستخدمين في Django](https://www.youtube.com/playlist?list=PL-51WBLyFTg2vW-_6XBoUpE7vpQpQyFtg)<br>- **بحث:** `"django authentication tutorial login register"` |
| **2. نظام الصلاحيات (RBAC)** | حماية الصفحات والوظائف بناءً على دور المستخدم (طالب، مدرس، مسؤول). | - **فيديو:** [شرح صلاحيات المستخدمين في Django](https://www.youtube.com/watch?v=A-i_g8OKBqA)<br>- **بحث:** `"django user roles and permissions tutorial"` |

---

### المراحل 4-7 (بقية الأيام)

- **اتبع نفس الخطوات المذكورة في الدليل السابق**، واستخدم عبارات البحث التالية عند الحاجة:

| المرحلة | عبارات بحث مقترحة |
|---|---|
| **إدارة المحتوى** | `"django file upload tutorial"`, `"django display files from model"` |
| **نظام الإشعارات** | `"django notification system tutorial"`, `"django signals for notifications"` |
| **لوحات التحكم** | `"django dashboard tutorial with charts"`, `"django chart.js integration"` |
| **دمج Gemini API** | `"how to use google gemini api with python django"`, `"django generative ai tutorial"` |
| **الاختبار** | `"django unit testing tutorial for beginners"`, `"django pytest setup"` |
| **النشر (Deployment)** | `"deploy django docker to railway"`, `"django docker production setup gunicorn nginx"` |

---

## 💡 نصائح إضافية

- **استخدم سير عمل Git:** التزم باستراتيجية الفروع التي تم شرحها في `Guides/GIT_WORKFLOW.md`.
- **اكتب رسائل Commit واضحة:** هذا يساعد الفريق على فهم التغييرات.
- **لا تتردد في البحث:** مجتمع Django و Docker ضخم جداً، وأي مشكلة تواجهك قد واجهت شخصاً آخر وتم حلها.
