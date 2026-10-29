# Time Management System

**Faculty Workload Management and Intelligent Scheduling System**

Planora is a web-based application designed to help faculty members
organize their responsibilities, manage schedules, and plan their work
more effectively. It combines task management, workload visibility,
fixed-schedule management, academic calendar information, and
AI-generated scheduling recommendations in one platform.

The project was developed as a Design Thinking prototype to address the
challenge of balancing teaching, research, mentoring, administrative
work, meetings, deadlines, and institutional events.

## Features

### Task and workload management

-   Create, edit, delete, and complete tasks.
-   Organize tasks by category: Teaching, Research, Mentoring,
    Administrative, and Meeting.
-   Record task priority, date, start time, duration, deadline, and
    description.
-   View task and workload summaries, including pending and completed
    work.

### Personalized working hours

-   Configure individual working-hour boundaries.
-   Use saved working hours when calculating available time slots and
    validating AI recommendations.

### Schedule management

-   Add, edit, select, and delete fixed schedule entries.
-   View commitments in a weekly timetable.
-   Set validity dates for schedule entries so different timetable
    periods can be represented.
-   Upload an OCR-readable timetable PDF. Gemini extracts structured
    schedule entries, which are saved to Oracle and displayed in the
    timetable.

### Academic calendar

-   Upload an academic calendar PDF for AI-assisted event extraction.
-   Store calendar events with dates, event types, campus information,
    and descriptions.
-   Review and update extracted events before they are treated as
    confirmed or updated events.

### Smart time-slot suggestions

-   Suggest a time slot based on task duration, priority, deadline,
    daily workload, and the user's working hours.
-   Check fixed schedule entries and existing scheduled tasks when
    looking for an available slot.
-   Respect schedule validity dates when checking fixed commitments.

### AI workload recommendations

-   Use Gemini to generate task scheduling recommendations from pending
    tasks, working hours, fixed schedules, and relevant academic
    calendar events.
-   Validate generated recommendations in the Flask backend before
    saving them.
-   Apply a recommendation to a task and record its acceptance status.

### Calendar and focus tools

-   Calendar views for navigating dates and tasks.
-   Focus timer for supporting individual work sessions.
-   Calendar colour customization.

## Technology Stack

  -----------------------------------------------------------------------
  Layer                               Technologies
  ----------------------------------- -----------------------------------
  Frontend                            HTML, CSS, JavaScript

  Backend                             Python, Flask

  Database                            Oracle Database / Oracle SQL

  AI                                  Google Gemini API

  PDF text extraction                 `pypdf`

  Database connectivity               Python Oracle database driver
                                      (`oracledb`, depending on project
                                      setup)
  -----------------------------------------------------------------------

## Architecture Overview

``` text
Browser (HTML, CSS, JavaScript)
              |
              v
         Flask Backend
          /    |     \
         v     v      v
      Oracle  Gemini  PDF Processing
      Database  API
          |
          v
 Tasks, schedules, events,
 users and recommendations
```

The application uses a hybrid approach:

-   **Program logic** handles authentication, database operations, task
    and schedule management, date/time checks, and recommendation
    validation.
-   **Gemini** assists with extracting structured information from PDFs
    and generating workload-based scheduling recommendations.
-   **Oracle Database** stores users, tasks, schedules, college events,
    documents, and AI recommendations.

AI output is treated as a suggestion and checked by backend rules before
it is saved. The prototype is intended to assist faculty members with
planning, not to replace their judgement.

## Project Structure

The project is organized around a Flask application and a frontend
directory. The exact files may vary with the current version of the
repository.

``` text
time_management_system/
├── app.py
├── Backend/
├── Frontend/
│   ├── Login.html
│   └── Timer.html
├── uploads/              # Uploaded PDF files; may be created at runtime
├── requirements.txt      # Add or maintain the project's dependencies
└── README.md
```

## Prerequisites

Before running the application, make sure you have:

-   Python 3.10 or a compatible version supported by your installed
    dependencies.
-   Oracle Database running and accessible from your computer.
-   An Oracle user with permission to access the application's tables
    and sequences.
-   A Google Gemini API key.
-   A browser such as Chrome, Firefox, or Safari.

The database schema, sequences, and triggers expected by the application
must be created in Oracle before using database-backed features.

## Setup and Installation

### 1. Clone the repository

``` bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_PROJECT_DIRECTORY>
```

Replace the placeholders with your repository URL and project directory.

### 2. Create and activate a virtual environment

**macOS / Linux**

``` bash
python3 -m venv venv
source venv/bin/activate
```

**Windows**

``` bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install dependencies

If the repository includes a `requirements.txt` file:

``` bash
pip install -r requirements.txt
```

Otherwise, install the packages required by your current application,
including Flask, the Oracle database driver, the Gemini SDK used by the
code, and `pypdf`. Keep dependency versions consistent with the working
development environment.

### 4. Configure Oracle Database

Start Oracle Database and create/configure the database user and schema
expected by the application.

The application currently relies on tables for:

-   `USERS`
-   `TASKS`
-   `USER_SCHEDULE`
-   `COLLEGE_EVENTS`
-   `DOCUMENTS`
-   `AI_RECOMMENDATIONS`

It also uses related sequences and triggers. Ensure the database objects
and columns match the SQL used by the version of `app.py` in your
repository.

Configure the Oracle connection settings in the application or
environment configuration used by your version of the code. **Do not
commit database passwords or other credentials to GitHub.**

### 5. Configure Gemini

Create a Gemini API key and configure it using the method supported by
your current application. If you move credentials to environment
variables, keep them in a local `.env` file and add `.env` to
`.gitignore`.

Never publish your API key in the repository, screenshots, or
documentation.

### 6. Run the application

From the project directory, activate the virtual environment and run:

``` bash
python app.py
```

Open the local URL printed by Flask in your terminal. If your
application uses the routes from the current implementation, the login
page is available at:

``` text
http://127.0.0.1:5000/login-page
```

The application must be able to connect to Oracle and access a valid
Gemini API configuration for database-backed and AI-powered features to
work.

## Using Planora

1.  **Log in or register** using the application.
2.  **Create tasks** and provide their categories, priorities, dates,
    durations, and deadlines.
3.  **Set working hours** to match your availability.
4.  Open **Manage Schedule** to add fixed commitments or upload an
    OCR-readable timetable PDF.
5.  Set the validity period when uploading a timetable.
6.  Upload the academic calendar PDF and review extracted events as
    required.
7.  Use **Smart Time Slot** to view a suggested time for a selected
    task.
8.  Open **AI Recommendations** to request AI-generated scheduling
    suggestions.
9.  Review a recommendation and use **Apply** when you want to schedule
    the task accordingly.
10. Use the calendar and focus timer to support day-to-day planning.

### Timetable PDF requirement

The current schedule-upload workflow expects a **searchable/OCR-readable
PDF**. If a timetable is only an image or a scanned PDF without an
extractable text layer, convert it to a searchable PDF with an OCR tool
before uploading it. Automatic OCR of image-only timetables is not
currently part of this prototype.

## Database Overview

  -----------------------------------------------------------------------
  Table                               Purpose
  ----------------------------------- -----------------------------------
  `USERS`                             User account details and
                                      personalized working hours

  `TASKS`                             Task details, deadlines,
                                      priorities, duration, and status

  `USER_SCHEDULE`                     Fixed weekly schedule entries and
                                      validity dates

  `COLLEGE_EVENTS`                    Academic calendar events and their
                                      review status

  `DOCUMENTS`                         Uploaded document metadata

  `AI_RECOMMENDATIONS`                Generated recommendations and
                                      acceptance status
  -----------------------------------------------------------------------

Refer to the SQL scripts or database setup notes included in the
repository for the exact schema and constraints.

## Security and Limitations

-   Keep API keys, database credentials, and secret configuration out of
    version control.
-   Use a local environment file or another secure configuration
    mechanism for secrets.
-   Uploaded PDFs should be validated and handled carefully.
-   AI-generated timetable entries and recommendations may need human
    review; extraction and reasoning are not guaranteed to be
    error-free.
-   Schedule upload currently relies on text being extractable from the
    PDF.
-   The project is a functional prototype. Production use would require
    additional security hardening, deployment configuration, broader
    testing, and stakeholder validation.

## Future Enhancements

Possible future improvements include:

-   Built-in OCR for image-only timetable PDFs.
-   A preview-and-confirm step for extracted timetable entries.
-   More advanced workload analytics and reporting.
-   Learning from historical scheduling outcomes, if suitable data
    becomes available.
-   Deployment for multiple departments and campuses.
-   Additional automated tests and accessibility improvements.

## Contributing

This project was developed as an academic Design Thinking prototype. If
extending it, test changes against the Oracle schema and existing
scheduling constraints before merging them.

## Acknowledgements

-   Christ University --- academic project context.
-   Google Gemini --- AI-assisted timetable/calendar extraction and
    workload recommendations.
-   Flask, Oracle Database, Python, and related open-source tooling used
    in the application.

------------------------------------------------------------------------

**Planora** --- helping faculty organize responsibilities and plan
their time more effectively.
