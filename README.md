# JDBC_JSPbasic

Simple JSP/Servlet example that performs CRUD against a MySQL/MariaDB database.

Important notes about publishing to GitHub

- Do NOT commit secrets (passwords, private keys, connection strings) into the repository. This project previously included database credentials in `UserDAO.java`; the code now reads credentials from environment variables:
  - `JDBC_URL` (default: `jdbc:mysql://localhost:3306/demo?useSSL=false`)
  - `JDBC_USER` (default: `app`)
  - `JDBC_PASS` (default: `password`)

- Add a file like `.env` locally (and add it to `.gitignore`) for development if you prefer, but never commit it. Prefer using your OS/service manager or CI/CD secrets for production credentials.

Quick start (development)

1. Ensure a MySQL/MariaDB server is running and accessible on the host.

2. Install MariaDB (Linux) or MySQL (Linux alternative)

- Fedora / RHEL / CentOS (MariaDB server):

```bash
sudo dnf install -y mariadb-server
sudo systemctl enable --now mariadb
```

- Debian / Ubuntu (MariaDB server):

```bash
sudo apt update
sudo apt install -y mariadb-server
sudo systemctl enable --now mariadb
```

- Install the command-line client (if separate):

```bash
# Debian/Ubuntu
sudo apt install -y mariadb-client    # or mysql-client

# Fedora
sudo dnf install -y mariadb           # includes client tools
```

- If you prefer Oracle MySQL Server (Debian/Ubuntu), install the package provided by your distro or the MySQL APT repo:

```bash
sudo apt update
sudo apt install -y mysql-server mysql-client
sudo systemctl enable --now mysql
```

3. Create the database, create the app user, and load the schema

```bash
# create DB
sudo mysql -e "CREATE DATABASE IF NOT EXISTS demo;"

# create app user and grant privileges (change password for production)
sudo mysql -e "CREATE USER IF NOT EXISTS 'app'@'localhost' IDENTIFIED BY 'password'; GRANT ALL PRIVILEGES ON demo.* TO 'app'@'localhost'; FLUSH PRIVILEGES;"

# load schema
sudo mysql demo < create_users_table.sql
```

4. (Optional) Set environment variables to override defaults:

```bash
export JDBC_URL="jdbc:mysql://localhost:3306/demo?useSSL=false"
export JDBC_USER=app
export JDBC_PASS=password
```

5. Run with Maven (Jetty):

```bash
mvn -DskipTests jetty:run
```

Security checklist before publishing

- Remove or rotate any secrets that were committed previously.
- Add `target/`, IDE files, and any files with secrets to `.gitignore`.
- Consider adding a `security.md` or `SECURITY.md` explaining how to report issues.

If you want me to add an `env.sample` file or automated VS Code tasks/launch configs to the repo, tell me and I'll add them.
