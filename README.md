# Домашнее задание к лекции «Объекты и классы. Инкапсуляция, наследование и полиморфизм»
​
Привет! Домашняя работа по данной теме является продолжением квиза к предыдущей лекции. Мы продолжим реализовывать логику функционала учебной группы в парадигме ООП.
Удачи!

## Задание № 1. Наследование
Исходя из квиза к предыдущему занятию, у нас уже есть [класс преподавателей и класс студентов](https://github.com/netology-code/py-homeworks-basic/blob/new_oop/6.classes/students_and_mentor.py) (вы можете взять этот код за основу или написать свой). Студентов пока оставим без изменения, а вот преподаватели бывают разные, поэтому теперь класс Mentor должен стать родительским классом, а от него нужно реализовать наследование классов Lecturer (лекторы) и Reviewer (эксперты, проверяющие домашние задания). Очевидно, имя, фамилия и список закрепленных курсов логично реализовать на уровне родительского класса. А чем же будут специфичны дочерние классы? Об этом в следующих заданиях.


## Решение № 1. Наследование

```
lass Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)


class Reviewer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)


best_student = Student('Ruoy', 'Eman', 'your_gender')
best_student.courses_in_progress += ['Python']
cool_mentor = Mentor('Some', 'Buddy')
cool_mentor.courses_attached += ['Python']
cool_mentor.rate_hw(best_student, 'Python', 10)
cool_mentor.rate_hw(best_student, 'Python', 10)
cool_mentor.rate_hw(best_student, 'Python', 10)
print(best_student.grades)
```

## Задание № 2. Атрибуты и взаимодействие классов.
В квизе к предыдущей лекции мы реализовали возможность выставлять студентам оценки за домашние задания. Теперь это могут делать только Reviewer (реализуйте такой метод)! А что могут делать лекторы? Получать оценки за лекции от студентов :) Реализуйте метод выставления оценок лекторам у класса Student (оценки по 10-балльной шкале, хранятся в атрибуте-словаре у Lecturer, в котором ключи – названия курсов, а значения – списки оценок). Лектор при этом должен быть закреплен за тем курсом, на который записан студент. 

## Решение № 2. Атрибуты и взаимодействие классов.


```
class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer,
                      Lecturer) and course in lecturer.courses_attached and course in self.courses_in_progress:
            if course in lecturer.grades:
                lecturer.grades[course] += [grade]
            else:
                lecturer.grades[course] = [grade]
        else:
            return 'Ошибка'


class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}


class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'


best_student = Student('Ruoy', 'Eman', 'your_gender')
best_student.courses_in_progress += ['Python']

cool_reviewer = Reviewer('Some', 'Buddy')
cool_reviewer.courses_attached += ['Python']

cool_reviewer.rate_hw(best_student, 'Python', 10)

cool_lecturer = Lecturer('John', 'Doe')
cool_lecturer.courses_attached += ['Python']

best_student.rate_lecturer(cool_lecturer, 'Python', 9)

print(best_student.grades)
print(cool_lecturer.grades)
```



## Задание № 3. Полиморфизм и магические методы
1. Перегрузите магический метод ```__str__``` у всех классов. 

У проверяющих он должен выводить информацию в следующем виде:
```python
print(some_reviewer)
Имя: Some
Фамилия: Buddy
```

У лекторов:
```python
print(some_lecturer)
Имя: Some
Фамилия: Buddy
Средняя оценка за лекции: 9.9
```

А у студентов так:
```python
print(some_student)
Имя: Ruoy
Фамилия: Eman
Средняя оценка за домашние задания: 9.9
Курсы в процессе изучения: Python, Git
Завершенные курсы: Введение в программирование
```

2. Реализуйте возможность сравнивать (через операторы сравнения) между собой лекторов по средней оценке за лекции и студентов по средней оценке за домашние задания.


## Решение № 3. Полиморфизм и магические методы

```
class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []


class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

    def __str__(self):
        return f"Имя: {self.name}\nФамилия: {self.surname}"


class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

    def rate_lecture(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in self.grades:
                self.grades[course] += [grade]
            else:
                self.grades[course] = [grade]

    def average_grade(self):
        total_grades = [grade for grades in self.grades.values() for grade in grades]
        return sum(total_grades) / len(total_grades) if total_grades else 0

    def __str__(self):
        return f"Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {self.average_grade():.1f}"


class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def average_grade(self):
        total_grades = [grade for grades in self.grades.values() for grade in grades]
        return sum(total_grades) / len(total_grades) if total_grades else 0

    def __str__(self):
        return (f"Имя: {self.name}\nФамилия: {self.surname}\n"
                f"Средняя оценка за домашние задания: {self.average_grade():.1f}\n"
                f"Курсы в процессе изучения: {', '.join(self.courses_in_progress)}\n"
                f"Завершенные курсы: {', '.join(self.finished_courses)}")

    def __lt__(self, other):
        if isinstance(other, Student):
            return self.average_grade() < other.average_grade()
        return NotImplemented

    def __le__(self, other):
        if isinstance(other, Student):
            return self.average_grade() <= other.average_grade()
        return NotImplemented

    def __gt__(self, other):
        if isinstance(other, Student):
            return self.average_grade() > other.average_grade()
        return NotImplemented

    def __ge__(self, other):
        if isinstance(other, Student):
            return self.average_grade() >= other.average_grade()
        return NotImplemented

    def __eq__(self, other):
        if isinstance(other, Student):
            return self.average_grade() == other.average_grade()
        return NotImplemented


# Пример использования
best_student = Student('Ruoy', 'Eman', 'your_gender')
best_student.courses_in_progress += ['Python']

cool_reviewer = Reviewer('Some', 'Buddy')
cool_reviewer.courses_attached += ['Python']

cool_lecturer = Lecturer('John', 'Doe')
cool_lecturer.courses_attached += ['Python']

cool_reviewer.rate_hw(best_student, 'Python', 10)
cool_reviewer.rate_hw(best_student, 'Python', 9)

cool_lecturer.rate_lecture(best_student, 'Python', 10)
cool_lecturer.rate_lecture(best_student, 'Python', 8)

print(best_student)
print(cool_reviewer)
print(cool_lecturer)
```


