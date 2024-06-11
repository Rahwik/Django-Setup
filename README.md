# Django Setup and Tutorial

## 1. Set-Up:

1. Install Python in the system.
2. Make a local folder in the disk (e.g., Django).
3. Open the folder in the command prompt by typing `cmd` in the address bar.
4. Install virtual environment in the folder:
    ```sh
    pip install virtualenv
    ```
5. Create a virtual environment folder:
    ```sh
    virtualenv env
    ```
6. Activate the virtual environment by navigating to scripts:
    ```sh
    cd env
    cd scripts
    activate
    ```
7. Navigate back to the main folder:
    ```sh
    cd ..
    ```
8. Install Django in the virtual environment:
    ```sh
    pip install Django
    ```
9. Check if Django is successfully installed:
    ```py
    python
    import django
    django.__version__
    ```
    To exit, press `ctrl + Z`.

## 2. Project Creation In Django:

1. Create a new Django project:
    ```sh
    django-admin startproject core .
    ```
2. Navigate to the project folder:
    ```sh
    cd core
    ```
3. Open VS Code to view the project:
    ```sh
    code .
    ```

## 3. APP Creation In Django:

1. Create a new app:
    ```sh
    python manage.py startapp home
    ```

## 4. Run the Server:

1. Run the development server:
    ```sh
    python manage.py runserver
    ```
    To exit the server, press `CTRL + C`.
2. Run the server on a custom port:
    ```sh
    python manage.py runserver 0.0.0.0:5000
    ```

Note: Always add apps in the main directory in the `settings.py` in the "INSTALLED_APPS".

## 5. Views and URLs:

### A. HttpResponse (from the apps):

1. Open the `views.py` in the app created to add the attributes.
2. Import HttpResponse:
    ```py
    from django.http import HttpResponse
    ```
3. Create functions to add HTML content:
    ```py
    def home(request):
        return HttpResponse("<h1>Hey, this is a Django server</h1>")
    ```
4. In the main directory, open the `urls.py` to add the path of the view just created.
5. Import the views from the app:
    ```py
    from home.views import *
    ```
6. Add the path of the function:
    ```py
    urlpatterns = [
        path('', home, name="home"),
        path('success_page/', success_page, name="success_page"),
    ]
    ```

### B. HTML response (from the apps):

1. Create a templates folder in the app folder (e.g., core/home/templates).
2. Create `index.html` in the templates folder.
3. Create a render function for the `index.html`:
    ```py
    def home(request):
        return render(request, "index.html")
    ```

## 6. Template Engine:

### A. Dynamic Data Manipulation:

#### 1. Passing A List:

1. Create a list of dictionaries in the `views.py`:
    ```py
    peoples = [
        {'name': 'Rahul Prasad', 'age': 21},
        {'name': 'Rahul Prasad', 'age': 21},
        {'name': 'Rahul Prasad', 'age': 21}
    ]
    ```
2. Declare the list in the return render:
    ```py
    return render(request, "home/index.html", context={'people': peoples})
    ```
3. Display the data in the `index.html`:
    ```html
    <table>
        <tr>
            <th>S.no</th>
            <th>Name</th>
            <th>Age</th>
        </tr>
        {% for people in peoples %}
        <tr>
            <td>{{ forloop.counter }}</td>
            <td>{{ people.name }}</td>
            <td>{{ people.age }}</td>
        </tr>
        {% endfor %}
    </table>
    ```

#### 2. Using Dynamic Data for Other Operations (if...else):

```html
<table>
    <tr>
        <th>S.no</th>
        <th>Name</th>
        <th>Age</th>
        <th>Can vote</th>
    </tr>
    {% for people in peoples %}
    <tr>
        <td>{{ forloop.counter }}</td>
        <td>{{ people.name }}</td>
        <td>{{ people.age }}</td>
        <td {% if people.age >= 18 %} style="background:green;color:white;" {% else %} style="background:red;color:white;" {% endif %}>
            {% if people.age >= 18 %}
                👍
            {% else %}
                👎
            {% endif %}
        </td>
    </tr>
    {% endfor %}
</table>
```

### B. Static Data Manipulation:

### 1. Passing a Text String:

Create a text string in the views.py:

```python
text = """Rahul embodies a rare blend of resilience and empathy that leaves a lasting impression on everyone he meets.
 His unwavering determination and tireless work ethic are matched only by his genuine kindness and compassion towards others."""
```
2. Calling the text in the index.html
```python
{{text}}
```

