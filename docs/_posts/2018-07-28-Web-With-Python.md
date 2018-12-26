---
title: "Web Development With Python"
excerpt_separator: "<!--more-->"
excerpt: "School grading Python web application using Django. Source code, code documentation and live-demo included with brief information on Django structure."
description: "School grading web application using Django"
date: "2018-07-28"
header:
    og_image: /assets/images/simplicity3.png    
categories:
  - Projects
tags:
  - Python
  - Django
  - Visual Studio Code
  - Web
  - Docker
---

{% include toc title="Contents" icon="list-ul" %}

## Introduction

I have been checking alternative frameworks to Microsoft's .NET for web development. There are several decent frameworks but since I intend to spend some time on Machine Learning too, I prefered using Python as a language.  There are two available popular options: [Flask](http://flask.pocoo.org/), a micro framework and [Django](https://www.djangoproject.com/), an all-inclusive framework. I chose Django mostly due to ORM.

![Python Powered]({{ site.url }}{{ site.baseurl }}/assets/images/pythonpoweredalt.png){:height="68px" width="165px"}
![Django]({{ site.url }}{{ site.baseurl }}/assets/images/django-logo-negative.png){:height="68px" width="150px"}

This quote is taken from Django project website::

Django is a high-level Python Web framework that encourages rapid development and clean, pragmatic design. Built by experienced developers, it takes care of much of the hassle of Web development, so you can focus on writing your app without needing to reinvent the wheel. It’s free and open source.
{: .notice}

Well known companies like Instagram, Mozilla, National Geographic and Pinterest use Django for their web projects.

## Goals

1. Using Python, develop a working web application with enough features that could be an early Alpha of a product
2. Works on all platforms. Both development and deployment.
3. Dockerize & Host
4. Document & Share

## Restrictions

* Only use free & cross platform software.
* Do not bloat the code with third party libraries. And keep it simple. (ie: no javascript libraries, everything in line with standard way of doing things)
* Project should be expandable.

## Prerequisites

* Basic knowledge of Python programming
* Basic knowledge of relational databases
* Basic knowledge of web programming
* Using Bash

## What This Project Does

The project is a basic web application that can be used as a grading management system for students.

1. Manager: Manager handles buerocratic stuff such as defining Schools and defining Class Rooms.
2. Lecturer: Defines Courses and gives Grade scores to enrolling students.
3. Student: Can Enroll to Courses and check their grade scores
4. Admin: Standard Django Admin rights

Here is a summary:
![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesaccessrights.png){:height="300px" width="680px"}

## Usage with Screenshots

Scenario: 

1. A manager, Two Lecturers and Two Students all signup to the system.
2. Manager creates three schools and two classrooms for each school.
3. Each Lecturer creates two courses.
4. Both students enroll to all courses.
5. Lecturers give grades to users.
6. Students view the results.

* Home Page

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradeshome.png){:height="360px" width="576px"}

[Click here to zoom image]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradeshome.png){:target="_blank"}

* Signup Page

A total of 5 users signup. 1 Manager, 2 Lecturers, 2 Students. 

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradessignup.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradessignup.png){:target="_blank"}

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradessignup2.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradessignup2.png){:target="_blank"}


Ps. This is for test purposes. In a production scenario there needs to be an approval system in place.

This is how users list looks like

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesuserslist.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesuserslist.png){:target="_blank"}

* Manager logs in

After signups, Manager logs in to the system.

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradeslogin.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradeslogin.png){:target="_blank"}


* Manager adds Schools

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesschools.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesschools.png){:target="_blank"}

* Manager adds Class Rooms

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesclassrooms.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesclassrooms.png){:target="_blank"}

* Lecturers add Courses

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescreatecourse.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescreatecourse.png){:target="_blank"}

Below is how it looks like after both lecturers add two courses each. Notice how the logged in lecturer can only modify / delete his / her own courses and can only view students of other courses.

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescourselist.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescourselist.png){:target="_blank"}

* Students enroll to courses

Students login to the system and can view available courses. Once they click enroll, there is no turning back (by design) and the course is moved from "Courses" page into "Taken Courses" page. 

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescoursenroll.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescoursenroll.png){:target="_blank"}

The student enrolled all 4 courses and they are moved into takencourses. The lecturers have not graded these students yet.

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradestakencourselist.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradestakencourselist.png){:target="_blank"}

* Lecturers grade courses

