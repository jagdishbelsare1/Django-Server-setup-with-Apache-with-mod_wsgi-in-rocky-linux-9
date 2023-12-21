# Django-Server-setup-with-Apache-with-mod_wsgi-in-rocky-linux-9
Django Server setup with Apache with mod_wsgi in rocky linux 9

[root@localhost ~]# python --version
Python 3.9.18
root@localhost ~]# cat /etc/os-release
NAME="Rocky Linux"
VERSION="9.3 (Blue Onyx)"
ID="rocky"
ID_LIKE="rhel centos fedora"
VERSION_ID="9.3"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Rocky Linux 9.3 (Blue Onyx)"
ANSI_COLOR="0;32"
LOGO="fedora-logo-icon"
CPE_NAME="cpe:/o:rocky:rocky:9::baseos"
HOME_URL="https://rockylinux.org/"
BUG_REPORT_URL="https://bugs.rockylinux.org/"
SUPPORT_END="2032-05-31"
ROCKY_SUPPORT_PRODUCT="Rocky-Linux-9"
ROCKY_SUPPORT_PRODUCT_VERSION="9.3"
REDHAT_SUPPORT_PRODUCT="Rocky Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="9.3"
Adding the user
[root@localhost ~]# adduser ngpadmin
[root@localhost ~]# passwd ngpadmin
Changing password for user ngpadmin
New password:
Retype new password:
passwd: all authentication tokens updated successfully.

[root@server-01 ~]# visudo
[root@localhost ~]# vi /etc/selinux/config #disable selinux
[root@localhost ~]# sudo systemctl stop firewalld
[root@localhost ~]# sudo systemctl disable firewalld
[root@localhost djangoproject]# dnf update
[root@localhost ~]# ls -ls /usr/bin/python* & ls -ls /usr/local/bin/python*
[root@localhost ~]# alternatives --config python
[root@localhost ~]# python --version
[root@localhost ~]# dnf groupinstall development -y

root@localhost ~]# su ngpadmin
[ngpadmin@localhost ~]$ mkdir djangoproject
[ngpadmin@localhost ~]$ cd djangoproject/
[ngpadmin@localhost djangoproject]$ python -m venv virtenv
[ngpadmin@localhost djangoproject]$ ll
total 0
drwxr-xr-x. 5 ngpadmin ngpadmin 74 Dec 21 09:37 virtenv

[ngpadmin@localhost djangoproject]$ source virtenv/bin/activate
(virtenv) [ngpadmin@localhost djangoproject]$ 
(virtenv) [ngpadmin@localhost djangoproject]$ deactivate
[ngpadmin@localhost djangoproject]$ source virtenv/bin/activate
(virtenv) [ngpadmin@localhost djangoproject]$
(virtenv) [ngpadmin@localhost djangoproject]$ pip install django
(virtenv) [ngpadmin@localhost djangoproject]$  pip install --upgrade pip
[ngpadmin@localhost djangoproject]$ python3 --version
Python 3.9.18
(virtenv) [ngpadmin@localhost djangoproject]$ django-admin --version
4.2.8
(virtenv) [ngpadmin@localhost djangoproject]$ django-admin.py startproject mysite 
(virtenv) [ngpadmin@localhost djangoproject]$python -m django startproject mysite .
(virtenv) [ngpadmin@localhost djangoproject]$ python manage.py runserver 0.0.0.0:8000
[ngpadmin@localhost djangoproject]$ vi mysite/settings.py
#edit below line
import os
ALLOWED_HOSTS = ['*']
'DIRS': ['templates'],
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static')
]

STATIC_URL = '/static/'
MEDIA_URL = '/images/'

(virtenv) [ngpadmin@localhost djangoproject]$ mkdir templates
(virtenv) [ngpadmin@localhost djangoproject]$ mkdir static
(virtenv) [ngpadmin@localhost djangoproject]$ ll
total 132
-rw-r--r--. 1 ngpadmin ngpadmin 131072 Dec 20 14:14 db.sqlite3
drwxr-xr-x. 5 ngpadmin ngpadmin     77 Dec 20 13:57 virtenv
-rwxr-xr-x. 1 ngpadmin ngpadmin    662 Dec 20 14:04 manage.py
drwxr-xr-x. 3 ngpadmin ngpadmin    108 Dec 20 14:10 mysite
drwxr-xr-x. 2 ngpadmin ngpadmin      6 Dec 20 14:15 static
drwxr-xr-x. 2 ngpadmin ngpadmin      6 Dec 20 14:14 templates

