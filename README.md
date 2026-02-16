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

# ASSIGNMENT 2:
![alt text](image-2.png)

1. app/views.py
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status
from .models import Student
from .serializers import StudentSerializer  

@api_view(['GET', 'POST'])
def student_list(request):

    if request.method == 'GET':
        students = Student.objects.all()
        serializer = StudentSerializer(students, many=True)  # Serialize all students
        return Response(serializer.data)  # serializer.data is already in correct format

    if request.method == 'POST':
        serializer = StudentSerializer(data=request.data)  # Pass request data to serializer
        
        if serializer.is_valid():
            serializer.save()  
            return Response(serializer.data, status=status.HTTP_201_CREATED)
        else:
            return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)

2. app/serailzier.py

from rest_framework import serializers
from .models import Student

class StudentSerializer(serializers.ModelSerializer):
    class Meta:
        model = Student
        fields = ['id', 'name', 'age', 'course']

