# SOP - Creating and Managing a Python Virtual Environment

## Purpose

This SOP explains how to **create**, **activate**, and **deactivate** a Python **virtual environment** for Django or other Python projects on both **Windows and Linux** systems.

---

## Create a Virtual Environment for Windows

### Open Command Prompt or PowerShell

Open either:

* Command Prompt (CMD), or
* Windows PowerShell

### Navigate to the Project Directory

- Change to your project folder.

  ```cmd
  cd D:\Project\Django Project
  ```

### Create a Virtual Environment

- Run the following command to create a virtual environment named `djangoenv`:

  ```cmd
  python -m venv djangoenv
  ```

  > After successful execution, a new folder named `djangoenv` will be created inside the project directory.

### Activate the Virtual Environment

- If using Command Prompt (CMD)

  ```cmd
  djangoenv\Scripts\activate.bat
  ```

- If using Windows PowerShell

  ```powershell
  djangoenv\Scripts\Activate.ps1
  ```

### Verify Activation

- After activation, your command prompt should display the **virtual environment** name at the beginning, similar to:

  ```cmd
  (djangoenv) D:\Project\Django Project>
  ```

  >This indicates that the virtual environment is **active** and any **Python packages** installed will be **isolated** to this project.

### Deactivate the Virtual Environment

- When you have finished working, **exit** the **virtual environment** by running:

  ```cmd
  deactivate
  ```

  >The `(djangoenv)` prefix will disappear from the command prompt.

---

## Create a Virtual Environment forLinux

### Open a Terminal

- Launch your preferred terminal application.

### Navigate to the Project Directory

- Move to your project folder.

  ```bash
  cd /home/web-project/django-project
  ```

### Create a Virtual Environment

- Create a virtual environment named `djangoenv`:

  ```bash
  python -m venv djangoenv
  ```

  > A new directory called `djangoenv` will be created in the current location.

### Activate the Virtual Environment

- Run the following command:

  ```bash
  source djangoenv/bin/activate
  ```

### Verify Activation

- After activation, your terminal prompt should resemble:

  ```bash
  (djangoenv) /home/web-project/django-project$
  ```

  > The `(djangoenv)` prefix confirms that the virtual environment is **active**.

### Deactivate the Virtual Environment

- To exit the virtual environment, execute:

  ```bash
  deactivate
  ```

  > The `(djangoenv)` prefix will be **removed** from the **terminal prompt**.

---

## Summary

| Operating System     | Create Virtual Environment | Activate                         | Deactivate   |
| -------------------- | -------------------------- | -------------------------------- | ------------ |
| Windows (CMD)        | `python -m venv djangoenv` | `djangoenv\Scripts\activate.bat` | `deactivate` |
| Windows (PowerShell) | `python -m venv djangoenv` | `djangoenv\Scripts\Activate.ps1` | `deactivate` |
| Linux                | `python -m venv djangoenv` | `source djangoenv/bin/activate`  | `deactivate` |

---

## Notes

* Create the **virtual environment** only **once per project**.
* Activate the **virtual environment** before **installing Python packages** or **running Django** commands.
* While the **virtual environment** is **active**, packages installed with `pip` are **isolated** to that **environment** and do not affect the **system-wide Python** installation.
* If `python` is not **recognized** on your system, you may need to use `python3` instead, particularly on many **Linux distributions**.