Lecturers go to Course List and click Grade. From there;

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesgivegrades.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesgivegrades.png){:target="_blank"}

* Finally students check their grades

If the grade is lower than 50 points, then the student has failed the course.

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescheckgrades.png){:height="360px" width="576px"}

[Click here to zoom images]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradescheckgrades.png){:target="_blank"}

Lecturers can change the grades at anytime.

## Structure

Django applications are MVC(Model-View-Controller) applications, or more accurately MVT (Model-View-Template). URL dispatcher dispatches the user request to appropriate Views. Views handle user requests and business logic using Models to interact with the database and Templates to generate Html to return to the user. MVT is a little different than MVC but the the intend is the same.

![MVT]({{ site.url }}{{ site.baseurl }}/assets/images/grades/mvtarch.png)

A Django project's root is "Project" that can hold multiple "Applications". A project is a collection of configuration and apps for a particular website. A project can contain multiple apps. An app can be in multiple projects.

This is the file structure of the project root directory.

![Root]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesroottree.png){:height="265px" width="187px"}

* ```urls.py``` This is the entry point of all requests to our website. Inside this file, we can redirect all requests to appropriate sub-applications. Since we have only one application, We redirect all requests to the Grades and all requests to the root to Grades application's dispatcher.

```python

urlpatterns += [
    path('grades/', include('Grades.urls')),
    path('', include('Grades.urls'))
]
```

* ```settings.py``` Almost every global setting is done inside this file. Installed applications, Database connections, Authorization settings, Secret key etc.

* ```wsgi.py``` and ```manage.py``` The application object and the execution point. You don't need to touch these two. Manage.py helps with creating database, tables and running the application.

This is the file structure of the Grades application.

![Root]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradestree.png){:height="466px" width="200px"}

You can see Model, View files and Template folder here. I organised the Template folder so that each folder has its related html files.

### Routing

Django requests are first directed to urls.py at the project root, and in there the requests are bounced to Grades/urls.py. So all routing that counts is handled inside ```Grades/urls.py``` file. For readiblity, here I have seperated the urls by role. Students, Managers, Lecturers and Common. Here the requests are dispatched into views (with parameters if any)

For instance, students can click Enroll a course or view their Taken courses as well as access shared pages. When a student enrolls a course, the course ID is passed as a primary key into "Enroll" view.

```python

#Students
urlpatterns += [
    path('enroll/<int:pk>/', views.Enroll, name='enroll'),
    path('takencourses/', views.TakenCoursesListView.as_view(), name='takencourses'),
]

```

### Models

Django framework offers default User & Group tables, Authentication and Authorization by design. You can just use them and happily live after. But I have decided to override default User table to play around. I added a user type variable to it so I can control user role access rights inside the code simply by checking the type. I derived my CustomUser class from AbstractUser, so it inherited all the fields from the parent class (username, first_name, etc. [Here is the official list](https://docs.djangoproject.com/en/2.0/ref/contrib/auth/)).

Django models' unique ID fields are hidden and defined by default. So you don't need to explicitly define them. 

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradesentity.png){:height="414px" width="512px"}

A Course can have multiple students. And a student can take multiple courses. So there is a many-to-many relationship here. Django ORM creates intermediate M2M tables for you when you simply define a field as many to many. You don't need to include them in your code (much like not having to define unique IDs). 

Check the line "students = models.ManyToManyField" on the Model code.

```python
class Course(models.Model):
    '''
    Course model
    '''
    name = models.CharField(max_length=50, help_text='Enter Course Name')
    code = models.CharField(max_length=10, help_text='Enter Course Code')    
    classRoom = models.ForeignKey('ClassRoom', on_delete=models.SET_NULL, null=True, help_text='Select Class Room')
    lecturer = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    students = models.ManyToManyField(settings.AUTH_USER_MODEL, related_name='Students', help_text='Students')

    def __str__(self):
        return "{0} ({1})".format(self.name, self.code)
```

This is the helper table actually created on the database mapping the Courses with Students (which are actually users):

![Access Rights]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradeshelpertable.png){:height="403px" width="365px"}

### Views

Almost everything happens inside ```Grades/views.py``` file. Views take the requests from URL router, executes the business logic while using Models and selects appropriate templates & forms, passes processed data into templates and returns them to the user. The views can be in Class form or in method (ie: Enroll method in views) form. Below is a class based view example;