3. Truncatechars:  filter truncate the value if it is longer than the specified number of characters
```python
{{ text|truncatechars:80}}
```
4. Working on the data and using it:


   a. Create a list in the views.py

    ```python
    Vegetables =[ "pumpkin","tomato","potato"]
    ```
   b. Add the new list to retrn render
   
    ```python
    return render(request,"home/index.html",context={'people' : peoples,'vegetables':vegetables})
     ```
   c. Use the data in the index.html for data usage:
   
   ```html
    <p>
    {% if "cucumber" in vegetables %}
    yes avaiable
    {% elif "tomato" in vegetables %}
    yes avaiable
    {% endid %}
    </p>
    ```
### C. Naviagting to different pages:

   1. Create different html files in the templates.
      '''sh
         eg. about.html , contact.html
      ```
   2. Add diferent functions to the views.py for thr newly created html files.
      ```python
         def about(request):
             return render(request, "about.html")
         def contact(request):
             return render(request, "contact.html")
      ```
   3. Add the new functions in the urls to integrate them together in the urls.py of urlpatterns.
      ```python
         urlspatterns =[
                        path('',home,name="home"),
                        path('about/',about,name="about"),
                        path('contact/',contact,name="contact"),
                       ]
      ```
   4. Add the different pages to the html file.
      ```html
         <a href="/contact/">
            contact
         |
         <a href="/About/">
            About
      ```
   5. Do similarly to all the html to make them inter linked

### D. Elimate the repeated code: 

  1. Create a base.html to add all the comman attributes .
  
  2. add the repeated attributes to the base.html
  
  3. the part which you don't want ot repeat add the follwing part
     ```html
         {% block start %}
         {% endblock %}
     ```
   4. change the html files accordingly, write the unquie code in the block:
      ```html
         {% extends "base.html" %}
         {% block start %}
          //Write your code here
         {% endblock %}
      ```
### E. Display name of the page at the top:
  1. Add the context in each page attributes.
     ```python
         def about(request):
             context={'page':'About'}
             return render(request, "about.html",context)
         def contact(request):
             context={'page':'Contact'}
             return render(request, "contact.html",context)
     ```
   2. Return the new dunctions value in te return render.
      ```python
         return render(request,"home/index.html",context={'page':'context','people' : peoples})
      ```
  3. add the returned file to the base.html
     ```python
         <title>{{page}}</title>
     ```

## 7.Models and Migration:
```python
   *Models in Django are Python classes that define the structure and behavior of your data.
   *Migrations are a way to propagate changes you make to your models (such as adding new fields or altering existing ones) into 
    your database schema.
```

 ### A. How to create models in Django
 
  1. Open the app[home,account,any other app] that we have created and naviagte to models.py 
  2. Create a class and call the library model in the class.
     ```python
         class Student(models.Model):
     ```
  3. Within the class define various fileds as the attributes of the class created.
     ```python
         name = models.Charfiled(max_length=100)
         age = models.IntegerField()
         email = models.Emailfiled()
         address = models.TextField()
         image = models.ImageField()
         file = models.FilesFiled()
     ```
  4. Similarly we can create other classes in the models.py and add attributes in them.
     ```python
         class Result(models.Model):
             pass
     ```
```python
NOTE: sg Django use DBSqlite we need to install it in our system to see the different schema we created in out DataBase.
```
   
### B. How to use and the models and thier properties

  1. All the changes and the migration files are stored in the dependencies of the 0001_intial.py and similarlies in the other dependdencies as we go on making chances .
  
  2. we can then make migration in the terminal and add them to the database.
     ```python
     python manage.py makemigrations
      ```
  3. We can check all the models in the terminal and if they are working properly or not.
     ```python
     python manage.py migrate
     ```
## 8. Django Shell :

  ### A. To go inside the python django shell.
   ```python
      python manage.py shell
   ```
  ### B. To leave the shell in terminal
   ```python
      exit
   ```
  ### C. Import the model in the Terminal
   ```python
      from home.models import *
  ```
  ### D. To input the data in the schema create in the models.py
   ```python
      students=Students(name="rahul",age=22email="rahul.3057.12@gamil.com,address="ranchi")
   ```
  ### E. Call the Schema in the terminal.
   ```sh
      student
   ```
  a. Save the changes and then call the Schema.
   ```sh
         1. students.save
   ```
  ### F. Other and more efficient way to input data in the Schema.
   ```python
      student=Student.objects.create(name="rahul",age=22email="rahul.3057.12@gamil.com,address="ranchi")
  ```
  ### G. To call the saved data from the database in the terminal
   
   ```sh
      1. Student.objects.all()[]
      2. Student.objects.all()[0].name {return the data at the 0 interval}
      3. Student.objects.all()[0].address
  ```
  ### H. Run function in the Shell
   
   1. make a new file in the app
      ```sh
         utils.py
      ```
   2. Import the models and create a function in the file
      ```python
         from .models import Student
          def run_this_function()
          print("function Started")
          print("function Started..")
          print("function Started....")
          time.sleep(3)
          print("Function Ended")
        ```
  3. In order to run any file in the Django server we cannot directly run the python module in the IDe, in our case we have to import thr function in th shell and then execute it. 
      ```python
         from home.utlis import *
         ```

   4. Then give the function you want to run from that file.
      ```python
         run_this_function()
      ```
## 9. Advanced CRUD Operations:  Cretae, Read, Update, delete

 ### A. Create a new Schema to work on for the advanced CRUD operations
  1.
     ```python
      class Car(models.Model):
          car_name = models.CharField(max_length=500)
          speed = models.Integerfield(default=50)
     ```
  2. Create a new migration to add the new schema in the database
     ```python
        python manage.py makemigrate 
        python manage.py migrate
     ```
  3. start the python shell to perform the CRUD operations
     ```python
        python manage.py shell
     ```
  4. Import the schema from models
     ```python
        from home.models import *
     ```
  5. Make the class objects aviable in the shell
     ```python
        car=Car()
        car.save()
        car
      ```
### B. Operations on the Models

  1. method 1 to add values to the schema.
      ```python
        car=car(car_name="ford",speed=110)
        car.save()
        car
      ```
  2. method 2 to add values to the schema.
       ```python
        car,objects.create(car_name="toyota",speed=200)
        car
       ```
  4. method 3 to add values to the schema.
        1. craete a dictionary and pass it to the class
           ```python
           car_dict = {"car_name": "maruti" , "speed":90}
           ```
        2. add the dictonary to the class
           ```python
           car.objects.create(**car_dict)
           ```
 ### C. Refining the Data input and optput

  1. Create a new function in the models.py
     ```python
          def __str__(self) -> str:
              return self.car_speed
     ```
 ### D. Create a loop out of the data inputs
 ```python
       for car in cars:
          print("the car {car.id} name is {car.car_name} with high speed of {car.speed}")
```

## 10. Master Django ORM:

### A.View count: Treack the number of post viewson a Django app.
  1. create a new model in the models.py
     ```python
        recipe_view_count = models.IntegerField(default=1)
     ```
  2. make migrations and migrate the new model to the database.
  3. open python shell in the powershell
  4. import the app in the shell to perform teh actions
    ```python
        from recipes.models import *
     ```
  5. check number of views created in the app
     ```python
        recipes = Recipe.objects.all()
        recipes
      ```
  7. import random in the shell
  8. set random value to the views using the random library
    ```python
        for rep in recipes:
            rep.recipe_view_count = random.randint(10,100)
            rep.save()
     ```
  9. see the views new random value.
     ```python
        recipes = Recipe.objects.all()
        recipes[0].recipe_view_count
        recipes[1].recipe_view_count
     ```

 ### B. Sort according to the views coount:
  1. to get sorted data we call order by
      ```python
      recipes = Recipe.objects.all().order_by('recipe_view_count')
       ```
   2. to get data in decesding order we just add a minus sign
      ```python
         vege = Receipe.objects.all().order_by('-recipe_view_count')
      ```
 ### C. to limit the number of records to be fetched:
  1. we just mention the range of the records to be fetched.
     ```python
        recipes = Recipe.objects.all().order_by('-recipe_view_count')[0:2]
     ```
 
 ### D. Filter the views count in Django:
  1. to get a specific view count nmber data value
     ```python
         Recipe.objects.filter(recipe_view_count = 55)
     ```
      b. to get all teh views count above or below the range (gte , lte)
     ```python
         Recipe.objects.filter(recipe_view_count_gte = 55)
                              OR
         Recipe.objects.filter(recipe_view_count_lte = 55)
     ```
## 11. Master Django ORM II:
     
### A. use of the admin panel in the django server
  1. create certain models in the models.py
     ```python
          class Department (models.Model):
                 department models.CharField(max_length=100)

                 def _str_(self) -> str:
                 return self.department
                 
                 class Meta:
                 ordering = ['department']


           class Student ID (models.Model):
                 student_id = models.CharField(max_length=100)

                 def _str_(self) -> str:
                 return self.student_id

          class Student (models.Model):
                department = models.ForeignKey(Department related_name="depart",on_delete=models.CASC
 
                student_id = models. OneToOneField (StudentID, related_name="studentid", on_delete-models
                student_name = models.CharField(max_length=100)
                student_email = models. EmailField(unique=True)
                student_age = models.IntegerField(default=18)
                student_address = models.TextField()

               def _str_(self) -> str:
               return self.student_name

               class Meta:
                     ordering = ['student_name']
                     verbose_name = "student"
     ```
   
### B. HOw to login the admin server:
  1. how to create the user for admin panel.
     ```python
         python manage.py createsuperuser
         Username (Leave blank to use 'pc'): rahul
         Email address:rahul.3057.12@gmail.com
         Password: 123
     ```
  2. Add the models to the admin.py
     ```python
         from django.contrib import admin
         # Register your models here.
         from .models import *
         admin.site.register (Recipe)
         admin.site.register(StudentID)
         admin.site.register(Student)
         admin.site.register(Department)
     ```

### C. Generating fake data for Django quries:
  1. using the faker library
  2. make a directory named seed.py in the app
  3. import the faker in the seed.py in the directory and generate the data
     ```python     
          from faker import Faker
          fake = Faker()
          import random
          from .models import *

          def seed_db(n=10) -> None:
              for in range(n):
                  departments_objs = IDepartment.objects.all()
                  random_index = random.randint(0, len (departments_objs)-1)
                  student_id == f'STU-0{random.randint (100, 999)}'
                  department = departments_objs [random_index]
                  student_name fake.name()
                  student_email = fake.email()
                  student_age = random.randint(20, 30)
                  student_address fake. address()

                  student_id_obj = Student ID.objects.create(student_id = student_id)

                  student_obj = Student.objects.create(
                  department =department,
                  student_id = student_id_obj,
                  student_name = student_name,
                  student_email =student_email,
                  student_age =student_age,
                  student_address =student_address,
                  )
     ```
 4. Create fake data in the admin panel by the shell in powershell
        ```python
           python manage.py shell
           from recipes.seed import *
           seed_db()m [Note: run this command as manay times as  manay fake data you need to create]
        ```
## 12. Advanced Quries About Django  And Working on the Querysets :
  ### A. Working on the data from the admin panel 
   1. Go to the python shell
      ```python
          python manage.py shell
      ```
   2. import the models in the shell
      ```python
          from recipes.models import *
      ```
  ### B. Filtering and Doing all sorts of data manipulation in the admin panel
   
   1.
   ```python
    queryset = Students.objects.filter(students_name__startswith == "rah")
    [note:using double underscores make it an  operator in django]
   ```
   2. printing the filtered data
   ```python
   queryset
   ```
   3.
   ```python
   queryset = Students.objects.filter(students_name__endswith == "sad")
   ```
   4.
   ```python
   queryset = Students.objects.filter(students_email__endswith == ".org")
   queryset
   ```
  5.
  ```python
  for q in queryset:
      print(q.student_mail)
   ```
  6. Use of icontains:
   ```python
   queryset = Student.objects.filter(student_name__icontains = "ra")
   queryset
   queryset[0].student_id
   queryset[0].department
  ```
  7. extracting the data from a particular views:
  ```python
  queryset Student.objects.filter(department__department = "Civil")
  [ The double underscore denotes the use of the foriegn key]
  ```
  8. use of teh icontains with the foriegn key :
  ```python
          queryset Student.objects.filter(department__department__icointains = "Civil")
  ```
  9.
  ```python
  count the derived querysets :
        queryset.count()
 ```
  10. extracting the data using the list .
  ```python
   d = ['Civil', 'Electrical'. 'Computer Science' ]
   queryset Student.objects.filter(department__department__in = d)
   queryset.count()
  ```
  11. using teh exclude keyword:
  ```python
  queryset Student.objects.exclude(department__department = "Civil")
  queryset
  ```
 ### C. Check if the data is been retrieved :
  1.
   ```python
      queryset = Student.objects.filter(student_name = "Rahul')
      len(queryset)
      [ note: if the set returned is zero than the data is not been retrieved ]
  ```
  2.
   ```python
   queryset.exists()
   ```
## 13. Aggregation and Annotataion:
     
 ### A. Aggregation in Django.
  1. works only for a single column .
 ### B. Annotations in Django :
  2. when we work on multiple columns .
 ### C. Using average operation with the aggregate function
  ```python
      from django.db.models import *
      from recipes.models import *
      Student.objects.aggregate(Avg('student_age'))
 ```
 ###  D. Using Max operation with the aggregate function 
 ```python
      from django.db.models import *
      from recipes.models import *
      Student.objects.aggregate(Max('student_age'))
```
 ### E. Using Min operation with the aggregate function 
 ```python
      from django.db.models import *
      from recipes.models import *
      Student.objects.aggregate(Min('student_age'))
```
 ### F. Using Sum operation with the aggregate function 
 ```python
      from django.db.models import *
      from recipes.models import *
      Student.objects.aggregate(Sum('student_age'))
```
 ### G. Using the annotation in the Dango 
 ```python
      from django.db.models import *
      from recipes.models import *
      Student.objects.values('student_age').annotate(Count('student_age'))
```
        







      
      
     
     
     
   



