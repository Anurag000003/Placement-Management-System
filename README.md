# Placement Management System

![Python](https://img.shields.io/badge/Python-3.x-3776AB?style=flat&logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-3.1-000000?style=flat&logo=flask&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?style=flat&logo=mysql&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?style=flat&logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-ECR%20%7C%20EC2%20%7C%20S3%20%7C%20CodeBuild-FF9900?style=flat&logo=amazonwebservices&logoColor=white)

A full-stack, enterprise-grade College Placement Management System (CPMS) engineered to streamline university recruitment drives, student career registrations, job postings, eligibility verifications, application pipelines, and interview scheduling. Fully containerized with Docker and powered by an automated AWS CI/CD pipeline using AWS CodeBuild, Amazon ECR, Amazon S3, and Amazon EC2.

---

## Key Highlights

- **Role-Based Access Control (RBAC)**: Dedicated dashboards and authenticated portals for Students, Corporate Recruiters/Companies, and Placement Cell Administrators.
- **Automated Eligibility Filtering**: Algorithmic evaluation matching student CGPA, backlogs, department, and graduation year directly against job drive criteria.
- **Application & Interview Tracking**: Real-time status pipeline tracking applications from submitted, shortlisted, and assessment, to technical interviews and offer rollouts.
- **Relational Data Architecture**: Robust normalized schema managing students, companies, job listings, applications, drive schedules, and placement offers.
- **Cloud-Native CI/CD Pipeline**: Automated DevSecOps pipeline with vulnerability audits (`pip-audit`), Docker image compilation, ECR registry publishing, S3 frontend hosting, and automated EC2 deployment via AWS Systems Manager (SSM).

---

## Architecture & Tech Stack

### Backend
- **Framework**: Python 3, Flask 3.1
- **Database Driver**: Flask-MySQLdb, PyMySQL, mysqlclient
- **Security & Authentication**: Werkzeug password hashing, CORS middleware (`flask-cors`)
- **Environment Management**: Python-dotenv
- **Containerization**: Docker (Python 3.10 slim base)

### Database
- **Engine**: MySQL 8.0
- **Schema**: Relational tables for users, students, companies, jobs, applications, and placement statistics

### Frontend
- **Interface**: Responsive multi-role web interface (Student, Company, Admin dashboards)
- **Styling**: Modular CSS design system
- **Client Logic**: Vanilla JavaScript asynchronous REST API consumption

### Cloud Infrastructure & DevSecOps
- **Security Scanning**: `pip-audit` automated dependency vulnerability scanner
- **Container Registry**: Amazon ECR (Elastic Container Registry)
- **Application Hosting**: Amazon EC2 with containerized systemd/shell deployments
- **Static Hosting**: Amazon S3 static website hosting
- **CI/CD Pipeline**: AWS CodeBuild configured via `buildspec.yml`

---

## Project Structure

```bash
college-placement-management-system/
├── backend/
│   ├── app.py                 # Core Flask application, REST endpoints & MySQL queries
│   ├── Dockerfile             # Multi-stage container definition
│   ├── requirements.txt       # Python dependency specifications
│   └── .dockerignore
│
├── frontend/
│   ├── index.html             # Landing page
│   ├── student-login.html     # Student authentication portal
│   ├── student-signup.html    # Student registration & academic profile setup
│   ├── student-dashboard.html # Job application portal, eligibility view & status tracker
│   ├── company-login.html     # Recruiter login
│   ├── company-signup.html    # Company profile & verification
│   ├── company-dashboard.html # Post job drives, review applicants & schedule rounds
│   ├── admin-dashboard.html   # Placement officer analytics, student approvals & drive oversight
│   ├── css/                   # Stylesheets
│   └── js/                    # Client-side API request controllers
│
├── database/
│   └── schema.sql             # MySQL schema definitions and relational constraints
│
├── docs/                      # Architectural diagrams and documentation
├── buildspec.yml              # AWS CodeBuild automated pipeline definition
└── README.md
```

---

## Database Architecture

The core relational database model includes:
- **`students`**: Academic credentials, CGPA, department, roll number, contact details, resume links.
- **`companies`**: Organization details, industry domain, HR contact, verification status.
- **`jobs`**: Job roles, salary packages (CTC), eligibility criteria (min CGPA, allowed departments, max backlogs), deadlines.
- **`applications`**: Student-to-job mappings with dynamic application states (Applied, Shortlisted, Interview Scheduled, Selected, Rejected).
- **`admins`**: Central administrative accounts overseeing placement records and reporting.

---

## Getting Started

### 1. Prerequisites
- **Python** (v3.10 or higher)
- **MySQL Server** (v8.0 or higher)
- **Docker** (optional, for containerized run)

### 2. Database Setup

Log in to your MySQL terminal and run:

```sql
CREATE DATABASE placement_db;
USE placement_db;
SOURCE database/schema.sql;
```

### 3. Backend Setup

```bash
cd backend
python -m venv venv
# On Windows:
venv\Scripts\activate
# On Linux/macOS:
source venv/bin/activate

pip install -r requirements.txt
```

Create a `.env` file inside `backend/`:

```env
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=your_mysql_password
DB_NAME=placement_db
SECRET_KEY=your_secret_key
PORT=5000
```

Start the Flask application:

```bash
python app.py
```
> API runs on `http://localhost:5000`.

### 4. Running with Docker

```bash
cd backend
docker build -t college-placement-backend .
docker run -d -p 5000:5000 --env-file .env college-placement-backend
```

### 5. Frontend Access

Open `frontend/index.html` in a web browser or serve it using any HTTP server:

```bash
cd frontend
python -m http.server 3000
```
> Access the interface at `http://localhost:3000`.

---

## AWS CI/CD Pipeline Workflow

The repository includes a production-grade `buildspec.yml` pipeline:

1. **Pre-Build (DevSecOps Audit)**:
   - Installs and executes `pip-audit` to detect vulnerable packages.
   - Authenticates Docker client against Amazon ECR.
2. **Build (Packaging & Static Sync)**:
   - Compiles backend Docker image and tags it for ECR.
   - Synchronizes frontend assets directly to Amazon S3 bucket (`aws s3 sync frontend s3://... --delete`).
3. **Post-Build (Deploy)**:
   - Pushes Docker container to Amazon ECR.
   - Triggers remote zero-downtime deployment script on Amazon EC2 via AWS Systems Manager (`aws ssm send-command`).

---

## API Endpoints Overview

| Method | Endpoint | Description | Role |
|---|---|---|---|
| POST | `/api/student/signup` | Register new student profile | Student |
| POST | `/api/student/login` | Authenticate student | Student |
| GET | `/api/student/jobs` | Retrieve eligible job drives | Student |
| POST | `/api/student/apply` | Apply for open job vacancy | Student |
| POST | `/api/company/signup` | Register recruiter profile | Company |
| POST | `/api/company/login` | Authenticate company | Company |
| POST | `/api/company/post-job` | Create new recruitment drive | Company |
| GET | `/api/company/applicants/<job_id>` | View applicants for drive | Company |
| POST | `/api/company/update-status` | Update applicant hiring stage | Company |
| GET | `/api/admin/analytics` | Overview placement rates & packages | Admin |

---

## License
This project is open-source and available under the [MIT License](LICENSE).
