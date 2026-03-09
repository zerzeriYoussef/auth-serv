## Backend – Installation & Setup

This is the FastAPI backend for the project.  
Use this guide to install it locally and connect it to a PostgreSQL database so a friend can run the same setup.

### Requirements

- **Python**: 3.10 or newer
- **PostgreSQL**: any recent version (e.g. 13+)
- **Git** (optional, if cloning from a repo)

### 1. Get the code

On the machine where you want to run the backend:

```bash
git clone <your-repo-url>
cd my-awesome-project/backend
```

If you just received the folder as a `.zip`, extract it and open a terminal inside the `backend` directory.

### 2. Create and activate a virtual environment

From the `backend` directory:

```bash
python -m venv .venv
.venv\Scripts\activate  # On Windows
# source .venv/bin/activate  # On Linux/macOS
```

### 3. Install Python dependencies

With the virtual environment activated and still inside `backend`:

```bash
pip install -e .
```

This installs the backend and all its required dependencies.

### 4. Configure environment variables

The backend reads configuration from a `.env` file **in the project root**, one level above `backend` (i.e. `my-awesome-project/.env`).

1. **Copy the example file**:

   - Copy `backend/.env` to the project root and rename it to `.env`:

   ```bash
   # from the project root (where the .git folder is)
   copy backend\.env .env  # Windows PowerShell / cmd
   # cp backend/.env .env  # Linux/macOS
   ```

2. **Edit `.env` in the project root** and adjust at least:

   - **Database**
     - `POSTGRES_SERVER` (e.g. `localhost`)
     - `POSTGRES_PORT` (default `5432`)
     - `POSTGRES_DB` (database name, e.g. `app`)
     - `POSTGRES_USER`
     - `POSTGRES_PASSWORD`
   - **Security & initial user**
     - `SECRET_KEY` (set to a long random string)
     - `FIRST_SUPERUSER` (email for the initial admin user)
     - `FIRST_SUPERUSER_PASSWORD` (strong password for that user)
   - **Frontend URL**
     - `FRONTEND_HOST` (e.g. `http://localhost:5173` while developing)

Make sure these values match the actual PostgreSQL user/database you create in the next step.

### 5. Prepare the PostgreSQL database

On the machine running PostgreSQL:

1. **Create a database and user** that match the values in `.env`. For example:

   ```sql
   CREATE DATABASE app;
   CREATE USER postgres WITH PASSWORD 'admin';
   GRANT ALL PRIVILEGES ON DATABASE app TO postgres;
   ```

   (Adjust names and passwords to match your `.env`.)

2. If you received a **database dump** (e.g. `backup.sql`), restore it into the database you just created, for example:

   ```bash
   psql -h localhost -U postgres -d app -f backup.sql
   ```

### 6. Run database migrations

From the `backend` directory, with the virtual environment activated and `.env` configured:

```bash
alembic upgrade head
```

This applies all Alembic migrations and updates the database schema.

### 7. Start the backend API

From the `backend` directory, with the virtual environment activated:

```bash
fastapi dev app/main.py
# or
# uvicorn app.main:app --reload
```

By default the API will be available at:

- **http://localhost:8000**
- Interactive docs at **http://localhost:8000/docs**

### 8. Sharing with a friend

To let a friend run this project and database:

- **Send the project folder** (or a link to the Git repo).
- **Send the database backup** (e.g. `backup.sql`) if you want them to have the same data.
- Ask them to follow steps **1–7** on their own machine, using your `.env` values or their own.

