# SOP: Setting Up Django Project

This SOP assumes that:

* `Python` is already installed.
* `pip` is already installed.
* You are using **Windows** or **Linux**.

---

## Verify `Python` Installation

Check that Python is installed correctly.

```bash
python --version
```

or

```bash
python3 --version
```

Example output:

```text
Python 3.13.5
```

---

## Verify `pip` Installation

Check the pip version.

```bash
pip --version
```

Example:

```text
pip 25.x.x
```

If using Linux:

```bash
pip3 --version
```

---

## Django project directory & Virtual Environment

### Create a Django project directory First

```bash
mkdir web-project
cd web-project
```

### Create a Virtual Environment before Start the Project

```bash
python3 -m venv djangoenv
```

>This creates an isolated Python **environment** inside the `djangoenv` directory.

### Activate the Virtual Environment

- Windows (Command Prompt)

	```cmd
	djangoenv\Scripts\activate.bat
	```

- Windows (PowerShell)

	```powershell
	djangoenv\Scripts\Activate.ps1
	```

- Linux

	```bash
	source djangoenv/bin/activate
	```

- When activated, the prompt should look similar to:

	```text
	(djangoenv) user@host:~/project$
	```

---

## Upgrade `pip` (Recommended)

- Run:

	```bash
	pip install --upgrade pip
	```
	>This will upgrade pip only inside that isolated environment

- Verify:

	```bash
	pip --version
	```

---

## Install Django

Install the latest Django release:

```bash
pip install django
```

To install a specific version:

```bash
pip install django==5.2
```

---

## Verify Django Installation

- Check the installed version:

	```bash
	django-admin --version
	```

- Example:

	```text
	5.2.7
	```

- Alternatively:

	```bash
	python -m django --version
	```

---

## Django Project Initialization

Navigate to the directory where you want to create the project.

### Create New Django Project

- Example in windows:

	```bash
	cd D:\web-project
	```

- or in linux

	```bash
	cd ~/web-project
	```

- Create the project:

	```bash
	django-admin startproject my-project
	```

- This creates:

	```text
	web-project/
	├── manage.py
	└── my-project/
			├── __init__.py
			├── asgi.py
			├── settings.py
			├── urls.py
			└── wsgi.py
	```

- Move into the Project Directory
	```bash
	cd my-project
	```

#

### Run Initial Database Migrations

- Initialize the default database (SQLite by default):

	```bash
	python manage.py migrate
	```

- Successful output ends with messages similar to:

	```text
	Applying auth...
	Applying admin...
	Applying sessions...
	OK
	```

#

### Create an Administrator Account

- Create a superuser for the Django admin interface:

	```bash
	python manage.py createsuperuser
	```

- Example prompts:

	```text
	Username: admin
	Email address: admin@example.com
	Password:
	Password (again):
	```

#

### Start the Django Development Server

- Run:

	```bash
	python manage.py runserver
	```

- Default output:

	```text
	Starting development server at http://127.0.0.1:8000/
	```

- To listen on all interfaces:

	```bash
	python manage.py runserver 0.0.0.0:8000
	```

#

### Open the Application in a Browser

- Visit:

	```text
	http://127.0.0.1:8000/
	```

	>If everything is configured correctly, you should see Django’s default welcome page.

- For the admin interface:

	```text
	http://127.0.0.1:8000/admin/
	```

	>Log in using the superuser credentials created earlier.

#

### Create a Django App

- Inside the project directory:

	```bash
	python manage.py startapp myapp
	```

- This creates:

	```text
	myapp/
	├── admin.py
	├── apps.py
	├── migrations/
	├── models.py
	├── tests.py
	├── views.py
	└── __init__.py
	```

#

### Register the App

- Edit `my-project/settings.py`.

	Locate:

	```python
	INSTALLED_APPS = [
	```

	Add your app:

	```python
	INSTALLED_APPS = [
			...
			"myapp",
	]
	```

#

### Verify the Project

- Check for configuration issues:

	```bash
	python manage.py check
	```

- Expected output:

	```text
	System check identified no issues (0 silenced).
	```

---

## Freeze Installed Packages

Save project dependencies:

```bash
pip freeze > requirements.txt
```

Example `requirements.txt`:

```text
Django==5.2.7
asgiref==3.x.x
sqlparse==0.x.x
```

---

## Install Dependencies Later

To recreate the environment on another machine:

```bash
pip install -r requirements.txt
```

---

## Deactivate the Virtual Environment

When finished:

```bash
deactivate
```

Your shell prompt will return to normal.

---

## Common Commands Summary

| Task                                       | Command                               |
| ------------------------------------------ | ------------------------------------- |
| Check Python version                       | `python --version`                    |
| Check pip version                          | `pip --version`                       |
| Activate virtual environment (Windows CMD) | `djangoenv\Scripts\activate.bat`      |
| Activate virtual environment (PowerShell)  | `djangoenv\Scripts\Activate.ps1`      |
| Activate virtual environment (Linux)       | `source djangoenv/bin/activate`       |
| Upgrade pip                                | `python -m pip install --upgrade pip` |
| Install Django                             | `pip install django`                  |
| Check Django version                       | `django-admin --version`              |
| Create project                             | `django-admin startproject myproject` |
| Change to project directory                | `cd myproject`                        |
| Run migrations                             | `python manage.py migrate`            |
| Create admin user                          | `python manage.py createsuperuser`    |
| Start development server                   | `python manage.py runserver`          |
| Create app                                 | `python manage.py startapp myapp`     |
| Check project                              | `python manage.py check`              |
| Save dependencies                          | `pip freeze > requirements.txt`       |
| Install dependencies                       | `pip install -r requirements.txt`     |
| Deactivate virtual environment             | `deactivate`                          |


This SOP follows the typical workflow: 

```
**install Python → create and activate a virtual environment → install Django → create a project → run migrations → create a superuser → start the server → create and register apps → save dependencies**.
```