# Command

| Command                                       | Description                                         |
| --------------------------------------------- | --------------------------------------------------- |
| `django-admin startproject <project-name>`    | Create django project                               |
| `django-admin startapp <app-name>`            | Create django app                                   |
| `python3 manage.py runserver`                 | Run django server                                   |
| `python3 manage.py makemigrations <app-name>` | Let django know there is a change in models.py      |
| `python manage.py sqlmigrate <app-name> 0001` | view SQL will be generated from a migration         |
| `python manage.py migrate`                    | Apply changes in models down to the database schema |
| `python manage.py shell`                      | Work with django on shell                           |

# Getting started
## Create environment
```shell
# Create virtual environment
python3 -m venv .venv
```
Then activate virtual environment base on your OS
- **Windows**
```shell
# Activate virtual environment
.\.venv\Scripts\activate
# Assign PYTHONPATH 
$env:PYTHONPATH = (Get-Location).Path
```
- **Linux**
```shell
# Activate virtual environment
source venv/bin/activate
# Assign PYTHONPATH 
export PYTHONPATH=$(pwd)
```
Then install django and create project
```shell
# Install django
python3 -m pip install django

# Create project
django-admin startproject <project-name>

# Go to project
cd <project-name>

# Start your first web app
python3 manage.py runserver 0.0.0.0:8000
```
## Database
Go to your \<project-name> then edit file settings.py
```shell
DATABASES = {
	'default': {
		'ENGINE': 'django.db.backends.mysql',    # sqlite3, postgresql
		'NAME': 'coursedb',     # your database name
		'USER': 'root',         # username
		'PASSWORD': '1234', # password
		'HOST': ''              # default is localhost
	} 
}
```
Then install `mysqlclient`
```shell
# Install mysqlclient
python3 -m pip install mysqlclient
```
## Create your first app
```shell
# Create app
django-admin startapp <app-name>
```
### How to add url to app
Create your first view in `<app-name>/views.py`
```python
from django.http import HttpResponse

def index(request):  # must has request
    return HttpResponse("e-Course App")   # this line will render text to your page
```
In your `<app-name>` create `urls.py` file then add this code:
```python
from django.urls import path
from . import views

# all the views you want to have just need to add it in here
urlpatterns = [
	# import index views, you can name it or leave it blank
    path('', views.index, name='index')  
]
```
Then go to `<project-name>/urls.py` to include `<app-name>/urls.py`
```python
from django.contrib import admin    # default
from django.urls import path, include  # add include

urlpatterns = [
    path('admin/', admin.site.urls),  # default
    path('', include('<app-name>.urls')),  # include app urls
]
```
## Add app to django
We need to let django know about the existence of the courses app via the `INSTALLED_APP` variable in the `settings.py` configuration file.
```python
# If you have many configs in your <app-name>/app.py file
INSTALLED_APPS = [
	...
	'<app-name>.apps.<app-name>Config'
]

# If you only have one config or not
INSTALLED_APPS = [
	...
	'<app-name>'
]
```
# Model
https://docs.djangoproject.com/en/5.2/topics/db/models/
## Field
https://docs.djangoproject.com/en/5.2/ref/models/fields/
### ImageField
- Must install `Pillow` pakage before use `ImageField`
- In `settings.py` add `MEDIA_ROOT = '%s/<app-name>/static/' % BASE_DIR` or you can change this directory, this variable is place to save your images.
### CloudinaryField
- Install `cloudinary` package
- Then import `from cloudinary.models import CloudinaryField`
- In your `settings.py` add
```python
import cloudinary

cloudinary.config(
    cloud_name="dxxwcby8l",
    api_key="448651448423589",
    api_secret="ftGud0r1TTqp0CGp5tjwNmkAm-A"
)
```
## Class Meta
`Meta` in a Django model is a special inner class used to **configure metadata options** for the model — put simply, it’s how you tell Django: “I want this model to behave in a special way like this!”
```python
class ModelBase(models.Model):
    created_date = models.DateTimeField(auto_now_add=True)
    updated_date = models.DateTimeField(auto_now=True)
    active = models.BooleanField(default=True)
    # If you want to use ImageField you must install Pillow package
    image = models.ImageField(upload_to='courses/%Y/%m/', null=True, blank=True)
    class Meta:
        abstract = True    # must have to inherit
        ordering = ['-id'] # sort descending by id

class Category(models.Model):
    name = models.CharField(max_length=100, unique=True)
    def __str__(self):
        return self.name

class Course(ModelBase):
    subject = models.CharField(max_length=100, unique=True)
    category = models.ForeignKey(Category, on_delete=models.CASCADE)

    class Meta:
        unique_together = ('subject', 'category') 
        # subject = math, category = IT
        # if we add one more course
        # subject = math, cat
    def __str__(self):
        return self.subject
```
## Foreign key
- You can read more about Forgein key arguments in offical page [Django docs](https://docs.djangoproject.com/en/5.2/ref/models/fields/#django.db.models.ForeignKey)
### Many to one
- [Django docs](https://docs.djangoproject.com/en/5.2/topics/db/examples/many_to_one/)
```python
class Reporter(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    email = models.EmailField()

    def __str__(self):
        return f"{self.first_name} {self.last_name}"

class Article(models.Model):
    headline = models.CharField(max_length=100)
    pub_date = models.DateField()
    reporter = models.ForeignKey(Reporter, on_delete=models.CASCADE)

    def __str__(self):
        return self.headline

    class Meta:
        ordering = ["headline"]
```
### Many to Many
- [Django docs](https://docs.djangoproject.com/en/5.2/topics/db/examples/many_to_many/)
- In this example, an `Article` can be published in multiple `Publication` objects, and a `Publication` has multiple `Article` objects:
```python
class Publication(models.Model):
    title = models.CharField(max_length=30)

    class Meta:
        ordering = ["title"]

    def __str__(self):
        return self.title

class Article(models.Model):
    headline = models.CharField(max_length=100)
    publications = models.ManyToManyField(Publication)

    class Meta:
        ordering = ["headline"]

    def __str__(self):
        return self.headline
```
### One to one
- [Django docs](https://docs.djangoproject.com/en/5.2/topics/db/examples/one_to_one/)
- In this example, a `Place` optionally can be a `Restaurant`:
```python
class Place(models.Model):
    name = models.CharField(max_length=50)
    address = models.CharField(max_length=80)

    def __str__(self):
        return f"{self.name} the place"

class Restaurant(models.Model):
    place = models.OneToOneField(
        Place,
        on_delete=models.CASCADE,
        primary_key=True,      # optional
    )
    serves_hot_dogs = models.BooleanField(default=False)
    serves_pizza = models.BooleanField(default=False)

    def __str__(self):
        return "%s the restaurant" % self.place.name
```
### Access to related field attribute
- For example we have `Course` and `Category` model
```python
class Category(models.Model):
	name = models.CharField(max_length=100, unique=True)

class Course(BaseModel):
    subject = models.CharField(max_length=100, unique=True)
	category = models.ForeignKey(Category, on_delete=models.CASCADE)
```
- We want to get `Category` `name` attribute via `Course`
```python

```
## Model inheritance
- [Django docs](https://docs.djangoproject.com/en/5.2/topics/db/models/#model-inheritance)
- Abstract base classes are useful when you want to put some common information into a number of other models. You write your base class and put `abstract=True` in the [Meta](https://docs.djangoproject.com/en/5.2/topics/db/models/#meta-options) class. This model will then not be used to create any database table. Instead, when it is used as a base class for other models, its fields will be added to those of the child class.
```python
class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        abstract = True

class Student(CommonInfo):
    home_group = models.CharField(max_length=5)
```
## Class Meta inheritance
- [Django docs](https://docs.djangoproject.com/en/5.2/topics/db/models/#meta-inheritance)
```python
class CommonInfo(models.Model):
    name = models.CharField(max_length=100)
    age = models.PositiveIntegerField()

    class Meta:
        abstract = True
        ordering = ["name"]

class Unmanaged(models.Model):
    class Meta:
        abstract = True
        managed = False

class Student(CommonInfo, Unmanaged):
    home_group = models.CharField(max_length=5)

    class Meta(CommonInfo.Meta, Unmanaged.Meta):
        pass
```
## Making queries
- [Django docs](https://docs.djangoproject.com/en/5.2/topics/db/queries/)
- create()
- update()
- delete()
- save()
- get_or_create()
- updated_or_create()
- count()
- latest()
- earliest()
- first()
- last()
- exists()
- aggregate()
- object:
	+ all()
	+ filter()
		+ arg_contains
		+ arg_icontains
		+ arg_in
		+ arg_endswith
		+ ....
	+ exclude()

## Proxy models
- Learn more in [django docs](https://docs.djangoproject.com/en/5.2/topics/db/models/#proxy-models)
## Exercise
### Lazy loading
- Create 2 model like this
```python
class Category(models.Model):
    name = models.CharField(max_length=100, unique=True)

class Course(BaseModel):
    subject = models.CharField(max_length=100, unique=True)
    category = models.ForeignKey(Category, on_delete=models.CASCADE)
```
- Print `Category` `name` attribute via first `Course`
```python
# Method 1: Fail because Django is using lazy loading
ls = Course.objects.all()
print(ls[0].category.name)

# Method 2: True
ls = Course.objects.select_related('category').all()
print(ls[0].category.name)
```
## Implicit ForeignKey field
```python
# category_id is not a field you define in the model, but Django automatically creates it when you use ForeignKey
print(Course.objects.filter(category_id=1))
```
# Admin
> To get to the admin view just add /admin in your URL
## createsuperuser
- First we need an admin account, just type `python3 manage.py createsuperuser` to create one.
## Site register
- Add this code in `admin.py` file
```python
from django.contrib import admin
from .models import Category, Course

admin.site.register(Category) 
admin.site.register(Course)
```
## Register decorator
### Dash board
- We want to show more infomation about an object
```python
@admin.register(Lesson)
class LessonAdmin(admin.ModelAdmin):
    list_display = ['id', 'subject', 'created_date', 'course']
    list_filter = ['subject', 'created_date']
    search_fields = ['subject', 'course__subject']
	
# Or you can do this
class LessonAdmin(admin.ModelAdmin):
    list_display = ['id', 'subject', 'created_date', 'course']
    list_filter = ['subject', 'created_date']
    search_fields = ['subject', 'course__subject']
	
admin.site.register(Lesson, LessonAdmin)
```
### Display image
- Read about ImageField
```python 
@admin.register(Course)
class CourseAdmin(admin.ModelAdmin):
    list_display = ['id', 'subject', 'description']

    # Add avatar display in Courses
    readonly_fields = ['avatar']
    def avatar(self, obj):
        if obj:
            return mark_safe(
                '<img src="/static/{url}" width="120" />'.format(url=obj.image.name)
            )
```
## Add CSS and JS
```python
@admin.register(Lesson)
class LessonAdmin(admin.ModelAdmin):
    list_display = ['id', 'subject', 'created_date', 'course']
    list_filter = ['subject', 'created_date']
    search_fields = ['subject', 'course__subject']
    
    # Add CSS and JS
    class Media:
        css = {
            'all': ('/static/css/style.css', )
        } 
        js = ('/static/js/script.js', )
```
## Customize admin site
- [Django docs](https://docs.djangoproject.com/en/5.2/ref/contrib/admin/#django.contrib.admin.ModelAdmin.get_urls)
- Create folder like this in your app `templates/admin/`
- Then create a `HTML` file with structure like this
```html
{% extends "admin/base_site.html" %}
{% block content %}
...
Add any you like in here
{% endblock %}
```
- In your `admin.py`, in this example we will create stats-view with courses and lessons in each course
```python
class CourseAppAdminSite(admin.AdminSite):
    site_header = 'Hệ thống khoá học trực tuyến'

    def get_urls(self):
        urls = super().get_urls()
        my_urls = [path("stats_view/", self.stats_view)]
        return my_urls + urls

    def stats_view(self, request):
        count = Course.objects.filter(active=True).count()

        stats = Course.objects.annotate(
            lesson_count=Count('my_lesson')
        ).values('id', 'subject', 'lesson_count')

        return TemplateResponse(request, 'admin/course-stats.html', {
            'course_count': count,
            'course_stats': stats
        })

admin_site = CourseAppAdminSite(name='myadmin')
```
- Then go to `urls.py` add
```python
urlpatterns = [
	...
    # path('admin/', admin.site.urls),
    path('admin/', admin_site.urls),
]
```


# django-ckeditor-5
- Run `pip install django-ckeditor-5`
- Then in your `INSTALLED_APP` variable add `django_ckeditor_5`
```python
INSTALLED_APPS = [
	...
    'django_ckeditor_5',
]
```
- Go to https://pypi.org/project/django-ckeditor-5/ to get config and add to the end `settings.py` file
# Authenticated
## AbstractUser
- Customize your user in `model.py` file
```python
from django.contrib.auth.models import AbstractUser

class MyUser(User):
    avatar = models.ImageField(upload_to='upload/')
```
- Then add this line to `settings.py`
```python
AUTH_USER_MODEL='<app-name>.MyUser'
```
# Django Rest API
## Getting started
- [Django Rest API docs](https://www.django-rest-framework.org/tutorial/quickstart/)
- Install package `djangorestframework`
```shell
pip install django
pip install djangorestframework
```
- Then go to `settings.py` add this line:
```python
INSTALLED_APPS = [ 
	... 
	'rest_framework',
]
```
- Create `serializers.py` in your app then add:
```python
from django.contrib.auth.models import Group, User
from rest_framework import serializers


class UserSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = User
        fields = ['url', 'username', 'email', 'groups']


class GroupSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Group
        fields = ['url', 'name']
```
- Right, we'd better write some views then. Open `appname/views.py` and get typing.
```python
from django.contrib.auth.models import Group, User
from rest_framework import permissions, viewsets

from tutorial.quickstart.serializers import GroupSerializer, UserSerializer


class UserViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows users to be viewed or edited.
    """
    queryset = User.objects.all().order_by('-date_joined')
    serializer_class = UserSerializer
    permission_classes = [permissions.IsAuthenticated]


class GroupViewSet(viewsets.ModelViewSet):
    """
    API endpoint that allows groups to be viewed or edited.
    """
    queryset = Group.objects.all().order_by('name')
    serializer_class = GroupSerializer
    permission_classes = [permissions.IsAuthenticated]
```
- Okay, now let's wire up the API URLs. On to `appname/urls.py`
```python
from django.urls import include, path
from rest_framework import routers

from tutorial.quickstart import views

router = routers.DefaultRouter()
router.register(r'users', views.UserViewSet)
router.register(r'groups', views.GroupViewSet)

# Wire up our API using automatic URL routing.
# Additionally, we include login URLs for the browsable API.
urlpatterns = [
    path('', include(router.urls)),
    path('api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```
- Run server and access http://127.0.0.1:8000/api-auth/
## Pagination
### Global
- [Django Rest API docs](https://www.django-rest-framework.org/tutorial/quickstart/#pagination)
- Add this to `settings.py` file
```python
REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 
    'rest_framework.pagination.PageNumberPagination',
    'PAGE_SIZE': 2
}
```
### Local
- Create file `paginators.py`
```python
from rest_framework import pagination

class YourPaginator(pagination.PageNumberPagination):
    page_size = 6
```
- In your `views.py` append to your viewset `pagination_class = YourPaginator`, example:
```python
class CourseViewSet(viewsets.ModelViewSet):
    queryset = Course.objects.filter(active=True)
    serializer_class = CourseSerializer
    permission_classes = [permissions.IsAuthenticated]
    pagination_class = CoursePaginator
```
# Test with drf_yasg
## Getting started
- Install package `python -m pip install drf_yasg`
- Add this to `settings.py` file
```python
INSTALLED_APPS = [
    'drf_yasg',
]
```
- Add this to `<project-name>/urls.py`
```python
from drf_yasg.views import get_schema_view
from drf_yasg import openapi

schema_view = get_schema_view(
    openapi.Info(
        title="Course APIs",
        default_version='1.0',
        description="APIs for CourseApp",
        contact=openapi.Contact(email="datlevipprono1@gmail.com"),
        license=openapi.License(name="Lê Văn Đạt@2025"),
    ),
    public=True,
    permission_classes=(permissions.AllowAny,),
)

urlpatterns = [
	...
	    # Swagger để test API
    re_path(r'^swagger(?P<format>\.json|\.yaml)$',
            schema_view.without_ui(cache_timeout=0),
            name='schema-json'),
            re_path(r'^swagger/$',
            schema_view.with_ui('swagger', cache_timeout=0),
            name='schema-swagger-ui'),
    # Redoc để xem chi tiết các endpoint GET/POST/PUT...
    re_path(r'^redoc/$',
            schema_view.with_ui('redoc', cache_timeout=0),
            name='schema-redoc'),
]
```
## swagger
- Truy cập http://localhost:8000/swagger/
## redoc
- Truy cập http://localhost:8000/redoc/
## ViewSet
### Authenticated
- 
## More
### HyperlinkedModelSerializer
### Override UserSerializer's create method
# Some common error
## Can't migrate even everything correct
- Try remove the `migrations` folder.
# Some importance things
- **Model**:
	- **Field**:
	    - `CharField`, `TextField`, `IntegerField`, `BooleanField`, `DateTimeField`, ...
	    - `ImageField`: dùng để lưu hình ảnh, cần cài thêm `Pillow`
	    - `ForeignKey`: thiết lập quan hệ nhiều–một
	    - `ManyToManyField`: thiết lập quan hệ nhiều–nhiều
	    - `OneToOneField`: quan hệ một–một
	- **Class Meta**:
	    - `db_table`: đặt tên bảng trong DB
	    - `ordering`: mặc định sắp xếp
	    - `verbose_name`, `verbose_name_plural`
	- **ForeignKey (hoặc các liên kết khác)**:
	    - `related_name`: tên dùng để truy ngược từ model liên kết
	    - `related_query_name`: tên dùng để truy vấn khi dùng `filter()`
	- **Inheritance** (kế thừa model):
	    - Abstract Base Classes (chung cấu trúc)
	    - Multi-table Inheritance (chia bảng)
	    - Proxy models (thay đổi hành vi)
- **Serializer**
	- **Base Classes**
	    - `serializers.Serializer`: Serializer thuần (non-model), cần khai báo fields thủ công.
	    - `serializers.ModelSerializer`: Tự động ánh xạ từ model → fields.
	- **Fields**
	    - `CharField`, `IntegerField`, `ImageField`, `DateTimeField`, `BooleanField`, `SerializerMethodField`, v.v.
	    - `read_only`, `write_only`, `required`, `default`, `allow_blank`, `allow_null`
	- **SerializerMethodField**
	    - Cho phép định nghĩa logic tuỳ chỉnh cho field:
```python
class CourseSerializer(HyperlinkedModelSerializer):
    # Ghi đè để trả về đường dẫn tuyệt đối của hình
    # Before(ModelSerializer)            "image": "http://127.0.0.1:8000/courses/2025/04/myimg.jpg"
    # After(HyperlinkedModelSerializer)  "image": "http://127.0.0.1:8000/static/courses/2025/04/myimg.jpg"
    image = SerializerMethodField()
    def get_image(self, obj):
        request = self.context.get('request')
        if not obj.image:
            return None
        if obj.image.name.startswith('static/'):
            path = f"/{obj.image.name}"
        else:
            path = f"/static/{obj.image}"
        return request.build_absolute_uri(path)
```
``
	- **Validation**
	    - `validate_<field_name>(self, value)`: validate riêng từng field
	    - `validate(self, attrs)`: validate toàn bộ dữ liệu
	- **Nested Serializer**
	    - Dùng để serialize quan hệ (ForeignKey, ManyToMany...):
```python
class TagSerializer(ModelSerializer):
    class Meta:
        model = Tag
        fields = ['id', 'name']


class LessonSerializer(ModelSerializer):
    # Nested Serializer
    tags = TagSerializer(many=True)
    class Meta:
        model = Lesson
        fields = ['id', 'subject', 'content', 'created_date', 'updated_date', 'tags']
```
``
	- **Overriding `create()` / `update()`**
	    - Dùng để xử lý logic đặc biệt khi tạo/cập nhật object
