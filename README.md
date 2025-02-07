# AISchool
First AI driven digital platform to simplify learning in school

# Project Structure
## Frontend
### Stack:
- Angular

### Content
- Registration (using OAuth or email and password, also fields: name, surname, teacher / student, school, grade)
- Personal Cabinet (class stats, personal stats, pending tasks, homework)
- Diary (timetable, notes, homework)
- Subjects for classes (yaklass like)
- Lists of modules for each subject (yaklass like)
- Theory and Practice part in every module (yaklass like, in every theory built in prompt for chat gpt / deepseek, every task should be marked if its checked using chatgpt or just code)


## Backend
### Stack:
- Java

### Content
- Auth service (oauth, jwt tokens)
- Diary service (rest, crud for notes, homework, make autoimport from studii.md)
- Main monolith (theory pages, code to generate tasks (maybe implemented in frontend), api for chatgpt, no mutations, all changes are only rating pluses, made only on server)

## DB (basic tables & fields)
## (Auth)
### Account
constraints: email - unique
 - id: uint (primary key)
 - email: varchar(64)
 - password: varchar(64)
 - name: varchar(16)
 - surname: varchar(16)
## (Diary)
### Media
 - id: uint (primary key)
 - file: url
### School
 - id: uint (primary key)
 - name: varchar(32)
 - director: ForeignKey(Teacher)
### Class
constraints: form && letter - unique together
 - id: uint (primary key)
 - form: smallint
 - letter: char
 - teacher: ForeignKey(Teacher)
 - school: ForeignKey(School)
 - timetable: OneToOneField(TimeTable)
### Subject
 - id: uint (primary key)
 - name: verchar(32)
 - grade: smallint
### TimeTable
 - id: uint (primary key)
 - class: ForeignKey(Class)
 - lessons: ManyToManyField(Subject, through=Lesson)
### Lesson
constraints: starting && weekday - unique together
 - id: uint (primary key)
 - timetable: ForeignKey(TimeTable)
 - subject: ForeignKey(Subject)
 - starting: datetime
 - ending: datetime
 - weekday: varchar(2, choices=('MO', 'TU', 'WE', 'TH', 'FR', 'SA', 'SU'))
 - teacher: ForeignKey(Teacher)
### SpecificLesson
 - id: uint (primary key)
 - lesson: ForeignKey(Lesson)
 - date: datetime
 - homework: text
 - media: ManyToManyField(Media)
 - notes: ManyToManyField(Student, through=Notes)
 - uploaded_homework: ManyToManyField(Student, through=HomeWork)
### Notes
 - id: uint (primary key)
 - note: smallint
 - student: ForeignKey(Student)
 - comment: varchar(256)
 - timestamp: datetime
### Homework
 - id: uint (primary key)
 - specific_lesson: ForeignKey(SpecificLesson)
 - student: ForeignKey(Student)
 - comment: varchar(256)
 - media: ManyToManyField(Media)
### Teacher
 - id: uint (primary key)
 - account: OneToOneField(Account)
 - schools: ManyToManyField(School)
 - subjects: ManyToManyField(Subject)
### Student
 - id: uint (primary key)
 - account: OneToOneField(Account)
 - class: ForeignKey(Class)
## (Main)
### Module
 - id: uint (primary key)
 - subject: ForeignKey(Subject)
 - name: varchar(128)
### Topic
 - id: uint (primary key)
 - module: ForeignKey(Module)
 - name: varchar(128)
### Theory
 - id: uint (primary key)
 - topic: ForeignKey(Topic)
 - name: varchar(64)
 - file: url (every theory is stored in server as html, which is parsed in frontend)
 - stars: smallint
### CodeTask
 - id: uint (primary key)
 - topic: ForeignKey(Topic)
 - name: varchar(64)
 - file: url (every task is stored in server as html, which is parsed in frontend)
 - generator_code: text
 - verification_code: text
 - stars: task
### AIask
 - id: uint (primary key)
 - topic: ForeignKey(Topic)
 - name: varchar(64)
 - file: url (every task is stored in server as html, which is parsed in frontend)
 - prompt: text
 - stars: task