## Задание № 4. Полевые испытания
Создайте по 2 экземпляра каждого класса, вызовите все созданные методы, а также реализуйте две функции:
1. для подсчета средней оценки за домашние задания по всем студентам в рамках конкретного курса (в качестве аргументов принимаем список студентов и название курса);
2. для подсчета средней оценки за лекции всех лекторов в рамках курса (в качестве аргумента принимаем список лекторов и название курса).


## Решение № 4. Полевые испытания

```
class Mentor:
    def __init__(self, name, surname):
        self.name = name
        self.surname = surname
        self.courses_attached = []

    def __str__(self):
        return f'Имя: {self.name}\nФамилия: {self.surname}'

class Reviewer(Mentor):
    def rate_hw(self, student, course, grade):
        if isinstance(student, Student) and course in self.courses_attached and course in student.courses_in_progress:
            if course in student.grades:
                student.grades[course] += [grade]
            else:
                student.grades[course] = [grade]
        else:
            return 'Ошибка'

class Lecturer(Mentor):
    def __init__(self, name, surname):
        super().__init__(name, surname)
        self.grades = {}

    def __str__(self):
        avg_grade = self.get_average_grade()
        return f'Имя: {self.name}\nФамилия: {self.surname}\nСредняя оценка за лекции: {avg_grade}'

    def get_average_grade(self):
        if self.grades:
            return sum([sum(grades) for grades in self.grades.values()]) / sum([len(grades) for grades in self.grades.values()])
        return 0

class Student:
    def __init__(self, name, surname, gender):
        self.name = name
        self.surname = surname
        self.gender = gender
        self.finished_courses = []
        self.courses_in_progress = []
        self.grades = {}

    def __str__(self):
        avg_grade = self.get_average_grade()
        return (f'Имя: {self.name}\nФамилия: {self.surname}\n'
                f'Средняя оценка за домашние задания: {avg_grade}\n'
                f'Курсы в процессе изучения: {", ".join(self.courses_in_progress)}\n'
                f'Завершенные курсы: {", ".join(self.finished_courses)}')

    def get_average_grade(self):
        if self.grades:
            return sum([sum(grades) for grades in self.grades.values()]) / sum([len(grades) for grades in self.grades.values()])
        return 0

    def rate_lecturer(self, lecturer, course, grade):
        if isinstance(lecturer, Lecturer) and course in lecturer.courses_attached and course in self.courses_in_progress:
            if course in lecturer.grades:
                lecturer.grades[course] += [grade]
            else:
                lecturer.grades[course] = [grade]
        else:
            return 'Ошибка'

# Создание экземпляров
student_1 = Student('Ruoy', 'Eman', 'your_gender')
student_1.courses_in_progress += ['Python']
student_1.finished_courses += ['Введение в программирование']

student_2 = Student('John', 'Doe', 'your_gender')
student_2.courses_in_progress += ['Python']
student_2.finished_courses += ['Введение в программирование']

reviewer_1 = Reviewer('Some', 'Buddy')
reviewer_1.courses_attached += ['Python']

reviewer_2 = Reviewer('Alice', 'Smith')
reviewer_2.courses_attached += ['Python']

lecturer_1 = Lecturer('Mark', 'Twain')
lecturer_1.courses_attached += ['Python']

lecturer_2 = Lecturer('Jane', 'Austen')
lecturer_2.courses_attached += ['Python']

# Вызов методов
reviewer_1.rate_hw(student_1, 'Python', 10)
reviewer_1.rate_hw(student_2, 'Python', 8)

student_1.rate_lecturer(lecturer_1, 'Python', 9)
student_2.rate_lecturer(lecturer_1, 'Python', 10)
student_1.rate_lecturer(lecturer_2, 'Python', 8)

# Вывод информации
print(reviewer_1)
print(lecturer_1)
print(student_1)

# Функции для подсчета средней оценки
def average_hw_grade(students, course):
    total_grade = 0
    count = 0
    for student in students:
        if course in student.grades:
            total_grade += sum(student.grades[course])
            count += len(student.grades[course])
    return total_grade / count if count > 0 else 0

def average_lecture_grade(lecturers, course):
    total_grade = 0
    count = 0
    for lecturer in lecturers:
        if course in lecturer.grades:
            total_grade += sum(lecturer.grades[course])
            count += len(lecturer.grades[course])
    return total_grade / count if count > 0 else 0

# Пример использования функций
print(average_hw_grade([student_1, student_2], 'Python'))
print(average_lecture_grade([lecturer_1, lecturer_2], 'Python'))
```

