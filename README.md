# Online Learning Platform

A Django web application for browsing courses, enrolling in them, and taking multiple choice exams. Built as the final project for IBM's "Developing Applications with SQL Databases and Django" course on Coursera, part of the IBM Full Stack Developer path.

![Python](https://img.shields.io/badge/Python-3.8-blue?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-4.2.3-092E20?logo=django&logoColor=white)
![Database](https://img.shields.io/badge/Database-SQLite-lightgrey?logo=sqlite&logoColor=white)
![License](https://img.shields.io/badge/License-Apache%202.0-green)

## About This Project

The base app (course listings, user accounts, enrollment) was provided as a starting template. My work was designing and building the assessment system on top of it: the database models for questions and answers, the scoring logic, and the views that tie an exam submission back to a specific learner and course.

## Features

- Browse available courses and view course details
- User registration and login
- One click enrollment in a course
- Multiple choice exams with several questions per course
- Automatic scoring: a question only counts if every correct choice is selected and no incorrect ones are
- Exam results page showing the learner's grade after submission
- Django admin panel for managing courses, lessons, questions, and choices

## What I Built

The starting template included the `Course`, `Lesson`, `Instructor`, `Learner`, and `Enrollment` models along with the course browsing and enrollment views. I added the exam feature end to end:

**Data models** (`onlinecourse/models.py`)
- `Question`: linked to a course, holds the question text and a point value
- `Choice`: linked to a question, holds the answer text and whether it is correct
- `Submission`: linked to an enrollment, records which choices a learner selected
- A helper method on `Question` that checks whether a learner's selected answers exactly match the correct ones

**Views** (`onlinecourse/views.py`)
- `submit`: reads a learner's selected choices from the exam form, creates a `Submission` tied to their enrollment, and redirects to the results page
- `show_exam_result`: recalculates the score by comparing each question's correct choices against what the learner submitted, then renders the result

**Admin** (`onlinecourse/admin.py`)
- Registered `Question` and `Choice` with an inline editor, so questions and their answer choices can be managed together from one screen instead of two

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Django 4.2.3 (Python 3.8) |
| Database | SQLite (default; swappable for PostgreSQL or MySQL) |
| Frontend | Django templates with Bootstrap |
| Image handling | Pillow |
| Deployment | Gunicorn, configured for IBM Cloud Foundry |

## Data Model

![Online course ER diagram](https://github.com/ibm-developer-skills-network/final-cloud-app-with-database/raw/master/static/media/course_images/onlinecourse_app_er.png)

A course has many lessons and many questions. Each question has several choices, and each choice is flagged correct or incorrect. When a learner submits an exam, a `Submission` record links their `Enrollment` to the `Choice` objects they picked, so the result can be recalculated at any time from that record.

## Getting Started

1. Clone the repository and move into the project folder.
   ```bash
   git clone https://github.com/Fragmatics/tfjzl-final-cloud-app-with-database.git
   cd tfjzl-final-cloud-app-with-database
   ```

2. Create and activate a virtual environment. This keeps the project's dependencies separate from anything else on your machine.
   ```bash
   python3 -m venv venv
   source venv/bin/activate
   ```

3. Install the dependencies listed in `requirements.txt`.
   ```bash
   pip install -r requirements.txt
   ```

4. Create the database tables. This applies the model definitions to a fresh SQLite database.
   ```bash
   python manage.py makemigrations onlinecourse
   python manage.py migrate
   ```

5. Create an admin account so you can add courses, questions, and choices through the Django admin.
   ```bash
   python manage.py createsuperuser
   ```

6. Start the development server.
   ```bash
   python manage.py runserver
   ```

7. Open `http://127.0.0.1:8000` for the app, or `http://127.0.0.1:8000/admin` to add course content.

## Project Structure

```
onlinecourse/
├── models.py       # Course, Learner, Enrollment, Question, Choice, Submission
├── views.py        # Course browsing, enrollment, exam submission and scoring
├── admin.py        # Admin panel configuration
├── urls.py         # URL routes for the app
└── templates/       # Bootstrap-based HTML templates
myproject/           # Django project settings and root URL config
static/              # CSS, course images, admin assets
manifest.yml         # IBM Cloud Foundry deployment config
Procfile             # Gunicorn start command for deployment
```

## Deployment

The `Procfile` and `manifest.yml` are set up for IBM Cloud Foundry, using Gunicorn as the application server and a separate static file buildpack for assets. The same Django app will run on any platform that supports Python, with the database swapped to PostgreSQL or MySQL for production use.

## Acknowledgements

Base project template generated from `ibm-developer-skills-network/coding-project-template` as part of IBM's Django and SQL databases course on Coursera.

## About the Developer

Built by Irfan, Launchpad Associate at CertiMinds, as part of hands-on training toward client-facing development work.

## License

Apache License 2.0. See `LICENSE` for details.
