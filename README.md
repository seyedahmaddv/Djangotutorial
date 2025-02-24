### **جنگو (Django) چیست؟**  
**جنگو (Django)** یک **فریم‌ورک قدرتمند و سطح بالا** برای توسعه‌ی وب با **زبان پایتون** است که توسعه‌ی سریع، امنیت بالا و مقیاس‌پذیری را فراهم می‌کند. این فریم‌ورک بر اساس **الگوی MVC (Model-View-Controller)** ساخته شده و در جنگو به آن **MTV (Model-Template-View)** گفته می‌شود.  

---

## **چرا جنگو را انتخاب کنیم؟**  
✅ **توسعه‌ی سریع:** جنگو بسیاری از قابلیت‌های موردنیاز را از قبل دارد، بنابراین نیازی به کدنویسی از صفر نیست.  
✅ **امنیت بالا:** جنگو در برابر حملات رایج وب (SQL Injection، XSS، CSRF) مقاوم است.  
✅ **مقیاس‌پذیری:** از سایت‌های کوچک گرفته تا پروژه‌های بزرگ مانند اینستاگرام و پینترست از جنگو استفاده می‌کنند.  
✅ **یکپارچگی با پایگاه‌داده:** پشتیبانی از **PostgreSQL، MySQL، SQLite، MongoDB** و دیگر دیتابیس‌ها.  

---

## **آموزش نصب جنگو**  
قبل از هر چیز باید **پایتون** را روی سیستم خود نصب کنید. سپس، یک محیط مجازی ایجاد کرده و جنگو را نصب می‌کنیم.  

### **۱. نصب پایتون و ایجاد محیط مجازی**  
```bash
# نصب پایتون (اگر نصب نیست)
sudo apt install python3 python3-venv python3-pip  # برای لینوکس
# یا 
brew install python  # برای مک

# ایجاد یک محیط مجازی
python3 -m venv myenv
# فعال‌سازی محیط مجازی
source myenv/bin/activate  # در لینوکس و مک
myenv\Scripts\activate  # در ویندوز
```

### **۲. نصب جنگو**  
```bash
pip install django
```
برای اطمینان از نصب صحیح، دستور زیر را اجرا کنید:  
```bash
django-admin --version
```

---

## **ایجاد یک پروژه در جنگو**  
### **۱. ایجاد پروژه جدید**  
```bash
django-admin startproject myproject
cd myproject
```
**ساختار پروژه:**  
```
myproject/
│── manage.py
│── myproject/
│   ├── __init__.py
│   ├── settings.py  # تنظیمات اصلی پروژه
│   ├── urls.py  # مدیریت مسیرها
│   ├── asgi.py
│   ├── wsgi.py
```

### **۲. اجرای سرور جنگو**  
```bash
python manage.py runserver
```
با اجرای این دستور، **سرور لوکال جنگو راه‌اندازی می‌شود** و می‌توانید سایت را در **http://127.0.0.1:8000/** مشاهده کنید.  

---

## **ساخت یک اپلیکیشن در جنگو**  
در جنگو، پروژه می‌تواند چندین **اپلیکیشن (App)** داشته باشد. برای ساخت یک اپ جدید، دستور زیر را اجرا کنید:  
```bash
python manage.py startapp blog
```
این دستور پوشه‌ی جدیدی به نام `blog/` ایجاد می‌کند که شامل فایل‌های مربوط به اپلیکیشن است:  
```
blog/
│── migrations/
│── __init__.py
│── admin.py  # مدیریت پنل ادمین
│── apps.py
│── models.py  # تعریف مدل‌های دیتابیس
│── tests.py
│── views.py  # تعریف پردازش‌های مربوط به درخواست‌ها
```

---

## **تعریف مدل (Database Model) در جنگو**  
فایل `models.py` را باز کرده و مدل زیر را اضافه کنید:  
```python
from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=255)  # عنوان پست
    content = models.TextField()  # محتوای پست
    created_at = models.DateTimeField(auto_now_add=True)  # زمان انتشار

    def __str__(self):
        return self.title
```
### **اجرای مهاجرت دیتابیس**  
بعد از تعریف مدل، باید **مهاجرت دیتابیس** را اجرا کنیم:  
```bash
python manage.py makemigrations
python manage.py migrate
```

---

## **ایجاد صفحه نمایش داده‌ها (Views و Templates)**  
### **۱. اضافه کردن View در `views.py`**  
```python
from django.shortcuts import render
from .models import Post

def home(request):
    posts = Post.objects.all()
    return render(request, 'blog/home.html', {'posts': posts})
```

### **۲. تعریف مسیرها در `urls.py`**  
فایل `urls.py` را ویرایش کنید و مسیر مربوط به صفحه اصلی را اضافه کنید:  
```python
from django.urls import path
from .views import home

urlpatterns = [
    path('', home, name='home'),
]
```

### **۳. ایجاد فایل قالب HTML**  
در پوشه `blog/` یک پوشه به نام `templates/blog/` ایجاد کنید و یک فایل `home.html` بسازید:  
```html
<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <title>بلاگ</title>
</head>
<body>
    <h1>لیست مقالات</h1>
    <ul>
        {% for post in posts %}
            <li>{{ post.title }} - {{ post.created_at }}</li>
        {% endfor %}
    </ul>
</body>
</html>
```

---

## **مدیریت پنل ادمین در جنگو**  
جنگو یک **پنل مدیریت داخلی** دارد که به شما اجازه می‌دهد محتوای سایت را بدون نیاز به کدنویسی مدیریت کنید.  

### **۱. ایجاد حساب ادمین**  
```bash
python manage.py createsuperuser
```
سپس اطلاعات ورود را وارد کنید و سرور را اجرا کنید:  
```bash
python manage.py runserver
```
حالا به آدرس **http://127.0.0.1:8000/admin/** بروید و با نام کاربری و رمز عبور خود وارد شوید.  

### **۲. ثبت مدل‌ها در پنل ادمین**  
برای اینکه مدل `Post` در پنل ادمین نمایش داده شود، فایل `admin.py` را ویرایش کنید:  
```python
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

---

## **جمع‌بندی**  
🔹 **جنگو یک فریم‌ورک سریع، امن و مقیاس‌پذیر** برای توسعه وب با پایتون است.  
🔹 با جنگو می‌توان **سایت‌های شرکتی، وبلاگ، فروشگاه اینترنتی و حتی شبکه‌های اجتماعی** طراحی کرد.  
🔹 جنگو شامل **ابزارهای آماده برای مدیریت دیتابیس، احراز هویت کاربران و پنل مدیریت** است.  
🔹 اگر به دنبال توسعه **سایت‌های مدرن و بهینه‌شده برای سئو** هستید، **Next.js و React** گزینه‌های قدرتمندتری برای فرانت‌اند هستند.  

📌 **سوال یا پروژه‌ای دارید؟ من می‌توانم برای شما یک سایت حرفه‌ای با Next.js، React یا حتی جنگو طراحی کنم. برای مشاوره رایگان تماس بگیرید! 🚀**