```python
class SchoolCreateView(LoginRequiredMixin, UserPassesTestMixin, CreateView):
    '''
    School Create
    '''
    model = School
    form_class = SchoolCreateForm
    template_name = 'Manager/school.html'
    success_url = reverse_lazy('schoollist')
    raise_exception = True

    def test_func(self):
        print('SchoolCreatView UserType:{0}'.format(self.request.user.user_type))
        if self.request.user.user_type != 3:
            print('User type mismatch, test_func denied')
            return False
        return True

    def get_login_url(self):
        if not self.request.user.is_authenticated:
            print('not authenticated')
            return super(SchoolCreateView, self).get_login_url()
        return ''

    def get_context_data(self, **kwargs):
        context = super(SchoolCreateView, self).get_context_data(**kwargs)
        context['title'] = "Add School"
        return context  
```

This view is executed when the user would like to create a new School. It is derived from three classes

* LoginRequiredMixin: Prevents anonymous navigation. Only authenticated users can access. Otherwise the user is redirected to Login page.
* UserPassesTestMixin: If "LoginRequiredMixin" is Authentication, "UserPassesTestMixin" is Authorization for our business logic. This declares that aside from an authenticated user, there are certain conditions that must be fulfilled to navigate to this view. This is done by test_func method. You check the conditions (whatever they may be) inside this method and simply either return True or False. True means user can navigate, False means he/she cannot. Member variable raise_exception is set to True so if the user does not meet the criteria, "Forbidden" message is displayed to the user. Logically speaking, Only a Manager can create a new school. Here we check if the user is a manager or not.
* CreateView: This tells the framework that we intend to create a new object in the database within this view. And the Model is School as declared in member variables. And the form is SchoolCreateForm which is defined inside forms.py file and the template to use this form is school.html.

* The method get_context_data is where we can pass additional data to the template. I have decided to generate the titles of the templates from within the View instead of hard coding them into html's. This has some uses as sometimes I used a single template for multiple views. For instance both SchoolCreateView & SchoolUpdateView uses school.html template and I set the title from within the view.

If you check school.html file you can see the title is a variable taken from the view: { { title } }

### Templates

Templates are Html files with special Django syntax enabled. Both the Django framework and the Views calling them are responsible to build actual Html files from these. The following are all the templates in the project.

![Templates]({{ site.url }}{{ site.baseurl }}/assets/images/grades/gradestemplates.png){:height="533px" width="244px"}

I put them under their respective folders. For instance only Lecturers are allowed to manage Courses, and the related template file is course.html.

### Static files

Static files such as .css, images, videos etc. are placed under /Static folder. For more efficient production deployment, you may want to host static files in a seperate hosting provider and refer to those from within templates.

### Unit Tests

