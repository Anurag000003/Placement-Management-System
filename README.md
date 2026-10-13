# Placement Management System

![Java](https://img.shields.io/badge/Java-17%20%7C%2021-ED8B00?style=flat&logo=openjdk&logoColor=white)
![Java Swing](https://img.shields.io/badge/GUI-Java%20Swing%20%2F%20AWT-5382A1?style=flat&logo=java&logoColor=white)
![MySQL](https://img.shields.io/badge/Database-MySQL%208.0-4479A1?style=flat&logo=mysql&logoColor=white)
![JDBC](https://img.shields.io/badge/Driver-JDBC%20Connector%2FJ-007396?style=flat&logo=oracle&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Cross--Platform%20Desktop-lightgrey?style=flat)

An enterprise-grade College Placement Management System (CPMS) engineered as a Java Desktop Application (Java Swing / AWT + JDBC + MySQL). The software streamlines university recruitment operations, student registrations, corporate recruiter job drives, eligibility filtering, application review workflows, interview scheduling, and placement reports with a high-performance offline/LAN desktop interface.

---

## Key Highlights

- **Role-Based Desktop Dashboards**: Dedicated UI control panels for Students, Corporate Recruiters/Companies, and Placement Cell Administrators.
- **Automated Eligibility Engine**: Instant desktop validation checking student CGPA, backlogs, department, and graduation year against active job drive criteria.
- **Application & Interview Lifecycle**: Complete workflow tracking student applications from submission, shortlisting, written assessments, technical interviews, to final offers.
- **Relational Data Architecture**: Normalized MySQL relational schema accessed through robust JDBC Connection Pooling and prepared statements.
- **Native Desktop Performance**: Responsive UI layout management with Java Swing, custom table models, report generation, and direct database persistence.

---

## Architecture & Tech Stack

### Desktop Application Layer
- **Language**: Java (JDK 17 / 21)
- **GUI Framework**: Java Swing & Abstract Window Toolkit (AWT)
- **Look & Feel**: System Native Look & Feel / FlatLaf
- **Components**: `JFrame`, `JPanel`, `JTable`, `CardLayout`, `GridBagLayout`, Custom Renderers & Listeners

### Data Access & Persistence Layer
- **Database Engine**: MySQL 8.0
- **Connectivity**: Java Database Connectivity (JDBC) via MySQL Connector/J
- **Security**: PreparedStatement SQL injection defense, BCrypt / salted password hashing
- **Transaction Management**: ACID compliant JDBC transaction isolation

---

## Project Structure

```bash
college-placement-management-system/
├── src/
│   ├── com/placement/
│   │   ├── main/
│   │   │   └── MainApplication.java       # Desktop application launcher & GUI initialization
│   │   ├── config/
│   │   │   └── DatabaseConnection.java    # JDBC connection factory & credentials loader
│   │   ├── dao/
│   │   │   ├── StudentDAO.java            # Student CRUD and eligibility queries
│   │   │   ├── CompanyDAO.java            # Recruiter & job drive data access
│   │   │   ├── ApplicationDAO.java        # Placement application workflow persistence
│   │   │   └── AdminDAO.java              # Administrative metrics & reporting queries
│   │   ├── models/
│   │   │   ├── Student.java               # Student academic record entity
│   │   │   ├── Company.java               # Recruiter company entity
│   │   │   ├── Job.java                   # Recruitment drive vacancy entity
│   │   │   └── Application.java           # Job application state entity
│   │   ├── views/
│   │   │   ├── LoginFrame.java            # Multi-role authentication desktop window
│   │   │   ├── StudentDashboard.java      # Student job drive portal & status tracker
│   │   │   ├── CompanyDashboard.java      # Recruiter drive management & applicant review
│   │   │   └── AdminDashboard.java        # Placement cell administration & statistics
│   │   └── utils/
│   │       ├── ValidationUtils.java       # Form & eligibility validation helpers
│   │       └── TableModelHelper.java      # Dynamic JTable data bindings
│
├── lib/                                   # External dependencies (MySQL Connector/J JAR)
├── database/
│   └── schema.sql                         # MySQL schema definitions and relational constraints
├── resources/
│   ├── db.properties                      # Database credentials and configuration
│   └── images/                            # Desktop application icons and graphics
└── README.md
```

---

## Database Architecture

The MySQL database schema contains:
- **`students`**: Academic credentials, CGPA, department, roll number, contact details, resume path.
- **`companies`**: Organization name, industry sector, HR contact, credentials, verification state.
- **`jobs`**: Job postings, salary package (CTC), eligibility criteria (min CGPA, allowed departments, max backlogs), deadlines.
- **`applications`**: Student-to-job mappings with dynamic status states (Applied, Shortlisted, Interview Scheduled, Selected, Rejected).
- **`admins`**: Administrative credentials overseeing placement records and batch reporting.

---

## Getting Started

### 1. Prerequisites
- **Java Development Kit (JDK)**: Version 17 or higher
- **MySQL Server**: Version 8.0 or higher
- **MySQL Connector/J**: JDBC Driver JAR file

### 2. Database Setup

Open your MySQL client and initialize the database:

```sql
CREATE DATABASE placement_db;
USE placement_db;
SOURCE database/schema.sql;
```

Configure your credentials in `resources/db.properties`:

```properties
db.url=jdbc:mysql://localhost:3306/placement_db?useSSL=false&serverTimezone=UTC
db.user=root
db.password=your_mysql_password
```

### 3. Compilation & Execution

#### Option A: Command Line (javac / java)
Compile the Java source files with the MySQL JDBC driver:

```bash
# Compile all source files
javac -cp "lib/*;." -d bin src/com/placement/**/*.java

# Launch the Desktop Application
java -cp "bin;lib/*;resources" com.placement.main.MainApplication
```

#### Option B: In an IDE (Eclipse, IntelliJ IDEA, or NetBeans)
1. Open the project folder in your preferred Java IDE.
2. Ensure Project SDK is set to **JDK 17+**.
3. Add `mysql-connector-j.jar` to Project Libraries / Classpath.
4. Run `MainApplication.java` to start the desktop interface.

---

## Core Desktop Application Modules

| Module | Interface | Description | Target User |
|---|---|---|---|
| Authentication | `LoginFrame` | Multi-role credential verification and registration dialog | All Users |
| Student Portal | `StudentDashboard` | View eligible drives, apply with one click, track application status | Students |
| Recruiter Portal | `CompanyDashboard` | Post new vacancy drives, review applicants, update selection stages | Recruiters |
| Admin Control Panel | `AdminDashboard` | Monitor placement statistics, approve drives, export placement reports | Placement Cell |

---

## License
This project is open-source and available under the [MIT License](LICENSE).
