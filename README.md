# ASSIGNMENT 1:
![alt text](image.png)
![alt text](image-1.png)

1. app/urls.py

urlpatterns = [
    path('students/', student_list, name='student-list'),
]

2. app/views.py

from .models import Student

@api_view(['GET'])
def student_list(request):
    students = Student.objects.all()

    data = []
    for student in students:
        data.append({
            "id": student.id,
            "name": student.name,
            "age": student.age,
            "course": student.course
        })

    return Response(data)

3. app/models.py

from django.db import models

# Create your models here.
class Student(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()
    course = models.CharField(max_length=100)
    
    def __str__(self):
        return self.name

