# Install Odoo 18.0 on Ubuntu

A comprehensive guide to install Odoo 18 on Ubuntu using a specific commit.

## 1. Prerequisites

- Python 3.12 (recommended)
- Git
- PostgreSQL 15
- build-essential (for compiling some Python packages)
- wkhtmltopdf (for PDF generation)

## 2. Install Python, Git, PostgreSQL, Wkhtmltopdf

### Update System Packages

Open Terminal and run:

```bash
sudo apt update
sudo apt upgrade -y
```

### Install Build Tools

```bash
sudo apt install -y build-essential libssl-dev libffi-dev python3-dev
```

### Python 3.12

```bash
sudo apt install -y python3.12 python3.12-venv python3.12-dev
```

### Git

```bash
sudo apt install -y git
```

### PostgreSQL 15

```bash
sudo apt install -y postgresql postgresql-contrib
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

### wkhtmltopdf

```bash
sudo apt install -y wkhtmltopdf
```

## 3. Configure PostgreSQL

### Create User and Database for Odoo

Open Terminal and run:

```bash
sudo -u postgres psql
```

At the psql prompt, run:

```sql
CREATE USER odoo18 WITH PASSWORD 'odoo18';
ALTER ROLE odoo18 CREATEDB;
\q
```

⚠️ Replace password `odoo18` with a more secure password.

## 4. Clone Repository

⚠️ **Important:** It is recommended to place Odoo source code and related repositories in your home directory. In this guide, the project will be placed in `/home/your_username/Workspace/odoo` (replace `your_username` with your actual username).

### Create Working Directories

```bash
mkdir -p ~/Workspace/{odoo,odoo-ee,dev,conf,venv}
```

### Odoo 18 Community Edition

```bash
cd ~/Workspace/odoo
git clone -b 18.0 --single-branch https://github.com/odoo/odoo.git odoo18
```

Optional: Check out specific commit

⚠️ **Important:** In this guide, we need to check out the commit a9f0ec5c2 to match the Production environment.

```bash
cd odoo18
git checkout a9f0ec5c2
```

### Latihan Odoo18

```bash
cd ~/Workspace/dev
git clone https://github.com/fahriza-arkana/latihan_odoo18.git
```

## 5. Setup Virtual Environment and Configuration

### Create Virtual Environment

In a centralized `venv` folder:

```bash
cd ~/Workspace/venv
python3.12 -m venv .env-odoo18
```

### Create Configuration File

Create file `odoo18.conf` in the centralized `conf` folder:

```bash
cd ~/Workspace/conf
nano odoo18.conf
```

Fill with:

```ini
[options]
addons_path = /home/your_username/Workspace/odoo/odoo18/addons,/home/your_username/Workspace/dev/latihan_odoo18
admin_passwd = admin
db_host = localhost
db_port = 5432
db_user = odoo18
db_password = odoo18
bin_path = /usr/bin/wkhtmltopdf
```

⚠️ **Important:** Replace `your_username` with your actual username.

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
      "path": "/home/your_username/Workspace/odoo/odoo18/addons"
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
        "program": "/home/your_username/Workspace/odoo/odoo18/odoo-bin",
        "args": ["--config=/home/your_username/Workspace/conf/odoo18.conf"]
      }
    ]
  },
  "settings": {
    "python.defaultInterpreterPath": "/home/your_username/Workspace/venv/.env-odoo18/bin/python"
  }
}
```

⚠️ **Important:** Replace `your_username` with your actual username.

## 6. Install Dependencies

First, activate the virtual environment and open the VS Code terminal, then run

```bash
cd /home/your_username/Workspace/venv
source .env-odoo18/bin/activate
```

Then install dependencies:

```bash
python -m pip install --upgrade pip
pip install wheel
pip install -r /home/your_username/Workspace/odoo/odoo18/requirements.txt
```

⚠️ **Important:** Replace `your_username` with your actual username.

## 7. Run Odoo Service

### Using VS Code Debugger

1. Open folder `latihan_odoo18` in VS Code
2. Press `F5` or select **Run** → **Start Debugging**
3. Select configuration **"latihan_odoo18"**
4. Odoo will start running and can be accessed at `http://localhost:8069`

### Using Terminal

If you want to run without debugger:

```bash
cd /home/your_username/Workspace/dev/latihan_odoo18
source /home/your_username/Workspace/venv/.env-odoo18/bin/activate
python /home/your_username/Workspace/odoo/odoo18/odoo-bin --config=/home/your_username/Workspace/conf/odoo18.conf
```

⚠️ **Important:** Replace `your_username` with your actual username.

### Access Odoo

After the service is running, open your browser and access: `http://localhost:8069` and you're done!
