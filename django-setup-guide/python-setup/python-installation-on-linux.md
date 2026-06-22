# SOP: Install Python on Linux

This Standard Operating Procedure (SOP) explains how to install Python on common Linux distributions such as Ubuntu, Debian, RHEL, CentOS, Rocky Linux, AlmaLinux, and Fedora.

> **Note:** Many Linux distributions already include Python. Before installing, verify whether it is already present.

---

## Prerequisites


- **Administrator Privileges:** \
   Ensure you have a user account with `sudo` privileges (or root access).
   
- **Internet Connection:** \
   Verify that the system has internet connectivity.


---

## Check Whether Python Is Already Installed

- Open a terminal and run:

	```bash
	python3 --version
	```

	or

	```bash
	python --version
	```

- Example output:

	```text
	Python 3.12.3
	```

	> If Python is already installed and meets your requirements, you may not need to install it again.

---

## Update Package Repositories

### RHEL/CentOS/Rocky/AlmaLinux

- For modern and latest rhel or centos
	```bash
	sudo dnf makecache
	```

- For older CentOS 7 systems:

	```bash
	sudo yum makecache
	```

### Fedora

```bash
sudo dnf makecache
```

### Ubuntu/Debian

```bash
sudo apt update
```


---

## Install Python

### RHEL 8/9, Rocky Linux, AlmaLinux

```bash
dnf install -y python3
```

### CentOS 7

```bash
yum install -y python3
```

### Fedora

```bash
dnf install -y python3
```

### Ubuntu/Debian

```bash
apt install -y python3
```

---

## Verify `python`

- Run:

	```bash
	python3 --version
	```

- Example output:

	```text
	Python 3.12.3
	```

---

## Verify `pip`

- Check whether `pip` is available:

	```bash
	pip3 --version
	```

- Example output:

	```text
	pip 25.x from ...
	```

---

## Install `pip` (If Not Already Installed)

### RHEL/CentOS/Rocky/AlmaLinux/Fedora

- For modern rhel

	```bash
	dnf install -y python3-pip
	```

- For CentOS 7:

	```bash
	yum install -y python3-pip
	```

### Ubuntu/Debian

```bash
apt install -y python3-pip
```

### Verify:

```bash
pip3 --version
```

---

## Upgrade `pip` (Recommended)

- Run:

	```bash
	python3 -m pip install --upgrade pip
	```

---

## Test Python

-	Start the Python interpreter:

	```bash
	python3
	```

- You should see a prompt similar to:

	```python
	Python 3.x.x
	>>>
	```

- Run a simple test:

	```python
	print("Hello, World!")
	```

- Expected output:

	```text
	Hello, World!
	```

- Exit the interpreter:

	```python
	exit()
	```

	or press:

	```text
	Ctrl + D
	```

---

## Create a Test Script

- Create a file named `hello.py`:

	```bash
	vim hello.py
	```

- Add the following content:

	```python
	print("Python is installed successfully!")
	```

	>Save and exit the editor.

- Run the script:

	```bash
	python3 hello.py
	```

- Expected output:

	```text
	Python is installed successfully!
	```

---

## Create a Virtual Environment

- Navigate to your project directory:

	```bash
	cd ~/projects
	```

	Create a virtual environment:

	```bash
	python3 -m venv projectenv
	```

	Activate it:

	```bash
	source projectenv/bin/activate
	```

	The shell prompt should now begin with:

	```text
	(projectenv)
	```

	To deactivate the virtual environment:

	```bash
	deactivate
	```

---

## Verify the Python Executable Location

- Find the installed Python binary:

	```bash
	which python3
	```

- Example output:

	```text
	/usr/bin/python3
	```

- Find the `pip` executable:

	```bash
	which pip3
	```

- Example output:

	```text
	/usr/bin/pip3
	```

---

## Common Troubleshooting

| Problem                      | Solution                                                                                  |
| ---------------------------- | ----------------------------------------------------------------------------------------- |
| `python3: command not found` | Install Python using your distribution’s package manager (`apt`, `dnf`, or `yum`).        |
| `pip3: command not found`    | Install the `python3-pip` package.                                                        |
| `Permission denied`          | Use `sudo` for system-wide installation or install packages inside a virtual environment. |
| `No module named venv`       | Install the virtual environment package (`python3-venv` on Ubuntu/Debian) and retry.      |

---

## Quick Verification Checklist

* ✅ `python3 --version` executes successfully.
* ✅ `pip3 --version` executes successfully.
* ✅ A simple `hello.py` script runs correctly.
* ✅ A virtual environment can be created and activated.
* ✅ `which python3` returns the expected executable path.
* ✅ `python3 -m pip install --upgrade pip` completes successfully.

---