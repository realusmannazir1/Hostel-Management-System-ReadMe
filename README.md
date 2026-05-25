<div align="center">

# 🏠 Hostel Management System

**A professional desktop application for managing hostel operations — students, rooms, fees, employees, and financial reporting — all in one place.**

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python&logoColor=white)
![PySide6](https://img.shields.io/badge/GUI-PySide6%20%28Qt6%29-41CD52?logo=qt&logoColor=white)
![SQLite](https://img.shields.io/badge/Database-SQLite-003B57?logo=sqlite&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Windows-0078D6?logo=windows&logoColor=white)
![Version](https://img.shields.io/badge/Version-1.0.0-success)
![License](https://img.shields.io/badge/License-Proprietary-red)

</div>

---

## Overview

Hostel Management System is a full-featured Windows desktop application built with Python and PySide6 (Qt6). It covers the complete lifecycle of hostel operations across multiple branches — from student admissions and room allocation to monthly fee collection, employee salary management, PDF receipt generation, and financial analytics.

The system enforces hardware-based license activation, role-based access control, and bcrypt password security throughout.

---

## Screenshots

> UI is built with Qt Designer `.ui` files loaded dynamically at runtime via `QUiLoader`.

| Login | Admin Dashboard | Fee Payment |
|---|---|---|
| License-gated login with lockout protection | Multi-branch overview with analytics | Monthly fee collection with receipt printing |

---

## Key Features

### Multi-Branch Management
- Create and manage unlimited hostel branches
- Each branch has its own login credentials and scoped dashboard
- Admin super-user has full cross-branch visibility and control

### Student Management
- Add, edit, archive, and readmit students
- Photo storage (BLOB in SQLite, up to 5 MB)
- CNIC, phone, emergency contact, academic details
- Soft-delete with full archive history and readmission support

### Room Management
- Define rooms with capacity, floor, type (regular/basement), and monthly fee
- Real-time occupancy tracking — seats available vs. filled
- Prevent over-allocation with atomic room assignment transactions

### Room Allocation
- Allocate students to rooms with rent, mess charges, and other charges
- Move students between rooms atomically (old seat released before new seat checked)
- Automatic check-in/check-out date tracking

### Fee Management
- Monthly fee records auto-generated per student per allocation
- Partial payment support with installment receipt history
- Payment status: `unpaid` / `partial` / `paid`
- Duplicate record detection and merge on startup
- Future-month record prevention

### Employee & Salary Management
- Full employee profiles with job role, shift, salary, and photo
- Monthly salary records auto-generated from join date
- Partial salary payment support with slip history
- Archive and readmit employees

### PDF Receipts & Printing
- Fee receipts and salary slips generated as styled PDFs
- Modular HTML template system with reusable components
- Two rendering paths: ReportLab (direct drawing) and QWebEngineView (HTML → PDF)
- Print dialog with real printer detection
- Save-as PDF flow
- Professional A4 document formatting with signatures and stamps

### Analytics Dashboard
- Branch-level and system-wide summary statistics
- Total students, employees, rooms, occupancy, collected fees, pending fees
- Trend summaries and charts via `analytics_panel.py`

### Data Export
- Export students, employees, rooms, fees, salaries to CSV
- Export full receipt history (fee + salary) to CSV
- Export archived students and employees

### Database Backup & Restore
- One-click database backup to timestamped `.db` file
- Restore from backup file
- Branch-level export/import via `.hbx` format

### Security
- Hardware UUID-based license activation (PowerShell `Win32_ComputerSystemProduct`)
- License expiry support (6-month / 12-month / unlimited)
- bcrypt password hashing for all credentials (admin + branch users)
- Legacy plain-text passwords auto-upgraded to bcrypt on first login
- Per-account login lockout: 5 failures → 5-minute lock; 15 total → permanent lock
- Global system lockout: 20 failures across all accounts → 10-minute lock
- Admin re-authentication required for destructive operations
- Audit log viewer

### Build Optimization
- Post-build pruning removes unused QtWebEngine debug resources
- Automatic removal of Qt translation files
- Reduces distribution size by 10-20 MB
- Maintains full application functionality

---

## System Requirements

| Requirement | Minimum |
|---|---|
| OS | Windows 10 / 11 (64-bit) |
| Python | 3.10 or higher |
| RAM | 512 MB |
| Disk | 200 MB (app) + database growth |
| PowerShell | Required for license activation |

---

## Dependencies

```
PySide6>=6.4.0
PySide6-WebEngine>=6.4.0
reportlab>=3.6.0
bcrypt>=4.0.0
```

> `sqlite3` is part of the Python standard library — no separate install needed.

---

## Installation & Setup

### 1. Clone the repository

```bash
git clone https://github.com/realusmannazir1/Hostel-Management-System-ReadMe.git
cd Hostel-Management-System-ReadMe
```

### 2. Create a virtual environment (recommended)

```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install PySide6 PySide6-WebEngine reportlab bcrypt
```

> If you have a `requirements.txt`:
> ```bash
> pip install -r requirements.txt
> ```

### 4. Run the application

```bash
python main.py
```

On first launch, the license activation window will appear. Enter your hardware license key to proceed.

---

## Default Credentials

| Role | Username | Password |
|---|---|---|
| Admin | `admin` | `admin` |

> Change the admin password immediately after first login via **Settings**.

---

## How to Use

### First Launch
1. Run `python main.py`
2. The **License Activation** window opens — enter your hardware UUID key
3. After activation, the **Login** window opens
4. Log in with `admin` / `admin`
5. Change your password in **Settings**

### Adding a Branch
1. Log in as admin
2. Go to **Branches** → **Add Branch**
3. Fill in branch details and set a username/password for the branch manager

### Adding Students
1. Navigate to **Students** → **Add Student**
2. Fill in personal details and upload a photo (optional)
3. After saving, go to **Allocate Room** to assign a room

### Collecting Fees
1. Go to **Fee Submit**
2. Search for the student
3. Select the month and enter the amount paid
4. Print or save the receipt as PDF

### Generating Monthly Bills
1. From the dashboard, click **Generate Monthly Bills**
2. The system creates fee and salary records for all active students and employees for the current month
3. This operation is idempotent — running it twice will not create duplicates

### Paying Employee Salaries
1. Go to **Pay Salary**
2. Search for the employee
3. Select the month and enter the amount paid
4. Print or save the salary slip as PDF

### Database Backup
1. Admin Dashboard → **Backup Database**
2. A timestamped `.db` backup is saved automatically

---

## Project Structure

```
Hostel_Managment_System/
├── main.py                     # Entry point
├── hostel.db                   # SQLite database (dev)
├── style_light.qss             # Qt stylesheet
├── hostel.spec                 # PyInstaller spec
├── build_complete.bat          # Full build script
├── quick_build.bat             # Fast build script
├── build_installer.iss         # Inno Setup installer script
├── version_info.txt            # Windows .exe version metadata
├── assets/
│   ├── photos/                 # Student/employee photos
│   └── Lugo.ico                # App icon
├── UI/                         # Qt Designer .ui layout files
│   ├── login.ui
│   ├── admin_dashboard.ui
│   ├── not_admin_dashboard.ui
│   ├── add_student.ui
│   ├── add_employee.ui
│   ├── add_room.ui
│   ├── add_hostel.ui
│   ├── allocate_room.ui
│   ├── fee_submit.ui
│   ├── pay_salary.ui
│   ├── student_history.ui
│   ├── employee_history.ui
│   ├── recovery.ui
│   └── settings.ui
└── PY/                         # All Python source modules
    ├── app_controller.py       # Window flow: License → Login → Dashboard
    ├── login.py                # Login window, lockout logic
    ├── admin_dashboard.py      # Admin dashboard (all branches)
    ├── not_admin_dashboard.py  # Branch user dashboard (scoped)
    ├── _dashboard_shared.py    # Shared table/logo/popup helpers
    ├── add_student.py          # Student CRUD form
    ├── add_employee.py         # Employee CRUD form
    ├── add_room.py             # Room CRUD form
    ├── add_hostel.py           # Branch CRUD form
    ├── allocate_room.py        # Room allocation form
    ├── fee_submit.py           # Fee payment form
    ├── pay_salary.py           # Salary payment form
    ├── student_history.py      # Fee history for a student
    ├── employee_history.py     # Salary history for an employee
    ├── left_students.py        # Archived students list
    ├── left_employees.py       # Archived employees list
    ├── detail_view.py          # Student/Employee detail popup
    ├── analytics_panel.py      # Charts and stats widget
    ├── settings_dialog.py      # App settings (theme, credentials)
    ├── auth_dialog.py          # Admin re-auth dialog
    ├── recovery_dialog.py      # Password recovery
    ├── unlock_account.py       # Unlock locked accounts
    ├── view_audit_logs.py      # Audit log viewer
    ├── license_manager.py      # License validation and activation
    ├── license_window.py       # License activation UI
    ├── MyFunction.py           # SQLite data access layer (all DB ops)
    ├── pdf_utils.py            # HTML → PDF rendering, print dialog
    ├── templates.py            # HTML receipt/slip templates
    ├── generate_receipt.py     # ReportLab receipt generator
    ├── resource_path.py        # Path resolution (dev vs frozen exe)
    ├── error_handler.py        # Global exception logging
    ├── csv_import.py           # CSV bulk import
    ├── data_transfer.py        # Branch export/import (.hbx)
    ├── db_cleanup.py           # DB maintenance utilities
    ├── prune_dist.py           # Post-build distribution pruning
    └── version.py              # APP_VERSION constant
```

---

## Database Schema

The SQLite database (`hostel.db`) contains the following tables:

| Table | Purpose |
|---|---|
| `license` | Hardware license key and expiry |
| `app_config` | Admin credentials, theme, recovery key |
| `branches` | Hostel branches with login credentials |
| `students` | Student records with photo BLOB |
| `rooms` | Room definitions with capacity and fee |
| `room_allocations` | Student-to-room assignments |
| `employees` | Employee records with photo BLOB |
| `fee_payments` | Monthly fee records per student |
| `fee_receipts` | Individual installment receipts for fees |
| `salary_payments` | Monthly salary records per employee |
| `salary_receipts` | Individual installment receipts for salaries |
| `branch_charges` | Per-branch charge settings (rent tiers, mess, other) |
| `master_key_audit` | Audit log for master key usage |
| `student_left_hostel` | Archive of students who left |
| `employee_left_hostel` | Archive of employees who left |

All passwords are stored as bcrypt hashes. Student and employee IDs use a branch-prefixed format: `{branch_id}-{seq}` (e.g., `1-1`, `2-5`).

---

## Building the Executable

### Using PyInstaller

```bash
pyinstaller hostel.spec
```

Or use the provided build scripts:

```bash
# Full build (cleans previous output)
build_complete.bat

# Quick rebuild
quick_build.bat
```

### Post-Build Optimization

After building, the distribution is automatically pruned to remove unused resources:

```bash
python PY/prune_dist.py dist/HostelManagementSystem
```

This removes:
- QtWebEngine debug resources (.debug.pak files)
- Qt translation files (.qm files)
- Chromium devtools resources

**Typical savings**: 10-20 MB from the final distribution

### Creating the Windows Installer

After building the `.exe`, use [Inno Setup](https://jrsoftware.org/isinfo.php) with the provided script:

```
build_installer.iss
```

This produces `installer_output/HostelManagementSystem_Setup_v1.0.0.exe`.

---

## Architecture

### Window Flow

```
AppController.start()
    │
    ├─ License invalid? ──► LicenseWindow ──► on activation ──► init DB ──► LoginWindow
    │
    └─ License valid? ──► init DB ──► LoginWindow
                                          │
                              ┌───────────┴───────────┐
                              ▼                       ▼
                    AdminDashboardWindow    BranchDashboardWindow
```

### Key Design Patterns

- **UI Loading** — All windows load `.ui` files at runtime via `QUiLoader`. Widgets are found with `findChild()`.
- **Database** — Single module (`MyFunction.py`) handles all DB operations. Every function opens and closes its own connection. Write transactions use `BEGIN IMMEDIATE`.
- **Session** — `CURRENT_BRANCH_ID` and `CURRENT_ROLE` globals in `MyFunction.py` track the logged-in user.
- **Path Resolution** — `resource_path.py` provides `get_resource_path()` for read-only assets and `get_data_path()` for writable data. When frozen, writable data goes to `%APPDATA%\HostelManagementSystem`.
- **Soft Delete** — Records are never hard-deleted during normal operation (`is_active=0`). Hard delete requires explicit admin confirmation.

---

## Fast Navigation

The project includes an O(1) code navigation system using pre-built JSON indexes. Find any code instantly without scanning files.

### Setup

```bash
python fast_finder.py --build
```

This creates 4 index files in `.ai_index/`:
- `module_index.json` — 8 feature modules with keywords
- `symbol_index.json` — 28 functions, classes, methods, variables
- `file_index.json` — All 43 project files
- `word_index.json` — Auto-generated word-to-symbol mapping

### Usage

```bash
python fast_finder.py
```

**Interactive commands:**

```bash
# Find feature by keywords
f:login lockout
# → Returns login module with all related files

# Find symbol by exact name
c:validate_user
# → Returns function location, signature, docstring

# Find symbols containing word
w:password
# → Returns hash_password, verify_password

# Find file path
file:fee_submit.py
# → Returns PY/fee_submit.py

# Default (no prefix): feature search
student photo
# → Returns student_management module
```

**Search types:**

| Command | Description | Example |
|---|---|---|
| `f:<query>` | Feature search by keywords | `f:receipt pdf` |
| `c:<name>` | Exact symbol lookup | `c:AdminDashboard` |
| `w:<word>` | Word-based symbol search | `w:allocate` |
| `file:<name>` | File path lookup | `file:login.py` |
| `<query>` | Default feature search | `dashboard analytics` |

**Example output:**

```
🔍 > f:fee payment

📦 fee_payment
   Monthly fee collection with receipt printing
   Files (6):
     • UI/fee_submit.ui
     • PY/fee_submit.py
     • PY/pdf_utils.py
     • PY/templates.py
     • PY/generate_receipt.py
     • PY/MyFunction.py

🔍 > c:validate_user

🔍 validate_user
   Type: method
   File: PY/login.py:45
   Module: login
   Signature: def validate_user(self, username, password)
   Doc: Validate credentials against database
```

**Note:** Re-run `python fast_finder.py --build` after adding new files or functions to update the indexes.

---

## Troubleshooting

**License activation fails**
- Ensure PowerShell is available at `C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe`
- Run the application as Administrator if PowerShell is blocked by policy

**Database is locked**
- Close all other instances of the application
- Check if antivirus is scanning `hostel.db`
- Wait a few seconds and retry

**QtWebEngine not found**
- Install separately: `pip install PySide6-WebEngine`
- PDF generation will fall back to ReportLab if WebEngine is unavailable

**Login is locked**
- Per-account lockout resets after 5 minutes
- Permanent lockout (15 failures) requires admin to unlock via **Unlock Account** dialog
- Lockout state is stored in `.account_lockouts` in the data directory

**Application crashes on startup**
- Check `logs/error_YYYYMMDD_HHMMSS.log` for the full traceback
- Ensure all dependencies are installed: `pip install PySide6 PySide6-WebEngine reportlab bcrypt`

---

## Support

For license activation or technical support:

📞 **+92 320 932 9214**

---

## Credits

This software was developed by:

**Zakirullah Afridi** & **Muhammad Usman Nazir**

---

<div align="center">

Made with Python & PySide6 · Version 1.0.0

</div>
