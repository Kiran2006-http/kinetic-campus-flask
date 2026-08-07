# 🚀 Kinetic Campus — Python Flask Application

> **College submission version** — A complete conversion of the original Java Servlet + JSP project to Python Flask, keeping identical functionality, routes, database schema, and UI.

---

## 📁 Project Structure

```
FLASK APP/
├── app.py                  ← Main Flask application (all routes)
├── requirements.txt        ← Python dependencies
├── setup.sql               ← MySQL database schema
├── static/
│   └── uploads/            ← Uploaded event banner images (auto-created)
└── templates/
    ├── index.html          ← Home page  (was Index.jsp)
    ├── register.html       ← Login/Register page  (was Register.jsp)
    ├── dashboard.html      ← Post-registration success  (was Dashboard.jsp)
    ├── explore.html        ← Browse events + calendar  (was explore.jsp)
    ├── create_event.html   ← Create new event form  (was CreateEvent.jsp)
    ├── event_details.html  ← Event RSVP page  (was EventDetails.jsp)
    ├── my_events.html      ← Student's registered events  (was MyEvents.jsp)
    ├── admin_dashboard.html← Admin analytics panel  (was AdminDashboard.jsp)
    └── success.html        ← Event creation success  (was Success.jsp)
```

---

## 🔗 Route Mapping (Java → Flask)

| Original Java Servlet / JSP | Flask Route | Method |
|---|---|---|
| `Index.jsp` | `/` | GET |
| `Register.jsp` | `/register` | GET |
| `RegisterServlet` | `/register` | POST |
| `LoginServlet` | `/login` | POST |
| `LogoutServlet` | `/logout` | GET/POST |
| `Dashboard.jsp` | `/dashboard` | GET |
| `CreateEvent.jsp` | `/create-event` | GET |
| `CreateEventServlet` | `/create-event` | POST |
| `Success.jsp` | `/success` | GET |
| `explore.jsp` | `/explore` | GET |
| `EventDetails.jsp` | `/event-details?id=X` | GET |
| `RegisterEventServlet` | `/register-event` | POST |
| `MyEvents.jsp` | `/my-events` | GET |
| `DeregisterServlet` | `/deregister` | POST |
| `AdminDashboard.jsp` | `/admin` | GET |
| `CheckNotificationServlet` | `/check-notifications` | GET |
| `GetPreviousEventsServlet` | `/get-previous-events` | GET |

---

## ⚙️ Setup & Run

### 1. Install Python dependencies
```bash
pip install -r requirements.txt
```

### 2. Set up the MySQL database
Open MySQL and run:
```bash
mysql -u root -p < setup.sql
```
Or paste the contents of `setup.sql` into MySQL Workbench / phpMyAdmin.

### 3. Configure DB credentials
In `app.py`, update the `DB_CONFIG` dictionary if your credentials differ:
```python
DB_CONFIG = {
    "host":     "localhost",
    "user":     "root",
    "password": "gaurav@03",   # ← change this if needed
    "database": "kinetic_db"
}
```

### 4. Run the Flask app
```bash
python app.py
```

The app will start at **http://localhost:5000**

---

## 🗄️ Database Schema

Same as the original Java project, plus the **`department`** column added to `event_registrations` (required by the RSVP form):

| Table | Key Columns |
|---|---|
| `users` | id, full_name, email, password, role |
| `events` | id, title, event_date, semester, purpose, outcome, location, capacity, banner_image, join_count |
| `event_registrations` | id, event_id, student_name, semester, **department**, section, university_id, registration_date |
| `notifications` | id, event_id, message, is_read, created_at |

> **Note:** If you already created the database with the old Java schema (without `department`), run:
> ```sql
> ALTER TABLE event_registrations ADD COLUMN department VARCHAR(255) DEFAULT NULL AFTER semester;
> ```

---

## ✨ Features

- 🔐 **Session-based authentication** — login, register, auto-login after signup, logout
- 👤 **Role-based access** — Student vs Admin views
- 📅 **Interactive calendar** — filter events by date
- 🖼️ **Banner image uploads** — stored in `static/uploads/`
- 🔔 **Live notifications** — bell icon polls every 3 seconds via AJAX
- 📊 **Admin dashboard** — expandable event rows showing registered students
- 📋 **My Events page** — view and deregister from events
- 🎯 **Event RSVP** — register with name, semester, department, section, university ID

---

## 🛠️ Tech Stack

| Component | Java Version | Python Version |
|---|---|---|
| Language | Java 17 | Python 3.x |
| Framework | Jakarta Servlet API | Flask 3.x |
| Templates | JSP | Jinja2 HTML |
| Database | MySQL via JDBC | MySQL via mysql-connector-python |
| Sessions | HttpSession | Flask session (server-side) |
| File Uploads | `@MultipartConfig` | Werkzeug / Flask `request.files` |
| CSS | Tailwind CDN | Tailwind CDN (identical) |