Django has an extensive automated testing mechanism which you should implement inside tests.py file. But this is out of my scope for now. You can read [official documentation](https://docs.djangoproject.com/en/2.0/topics/testing/)

## Running the code

Since there are no external dependencies or other third parties. Everything is inline with running a standard Django application. There are no additional steps other than most basic, official ones. They are listed below anyway (Classic Method).

On the other hand, if you want to run the application with docker, I have prepared necessery files so everything is much much simpler. Literally all you need to do is Install [Docker](https://www.docker.com/community-edition) and run a single line of code at the project directory; ```docker-compose up```. You don't even need to install any development stuff such as Python, Django, Postgres, Web Server or Virtualenv on your host machine. All of them are going to be Dockerized for you.

### Classic Method

Steps:

1. Optional: Install [Virtualenv](https://virtualenv.pypa.io/en/stable/) if you would like to isolate your  development environment from your host OS and to happily sandbox with different versions. If you install virtualenv, you can and should install everything else inside it.

2. Install [Python](https://www.python.org/downloads/) and [Django](https://docs.djangoproject.com/en/2.0/topics/install/) if you haven't already. (I used ```homebrew```). The project was working fine with Python 3.7 and Django 2.0 versions.

3. Install the Django supported database of your choice. [Here is a list](https://docs.djangoproject.com/en/2.0/ref/databases/)

4. Get the [Source on Github](https://github.com/ayhanavci/LainSimpleGrades), edit ```/school/settings.py```. 

    4.a. Set the database. There are three options that I put in there, Sqlite, Local Postgre, Docker Postgre all of which I have used during development. Just uncomment the connection of your choice and edit the parameters. You can naturally use [any Database supported by Django]((https://docs.djangoproject.com/en/2.0/ref/databases/))

    For simplicity, I recommend using Sqlite. Just uncomment the top option and comment the other two.

    ```python
    DATABASES = {
        'default': {
            'ENGINE': 'django.db.backends.sqlite3',
            'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
        }
    }
    ```

    4.b. You may want to edit the following parameters in ```/school/settings.py``` for production deployment.

    ```python
    # SECURITY WARNING: keep the secret key used in production secret!
    SECRET_KEY = 's$0m8kpg7(syqnff9kwdc39w48_4ba__$c!p5_$(b()@2ys*g5'

    # SECURITY WARNING: don't run with debug turned on in production!
    DEBUG = True
    ```

5. Change directory into project root and run bash commands to create database using python (or use ```python3``` depending on your installation)

```bash
python manage.py makemigrations Grades
python manage.py migrate
python manage.py runserver
```

That's it. The application should work on http://127.0.0.1:8001/. If you would like to add an administrator account, run ```python manage.py createsuperuser``` before ```runserver``` but after ```migrate```, and follow instructions.

(You can change the port like this: ```python manage.py runserver 7000```)

### With Docker

1. Download and extract zip from [here](https://github.com/ayhanavci/LainSimpleGrades/archive/master.zip) or from Bash

```bash
git clone https://github.com/ayhanavci/LainSimpleGrades.git
```

2. Enter directory and docker-compose up (-d parameter to run in background)

```bash
cd LainSimpleGrades
docker-compose up
```

That's it. This command does all of the following in order;

Everything below 4 happens inside docker containers, so nothing gets installed on your host machine. Everything is isolated.

1. Downloads Official Python 3.7 Docker Image
2. Downloads Official Postgre Sql Docker Image
3. Creates docker images on your host (Python, Postgre and lainsimplegrades)
4. Creates docker containers from images
5. Binds the containers
6. Runs Postgre SQL
7. Waits for Postgres to be up and running, and then creates database and tables and hosts the web application.
8. Runs the web server inside lainsimplegrades container on port 8000 and binds it to your host's port 8001

The application should work at http://127.0.0.1:8001/. 

Before docker-compose;

* You may want to change settings in ```/school/settings.py``` as described above.
* If you change database settings in ```/school/settings.py```, you have to edit ```docker-compose.yml``` to match the values.
* If you want to change the port, edit ```docker-compose.yml```.

## Source Code

[Source on Github](https://github.com/ayhanavci/LainSimpleGrades)

Other than a few lines to make the menu responsive and a small function to bind two dropdowns (school & related class room dropdowns on course creation page) there is no Javascript code in the project. This was by design.

## Live Demo

Currently I am hosting the application on a Dockerized container on Ubuntu. You can access the live application at
[http://grades.lain.run/](http://grades.lain.run/). I have not yet installed the SSL certificate. But it is hardly needed as this is just a sandbox. The website is fully functional. You can Signup with new users (Don't forget to select a role during signup) and can use every feature described. However, I do not promise to keep this live demo up and running all the time since I am hosting it on my personal sandbox Linux.

I have explained how I Dockerized my Python web application on Linux on a seperate post here: [Dockerizing Python on Linux]({% post_url 2018-07-29-Dockerizing-Python-on-Linux %})

## Conclusion

I consider my goals achieved. Coded, Dockerized and shipped a Python web application with Simplicity and Unix philosophy in mind. Django does its job well, it is fast, flexible and employs a very popular language. I encourage you to use it for your web projects. Also I consider the following to be good alternatives; Ruby on Rails, .NET Core and Phoenix and Flask.

## License

This project is licensed under the [GNU Public License](https://www.gnu.org/licenses/#GPL)

## Software Used

For Development;

* [Visual Studio Code](https://code.visualstudio.com/) (and free addons) 
* [Python](https://www.python.org/) 
* [Django](https://www.djangoproject.com/) 
* [Sqlite](https://www.sqlite.org/index.html)
* [VirtualEnv](https://virtualenv.pypa.io/)

For Deployment;

* [PostgreSQL](https://www.postgresql.org/)
* [Docker Community Edition](https://www.docker.com/community-edition)
* [Ubuntu](https://www.ubuntu.com/)
* [NGinx](https://www.nginx.com/)

Front end cosmetics:

* [Font Awesome](https://fontawesome.com/)
* [W3.css](https://www.w3schools.com/w3css/)
