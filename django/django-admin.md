# `django-admin`

## 1. Definition and Purpose

`django-admin` is Django’s primary command-line utility for administrative and development tasks. It allows developers to create projects, manage databases, run servers, and perform many other operations.

It is closely related to `manage.py`.

* `django-admin` is a global command-line tool.
* `manage.py` is a project-specific wrapper that configures environment variables automatically.

In practice, both tools run the same commands, but `manage.py` simplifies usage inside a project.

---

## 2. Conceptual Architecture

### 2.1 Role in the Django Ecosystem

`django-admin` interacts with:

* Project settings
* Installed applications
* Database migrations
* Django management commands
* Middleware and configuration layers

It is the main interface between the developer and the Django framework.

---

## 3. Command Syntax

General syntax:

```bash
django-admin <command> [options]
```

Example:

```bash
django-admin startproject myproject
```

---

## 4. Global Options (Applicable to All Commands)

Every `django-admin` command supports several global parameters.

### 4.1 `--settings`

Specifies the settings module to use.

Example:

```bash
django-admin migrate --settings=myproject.settings
```

Explanation:
If not provided, Django uses the `DJANGO_SETTINGS_MODULE` environment variable.

---

### 4.2 `--pythonpath`

Adds a directory to Python’s import path.

Example:

```bash
django-admin migrate --pythonpath=/path/to/project
```

Purpose:
It allows Django to locate project modules.

---

### 4.3 Other Common Global Options

Although not all are listed in the same section of documentation, typical options include:

* `--verbosity`: controls output detail.
* `--traceback`: shows full error trace.
* `--no-color`: disables colored output.
* `--force-color`: forces colored output.

These options influence execution behavior rather than application logic.

---

## 5. Core Project and Application Commands

### 5.1 `startproject`

Creates a new Django project.

```bash
django-admin startproject myproject
```

Result:

* Generates configuration files.
* Creates a project directory structure.

Purpose:

* Initializes a Django project environment.

---

### 5.2 `startapp`

Creates a new Django application.

```bash
django-admin startapp myapp
```

Purpose:

* Adds modular functionality inside a project.

---

## 6. Development and Runtime Commands

### 6.1 `runserver`

Starts the development server.

```bash
django-admin runserver
```

Common options:

* `runserver 8000` → custom port.
* `runserver 0.0.0.0:8000` → network access.

Purpose:

* Provides a lightweight development server.

---

### 6.2 `shell`

Opens an interactive Python shell with Django context.

```bash
django-admin shell
```

Advanced options:

* `-i ipython` or `-i bpython`: select shell interface.
* `--command`: execute a command directly.
* `--nostartup`: disable startup scripts.

Purpose:

* Interactive debugging and experimentation.

---

## 7. Database and Migration Commands

### 7.1 `makemigrations`

Creates migration files from model changes.

```bash
django-admin makemigrations
```

Purpose:

* Records schema changes.

---

### 7.2 `migrate`

Applies migrations to the database.

```bash
django-admin migrate
```

Purpose:

* Synchronizes database schema with models.

---

### 7.3 `showmigrations`

Displays migration status.

```bash
django-admin showmigrations
```

Options:

* `--list`: show migrations and their status.
* `--plan`: show migration execution plan.
* `--database`: specify database.

Purpose:

* Understand migration state and dependencies.

---

### 7.4 `sqlmigrate`

Displays SQL statements for a migration.

```bash
django-admin sqlmigrate app_name migration_name
```

Purpose:

* Inspect generated SQL.

---

### 7.5 `flush`

Removes all data from the database.

```bash
django-admin flush
```

Purpose:

* Reset database content.

---

## 8. Data Import and Export Commands

### 8.1 `dumpdata`

Exports database data as JSON or other formats.

```bash
django-admin dumpdata
```

Purpose:

* Backup or migrate data.

---

### 8.2 `loaddata`

Loads fixture data into the database.

```bash
django-admin loaddata data.json
```

Purpose:

* Restore or seed data.

---

## 9. Authentication and User Management Commands

These commands are available when Django’s authentication app is enabled.

### 9.1 `createsuperuser`

Creates an administrative user.

```bash
django-admin createsuperuser
```

Purpose:

* Access the Django admin interface.

---

### 9.2 `changepassword`

Changes a user password.

```bash
django-admin changepassword username
```

Option:

* `--database`: specify database.

Purpose:

* Manage user credentials.

---

## 10. System and Configuration Commands

### 10.1 `check`

Runs system checks.

```bash
django-admin check
```

Purpose:

* Identify configuration errors.

---

### 10.2 `diffsettings`

Shows differences between current settings and defaults.

```bash
django-admin diffsettings
```

Purpose:

* Debug configuration.

---

### 10.3 `inspectdb`

Generates models from an existing database.

```bash
django-admin inspectdb
```

Purpose:

* Reverse-engineer database schema.

---

## 11. Static Files and Internationalization

### 11.1 `collectstatic`

Collects static files for deployment.

```bash
django-admin collectstatic
```

Purpose:

* Prepare static assets for production.

---

### 11.2 `makemessages`

Extracts translatable strings.

```bash
django-admin makemessages -l fr
```

Purpose:

* Prepare localization files.

---

### 11.3 `compilemessages`

Compiles translation files.

```bash
django-admin compilemessages
```

Purpose:

* Enable translations.

---

## 12. Testing Commands

### 12.1 `test`

Runs test suites.

```bash
django-admin test
```

Purpose:

* Execute automated tests.

---

## 13. Advanced and Meta Commands

### 13.1 `call_command` (Programmatic Use)

Django allows management commands to be executed from Python code using `call_command()`.

Purpose:

* Integrate management commands into scripts and applications.

---

### 13.2 Custom Commands

Developers can create custom `django-admin` commands by defining modules in an app’s `management/commands/` directory.

Purpose:

* Extend Django’s CLI capabilities.

---

## 14. Commands Provided by Installed Applications

Some commands exist only if specific Django apps are installed.
For example:

* Authentication commands require `django.contrib.auth`.
* Admin commands require `django.contrib.admin`.

This design makes Django modular and extensible.

---

## 15. Operational and Usability Features

### 15.1 Bash Completion

Django provides shell completion for `django-admin`.

Purpose:

* Improve developer productivity.

---

### 15.2 Colorized Output

Django supports customizable terminal colors via environment variables.

Purpose:

* Improve readability of CLI output.

---

### 15.3 Code Formatting Integration

Some generated files are automatically formatted using `black` if available.

Purpose:

* Enforce consistent code style.

---

## 16. Relationship Between `django-admin` and `manage.py`

Key differences:

| Feature                | django-admin                              | manage.py         |
| ---------------------- | ----------------------------------------- | ----------------- |
| Scope                  | Global                                    | Project-specific  |
| Settings configuration | Manual                                    | Automatic         |
| Typical usage          | Project creation, multi-project workflows | Daily development |

`manage.py` sets the Python path and settings automatically, simplifying command execution inside a project.

---
