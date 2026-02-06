# Install Odoo 18.0 on Windows

A comprehensive guide to install Odoo 18 on Windows using a specific commit.

## 1. Prerequisites

- Python 3.12 (recommended)
- Git
- PostgreSQL 15
- Visual C++ Build Tools (for compiling some Python packages)
- wkhtmltopdf (for PDF generation)

## 2. Install Python, Git, PostgreSQL, Wkhtmltopdf

### Python 3.12

- Download from https://www.python.org/downloads/windows/
- ⚠️ **Important:** Check "Add Python to PATH" during installation

### Git

- Download and install from https://git-scm.com/download/win

### PostgreSQL 15

- Download from https://www.enterprisedb.com/downloads/postgres-postgresql-downloads

### wkhtmltopdf

- Download from https://wkhtmltopdf.org/downloads.html
- Select Windows version (stable release) 0.12.6
- Install with default settings
- ⚠️ Make sure the installation path is added to system PATH: `C:\Program Files\wkhtmltopdf\bin`

## 3. Configure PostgreSQL

### Add `psql` to PATH (if not already automatic)

Default location: `C:\Program Files\PostgreSQL\15\bin`

### Create User and Database for Odoo

Open PowerShell and run `psql`:

```powershell
psql -U postgres
```

At the psql prompt, run:

```sql
CREATE USER odoo18 WITH PASSWORD 'odoo18';
ALTER ROLE odoo18 CREATEDB;
\q
```

⚠️ Replace password `odoo18` with a more secure password.

## 4. Clone Repository

⚠️ **Important:** It is recommended not to place Odoo source code and related repositories in the C:\Program Files directory to avoid permission issues on Windows. In this guide, the project will be placed in C:\Workspace\odoo.

### Create Working Directories

```powershell
mkdir C:\Workspace\odoo, C:\Workspace\odoo-ee, C:\Workspace\dev, C:\Workspace\conf, C:\Workspace\venv
```

### Odoo 18 Community Edition

```powershell
cd C:\Workspace\odoo
git clone -b 18.0 --single-branch https://github.com/odoo/odoo.git odoo18
```

Optional: Check out specific commit

⚠️ **Important:** In this guide, we need to check out the commit a9f0ec5c2 to match the Production environment.

```powershell
cd C:\Workspace\odoo\odoo18
git checkout a9f0ec5c2
```

### Latihan Odoo18

```powershell
cd C:\Workspace\dev
git clone https://github.com/fahriza-arkana/latihan_odoo18.git
```

## 5. Setup Virtual Environment and Configuration

### Create Virtual Environment

In a centralized `venv` folder:

```powershell
cd C:\Workspace\venv
py -3.12 -m venv .env-odoo18
```

### Create Configuration File

Create file `odoo18.conf` in the centralized `conf` folder:

```powershell
cd C:\Workspace\conf
notepad odoo18.conf
```

Fill with:

```ini
[options]
addons_path = C:\Workspace\odoo\odoo18\addons,C:\Workspace\dev\latihan_odoo18
admin_passwd = admin
db_host = localhost
db_port = 5432
db_user = odoo18
db_password = odoo18
bin_path = C:\Program Files\wkhtmltopdf\bin\wkhtmltopdf.exe
```

### Setup VS Code Workspace

1. Open VS Code
2. Click **File** → **Open Folder** → select folder `latihan_odoo18`
3. Click **File** → **Save Workspace As...**
4. Open file `latihan_odoo18.code-workspace` and change its contents to:

```json
{
  "folders": [
    {
      "path": "."
    },
    {
      "path": "C:\\Workspace\\odoo\\odoo18\\addons"
    }
  ],
  "launch": {
    "version": "0.2.0",
    "configurations": [
      {
        "name": "latihan_odoo18",
        "type": "debugpy",
        "request": "launch",
        "stopOnEntry": false,
        "console": "integratedTerminal",
        "program": "C:\\Workspace\\odoo\\odoo18\\odoo-bin",
        "args": ["--config=C:\\Workspace\\conf\\odoo18.conf"]
      }
    ]
  },
  "settings": {
    "python.defaultInterpreterPath": "C:\\Workspace\\venv\\.env-odoo18\\Scripts\\python.exe"
  }
}
```

## 6. Install Dependencies

First, activate the virtual environment and open the VS Code terminal, then run

```powershell
cd C:\Workspace\venv
.\.env-odoo18\Scripts\Activate
```

Then install dependencies:

```powershell
python -m pip install --upgrade pip
pip install wheel
pip install -r C:\Workspace\odoo\odoo18\requirements.txt
```

## 7. Run Odoo Service

### Using VS Code Debugger

1. Open folder `latihan_odoo18` in VS Code
2. Press `F5` or select **Run** → **Start Debugging**
3. Select configuration **"prasmul"**
4. Odoo will start running and can be accessed at `http://localhost:8069`

### Using Terminal

If you want to run without debugger:

```powershell
cd C:\Workspace\dev\latihan_odoo18
.\..\venv\.env-odoo18\Scripts\Activate
python C:\Workspace\odoo\odoo18\odoo-bin --config=C:\Workspace\conf\odoo18.conf
```

### Access Odoo

After the service is running, open your browser and access: `http://localhost:8069` and you're done!