(virtenv) [ngpadmin@localhost djangoproject]$ python manage.py migrate
(virtenv) [ngpadmin@localhost djangoproject]$ python manage.py makemigrations
(virtenv) [ngpadmin@localhost djangoproject]$ python manage.py createsuperuser
(virtenv) [ngpadmin@localhost djangoproject]$ python manage.py runserver 0.0.0.0:8000

http://192.168.1.123:8000/
http://192.168.1.123:8000/admin/
(virtenv)[ngpadmin@localhost djangoproject]$ python manage.py startapp test_app
 (virtenv)[ngpadmin@localhost djangoproject]$ vi mysite/settings.py
'test_app',

(virtenv)[ngpadmin@localhost djangoproject]$ vi mysite/urls.py 
from django.urls import include

urlpatterns = [
    path('admin/', admin.site.urls),
    path("", include("test_app.urls")),
]

(virtenv) [ngpadmin@localhost djangoproject]$ vi test_app/views.py
from django.shortcuts import render
from . import views
from django.http import HttpResponse
# Create your views here.

def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")

(virtenv)[ngpadmin@localhost djangoproject]$ vi test_app/urls.py
from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"),

]
(virtenv) [ngpadmin@localhost djangoproject]$ python manage.py runserver 0.0.0.0:8000

http://192.168.1.123:8000/
http://192.168.1.123:8000/admin/
[ngpadmin@localhost djangoproject]$sudo dnf install httpd 
[ngpadmin@localhost djangoproject]$sudo httpd -v
[ngpadmin@localhost djangoproject]$ sudo dnf -y install mod_wsgi
[ngpadmin@localhost djangoproject]$ sudo dnf install mod_wsgi httpd
[ngpadmin@localhost djangoproject]$ sudo systemctl enable httpd
[ngpadmin@localhost djangoproject]$ sudo systemctl start httpd
(virtenv)[ngpadmin@localhost djangoproject]$ vi test_app/views.py
from . import views
from django.http import HttpResponse
from django.shortcuts import render, redirect
# Create your views here.
def index(request):
   # return HttpResponse("Hello, world. You're at the polls index.")
    return render(request, 'index.html')

 (virtenv) [ngpadmin@localhost djangoproject]$ cd templates/
 (virtenv) [ngpadmin@localhost templates]$ vi index.html
 (virtenv)) [ngpadmin@localhost djangoproject]$ python manage.py runserver 0.0.0.0:8000

http://192.168.1.123:8000/
http://192.168.1.123:8000/admin/

[root@localhost ~]# ll -d /home/ngpadmin/
drwx------. 5 ngpadmin ngpadmin 111 Dec 20 15:41 /home/ngpadmin/
[root@localhost ~]# chmod 711 /home/ngpadmin
[root@localhost ~]# ll -d /home/ngpadmin/
drwx--x--x. 5 ngpadmin ngpadmin 111 Dec 20 15:41 /home/ngpadmin/

(virtenv) [ngpadmin@localhost mysite]$ sudo vi /etc/httpd/conf.d/django.conf


<VirtualHost *:80>

    DocumentRoot /home/ngpadmin/djangoproject/templates/

     Alias /static /home/ngpadmin/djangoproject/static
        <Directory /home/ngpadmin/djangoproject/static>
                Require all granted
        </Directory>

        Alias /static /home/ngpadmin/djangoproject/media
        <Directory /home/ngpadmin/djangoproject/media>
                Require all granted
        </Directory>



    <Directory /home/ngpadmin/djangoproject/mysite>
    <files wsgi.py>
        Require all granted
    </files>
    </Directory>


WSGIDaemonProcess mysite python-path=/home/ngpadmin/djangoproject:/home/ngpadmin/djangoproject/virtenv/lib/python3.9/site-packages

WSGIProcessGroup mysite
WSGIScriptAlias / /home/ngpadmin/djangoproject/mysite/wsgi.py

</VirtualHost>
(virtenv) [ngpadmin@localhost mysite]$ sudo systemctl restart httpd
(virtenv) [ngpadmin@localhost mysite]$ sudo systemctl status httpd
[ngpadmin@localhost djangoproject]$ vi mysite/settings.py 
"DIRS": [BASE_DIR / "templates"],
http://192.168.1.123

Internal Server Error
[root@localhost ~]# yum list '*mod_wsgi*'
Last metadata expiration check: 1:56:50 ago on Thu 21 Dec 2023 10:07:08 AM IST.
Installed Packages
python3-mod_wsgi.x86_64                   4.7.1-11.el9                @appstream
Available Packages
python3.11-mod_wsgi.x86_64                4.9.4-1.el9                 appstream 

