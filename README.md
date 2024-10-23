# Odoo 18 Instance Setup

This README provides instructions for setting up your Odoo 18 instance based on the repository at https://github.com/qianyuzhou97/odoo18.

## Prerequisites

- Git
- Python 3.10 (see installation instructions below)

## Setup Instructions

1. Clone the Odoo 18 repository:
   ```
   git clone https://github.com/qianyuzhou97/odoo18.git
   cd odoo18
   ```

2. Clone the official Odoo repository:
   ```
   git clone -b 18.0 --single-branch --depth 1 https://github.com/odoo/odoo.git src/odoo
   ```

3. Create necessary directories (if they don't already exist):
   ```
   mkdir -p src filestore logs
   ```

4. (For Linux users) Install system dependencies and set up PostgreSQL:
   ```
   sudo apt install libldap2-dev libpq-dev libsasl2-dev postgresql postgresql-client
   sudo -u postgres createuser -d -R -S $USER
   createdb $USER
   ```

5. Set up the Python environment:

   a. For servers outside China:
      - Install `uv` (a faster, more reliable Python package installer and resolver):
        Choose the appropriate method for your operating system:
        - On macOS and Linux:
          ```
          curl -LsSf https://astral.sh/uv/install.sh | sh
          ```
        - On Windows (using PowerShell):
          ```
          powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
          ```
        - Using pip (cross-platform):
          ```
          pip install uv
          ```
      - Create a virtual environment and install dependencies:
        ```
        uv venv -p 3.10 env
        source env/bin/activate  # On Windows, use `env\Scripts\activate`
        uv pip install -r src/odoo/requirements.txt
        ```

   b. For servers inside China:
      - Download and install Python 3.10 from a reliable source or your organization's approved mirror.
      - Create a virtual environment:
        ```
        python3.10 -m venv env
        source env/bin/activate  # On Windows, use `env\Scripts\activate`
        ```
      - Upgrade pip and install dependencies:
        ```
        pip install --upgrade pip
        pip install -r src/odoo/requirements.txt
        ```
      Note: You may need to use a local PyPI mirror. Add the `-i` option to pip install commands with a reliable Chinese PyPI mirror if necessary.

6. Initialize Odoo with the correct configuration:
   ```
   bin/odoo --stop-after-init --save \
   --addons-path src/odoo/odoo/addons,src/odoo/addons,local \
   --data-dir filestore
   ```
   Note: This command will create or update your Odoo configuration file.

## Running Odoo

To start Odoo after the initial setup:

1. Activate the virtual environment:
   ```
   source env/bin/activate  # On Windows, use `env\Scripts\activate`
   ```

2. Run Odoo:
   ```
   ./odoo-bin --addons-path=addons,../../local -d mydb
   ```

Access Odoo in your web browser at `http://localhost:8069` (default port)

## Additional Notes

- Ensure that the `bin/odoo` script has execute permissions. If not, run:
  ```
  chmod +x bin/odoo
  ```
- Review and adjust the `odoo18.cfg` file for any custom configurations needed for your project.
- The `local/` directory is where you should place your custom modules.
