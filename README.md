# Auto-Attendance-app

WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 1
WorkFlow
HR & Attendance Management Platform
Complete Project Development Guide
Flutter (Mobile) + Node.js (Backend)
Version 1.0 | April 2025
10
Screens designed
8
Major modules
60+
Total features
4
Dev phases
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 2
Table of Contents
Table of Contents............................................................................................................................ 2
1.1 Product Vision ....................................................................................................................... 4
1.2 Target Users.......................................................................................................................... 4
1.3 Core Design Principles.......................................................................................................... 4
1.4 Technology Stack.................................................................................................................. 4
2.1 High-Level Architecture......................................................................................................... 6
2.2 Backend Folder Structure (Node.js) ..................................................................................... 6
2.3 Flutter App Folder Structure.................................................................................................. 6
3.1 Collections Overview............................................................................................................. 8
3.2 Employee Schema (employees)........................................................................................... 8
3.3 Attendance Schema (attendance) ........................................................................................ 9
4.1 Auth Endpoints .................................................................................................................... 10
4.2 Attendance Endpoints ......................................................................................................... 10
4.3 Leave Endpoints ................................................................................................................. 10
4.4 Payroll Endpoints ................................................................................................................ 11
4.5 Shift Endpoints .................................................................................................................... 11
5.1 All Screens List.................................................................................................................... 12
5.2 Home Screen Implementation (Flutter) .............................................................................. 13
5.3 Clock-In Flow (Biometric + GPS)........................................................................................ 13
6.1 Attendance Controller ......................................................................................................... 15
6.2 OT Calculation Service ....................................................................................................... 15
6.3 Payroll Calculation Service ................................................................................................. 16
6.4 Scheduled Jobs (Cron) ....................................................................................................... 16
7.1 Attendance & Time.............................................................................................................. 18
7.2 Leave Management ............................................................................................................ 18
7.3 Payroll.................................................................................................................................. 19
7.4 Core HR............................................................................................................................... 19
7.5 Performance Management ................................................................................................. 20
7.6 Employee Engagement....................................................................................................... 20
8.1 Phase 1 — Core MVP (Weeks 1–6)................................................................................... 21
8.2 Phase 2 — Growth Features (Weeks 7–16) ...................................................................... 21
8.3 Phase 3 — Engagement & Intelligence (Weeks 17–28).................................................... 22
8.4 Phase 4 — Enterprise Expansion (Month 7+).................................................................... 22
9.1 pubspec.yaml — Key Dependencies.................................................................................. 23
9.2 Node.js package.json — Key Dependencies ..................................................................... 24
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 3
10.1 Authentication & Authorization.......................................................................................... 25
10.2 Data Security..................................................................................................................... 25
10.3 Statutory Compliance Features ........................................................................................ 25
11.1 Admin Panel Pages........................................................................................................... 26
11.2 Admin Panel Tech Stack................................................................................................... 26
12.1 Backend Testing (Jest + Supertest) ................................................................................. 27
12.2 Flutter Testing ................................................................................................................... 27
13.1 Infrastructure ..................................................................................................................... 27
13.2 CI/CD Pipeline (GitHub Actions)....................................................................................... 28
Immediate next steps for the development team ..................................................................... 29
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 4
1. Project Overview
What WorkFlow is, who it's for, and why we're building it
1.1 Product Vision
WorkFlow is a mobile-first HR and workforce management platform inspired by Keka HR. It 
targets mid-to-large organizations needing a seamless, all-in-one system for attendance, 
payroll, leave, performance, and employee engagement — accessible from mobile for 
employees and web for managers and admins.
1.2 Target Users
User Role Platform Primary Actions
Employee Mobile (Flutter) Clock in/out, apply leave, view payslip, OT report, 
shift change
Manager / Team Lead Mobile + Web Approve leaves, monitor attendance, team 
dashboard
HR Admin Web (Admin Panel) Manage employees, payroll, reports, compliance
Super Admin Web System config, roles, org structure, integrations
1.3 Core Design Principles
• Mobile-first: every feature works flawlessly on mobile before web
• Keka parity: match all Keka HR features across 8 modules
• Biometric-native: fingerprint and face ID punch built into the core loop
• Real-time: live attendance, notifications, and team status
• Compliance-ready: PF, ESI, TDS, LWF, and Form 16 built in
1.4 Technology Stack
Layer Technology Purpose
Mobile App Flutter 3.x (Dart) Cross-platform iOS + Android from single 
codebase
Backend API Node.js + Express.js REST API server, business logic, auth
Database MongoDB (Atlas) ( and 
multiple free database Sets)
Primary data store — employees, attendance, 
payroll
Cache Redis Sessions, OTP, real-time attendance state
Push Notifications Firebase Cloud Messaging 
(FCM)
Leave alerts, late alerts, payslip notifications
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 5
Auth JWT + Firebase Auth Token-based auth, biometric, OAuth
File Storage AWS S3 / Firebase Storage Payslips, documents, profile photos
Real-time Socket.IO Live team attendance board
Maps / Geofence Google Maps API GPS punch validation, geofence detection
Biometric Flutter local_auth plugin Fingerprint + Face ID on device
CI/CD GitHub Actions + Fastlane Automated build, test, deploy
Monitoring Sentry + Datadog Error tracking, performance monitoring
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 6
2. System Architecture
How the entire platform is structured end-to-end
2.1 High-Level Architecture
WorkFlow follows a layered client-server architecture. The Flutter mobile app and React web 
admin panel both communicate exclusively with the Node.js REST API. The API layer handles 
all business logic and talks to MongoDB for persistence, Redis for caching, and Firebase for 
push notifications.
Architecture Pattern: MVC on backend (Model-View-Controller) + BLoC pattern on Flutter frontend for 
reactive state management.
2.2 Backend Folder Structure (Node.js)
workflow-backend/
├── src/
│ ├── config/ # DB, Redis, Firebase, env config
│ ├── middleware/ # Auth, role-check, error handler, rate limiter
│ ├── models/ # Mongoose schemas (Employee, Attendance, Leave...)
│ ├── routes/ # API route definitions
│ ├── controllers/ # Business logic handlers
│ ├── services/ # Reusable services (payroll calc, OT engine...)
│ ├── utils/ # Helpers, validators, formatters
│ └── jobs/ # Cron jobs (daily OT calc, payroll run, LOP)
├── tests/ # Jest unit & integration tests
├── .env # Environment variables
└── server.js # Entry point
2.3 Flutter App Folder Structure
workflow_app/
├── lib/
│ ├── core/ # Constants, themes, routes, DI container
│ ├── data/
│ │ ├── models/ # Dart data models (JSON serializable)
│ │ ├── repositories/ # Data access layer (API calls)
│ │ └── datasources/ # Remote (HTTP) + Local (Hive cache)
│ ├── domain/
│ │ ├── entities/ # Pure business objects
│ │ └── usecases/ # ClockIn, ApplyLeave, GetPayslip...
│ ├── presentation/
│ │ ├── blocs/ # BLoC state management per feature
│ │ ├── screens/ # All UI screens
│ │ └── widgets/ # Reusable components
│ └── main.dart # App entry point
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 7
├── assets/ # Images, icons, fonts
├── test/ # Widget + unit tests
└── pubspec.yaml # Dependencies
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 8
3. Database Design
MongoDB schemas for all core collections
3.1 Collections Overview
Collection Description Key Fields
employees All employee records empId, name, role, department, shift, salary, 
status
attendance Daily punch records empId, date, clockIn, clockOut, status, 
otHours, location
leaves Leave requests empId, type, fromDate, toDate, reason, 
status, approvedBy
payroll Monthly payroll runs empId, month, basic, allowances, 
deductions, net, status
shifts Shift definitions name, startTime, endTime, days, 
graceMinutes, otAfter
departments Org structure name, head, parentDept, location
notifications Push & in-app alerts userId, type, title, body, isRead, createdAt
expenses Expense claims empId, category, amount, receipt, status, 
approvedBy
assets Company assets assetId, type, assignedTo, condition, 
acknowledgedAt
performance Review cycles empId, cycle, goals, rating, feedback, 
reviewedBy
surveys Pulse surveys title, questions, responses, targetGroup, 
closedAt
tickets HR helpdesk empId, category, subject, status, resolvedAt
3.2 Employee Schema (employees)
{
 _id: ObjectId,
 empId: String, // "EMP001"
 name: { first, last },
 email: String,
 phone: String,
 avatar: String, // S3 URL
 role: String, // "employee" | "manager" | "hr" | "admin"
 department: ObjectId, // ref: departments
 designation: String,
 shift: ObjectId, // ref: shifts
 manager: ObjectId, // ref: employees
 joinDate: Date,
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 9
 employmentType: String, // "full-time" | "part-time" | "contract"
 salary: {
 basic: Number, allowances: Number, hra: Number,
 pf: Number, esi: Number, tds: Number
 },
 leaveBalance: { annual: Number, sick: Number, casual: Number, ... },
 status: String, // "active" | "inactive" | "on-notice"
 fcmToken: String, // Firebase push token
 biometricEnabled: Boolean,
 createdAt: Date, updatedAt: Date
}
3.3 Attendance Schema (attendance)
{
 _id: ObjectId,
 empId: ObjectId, // ref: employees
 date: Date,
 clockIn: { time: Date, location: { lat, lng }, method: "biometric"|"manual" },
 clockOut: { time: Date, location: { lat, lng }, method: "biometric"|"manual" },
 status: String, // "present"|"absent"|"half-day"|"pending"|"holiday"
 workingHours: Number, // calculated in hours
 otHours: Number, // overtime hours
 isRegularized: Boolean,
 regularizationReason: String,
 approvedBy: ObjectId,
 geofenceValid: Boolean,
 shift: ObjectId,
 createdAt: Date, updatedAt: Date
}
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 10
4. API Design (Node.js REST)
All endpoints, request/response contracts
4.1 Auth Endpoints
Method Endpoint Description Auth
POST /api/auth/login Email + password login Public
POST /api/auth/biometric Biometric token validation Public
POST /api/auth/logout Invalidate JWT session Bearer
POST /api/auth/refresh Refresh access token Refresh 
token
POST /api/auth/otp/send Send OTP to phone/email Public
POST /api/auth/otp/verify Verify OTP Public
4.2 Attendance Endpoints
Method Endpoint Description
POST /api/attendance/clock-in GPS + biometric clock-in, creates attendance 
record
POST /api/attendance/clock-out Clock out, calculates working hours and OT
GET /api/attendance/today Today's status for current employee
GET /api/attendance/me?month=&year= Employee's attendance log by month
POST /api/attendance/regularize Submit correction request for missed punch
GET /api/attendance/team?date= Manager: all team members status for a date
GET /api/attendance/ot￾report?empId=&month=
OT report with daily breakdown
PATCH /api/attendance/approve/:id HR/Manager approve regularization
GET /api/attendance/export?dept=&month= Export attendance as CSV
4.3 Leave Endpoints
Method Endpoint Description
GET /api/leaves/balance Get current leave balance per type
POST /api/leaves/apply Apply for leave with type, dates, reason
GET /api/leaves/history?year= Personal leave history
GET /api/leaves/team￾calendar?month=
Team leave calendar (manager)
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 11
GET /api/leaves/pending All pending approvals (manager/HR)
PATCH /api/leaves/:id/approve Approve leave request
PATCH /api/leaves/:id/reject Reject with reason
POST /api/leaves/encash Apply for leave encashment
GET /api/leaves/holidays Company holiday calendar
4.4 Payroll Endpoints
Method Endpoint Description
GET /api/payroll/payslip?month=&year= Employee: get payslip breakdown
GET /api/payroll/history List all past payslips
GET /api/payroll/payslip/:id/pdf Generate and download payslip PDF
POST /api/payroll/run Admin: trigger monthly payroll run
GET /api/payroll/summary?month= Admin: payroll cost summary
GET /api/payroll/tax-projection Employee: annual tax projection
POST /api/payroll/loans/apply Employee: apply for salary advance
GET /api/payroll/loans View loan balance and EMI schedule
4.5 Shift Endpoints
Method Endpoint Description
GET /api/shifts List all available shifts
GET /api/shifts/me Employee's current assigned shift
POST /api/shifts/change-request Request shift change
GET /api/shifts/change-requests HR: list all pending shift change requests
PATCH /api/shifts/change￾requests/:id/approve
HR approve/reject shift change
POST /api/shifts Admin: create new shift definition
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 12
5. Flutter Screens & Implementation
All 22+ screens with widget trees and BLoC states
5.1 All Screens List
# Screen Name Route Access
1 Splash / Onboarding /splash Public
2 Login /login Public
3 Biometric Setup /biometric-setup Employee
4 Home Dashboard /home Employee
5 Check-in Modal Bottom sheet from home Employee
6 Clock Out Confirmation Bottom sheet from home Employee
7 Attendance Log /attendance Employee
8 OT Report /attendance/ot Employee
9 Regularization Request /attendance/regularize Employee
10 Leaves /leaves Employee
11 Apply Leave /leaves/apply Employee
12 Team Leave Calendar /leaves/calendar Manager
13 Salary Breakup /payroll/payslip Employee
14 Loan & Advances /payroll/loans Employee
15 Employee Profile /profile/:id Employee/HR
16 Team View /teams Manager
17 Org Chart /org-chart Employee
18 Notifications / Alerts /alerts Employee
19 Shift Change Request /shifts/change Employee
20 Expense Claim /expenses/new Employee
21 Holiday Calendar /holidays Employee
22 HR Helpdesk /helpdesk Employee
23 Performance Dashboard /performance Employee/Manager
24 Social Wall /feed Employee
25 Settings & Logout /settings Employee
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 13
5.2 Home Screen Implementation (Flutter)
The home screen is the most critical screen. It loads user data, today's attendance state, and 
personal stats. Use BLoC for state management.
// home_bloc.dart
class HomeBloc extends Bloc<HomeEvent, HomeState> {
 final AttendanceRepository attendanceRepo;
 final EmployeeRepository employeeRepo;
 HomeBloc(this.attendanceRepo, this.employeeRepo) : super(HomeInitial()) {
 on<LoadHomeData>(_onLoadHome);
 on<ClockInRequested>(_onClockIn);
 on<ClockOutRequested>(_onClockOut);
 }
 Future<void> _onLoadHome(LoadHomeData event, Emitter emit) async {
 emit(HomeLoading());
 try {
 final employee = await employeeRepo.getProfile();
 final todayAttendance = await attendanceRepo.getToday();
 final monthStats = await attendanceRepo.getMonthStats();
 emit(HomeLoaded(employee, todayAttendance, monthStats));
 } catch (e) {
 emit(HomeError(e.toString()));
 }
 }
}
5.3 Clock-In Flow (Biometric + GPS)
1. User taps Clock In button on Home screen
2. App requests biometric authentication via local_auth plugin
3. On success, app requests device GPS location
4. GPS coordinates sent to /api/attendance/geofence/validate
5. If valid: proceed to Check-in confirmation bottom sheet
6. User confirms — POST /api/attendance/clock-in with location + timestamp
7. Response updates Home screen state via BLoC
8. Push notification sent to manager about employee check-in
// clock_in_service.dart
Future<AttendanceRecord> clockIn() async {
 // 1. Biometric auth
 final auth = await LocalAuthentication().authenticate(
 localizedReason: 'Authenticate to clock in',
 );
 if (!auth) throw BiometricException('Auth failed');
 // 2. Get GPS
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 14
 final pos = await Geolocator.getCurrentPosition(
 desiredAccuracy: LocationAccuracy.high,
 );
 // 3. Call API
 return await attendanceApi.clockIn(
 lat: pos.latitude, lng: pos.longitude,
 timestamp: DateTime.now().toIso8601String(),
 );
}
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 15
6. Backend Implementation (Node.js)
Key controllers, services, and cron jobs
6.1 Attendance Controller
// controllers/attendance.controller.js
exports.clockIn = async (req, res) => {
 const { lat, lng, timestamp } = req.body;
 const empId = req.user._id;
 // Check already clocked in today
 const existing = await Attendance.findOne({
 empId, date: dayjs().startOf('day').toDate()
 });
 if (existing?.clockIn?.time) {
 return res.status(400).json({ message: 'Already clocked in today' });
 }
 // Validate geofence
 const isValid = await geofenceService.validate(lat, lng, empId);
 
 // Create attendance record
 const record = await Attendance.create({
 empId, date: new Date(),
 clockIn: { time: new Date(timestamp), location: { lat, lng },
 method: 'biometric' },
 status: 'pending',
 geofenceValid: isValid,
 shift: req.user.shift,
 });
 // Notify manager
 await notificationService.sendToManager(empId, 'clock_in', record);
 res.status(201).json({ success: true, data: record });
};
6.2 OT Calculation Service
// services/ot.service.js
exports.calculateOT = async (attendance, shift) => {
 if (!attendance.clockOut) return 0;
 
 const shiftEnd = dayjs(shift.endTime, 'HH:mm');
 const clockOut = dayjs(attendance.clockOut.time);
 
 // OT = minutes worked beyond shift end
 const diffMinutes = clockOut.diff(shiftEnd, 'minute');
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 16
 
 if (diffMinutes <= (shift.graceMinutes || 0)) return 0;
 
 // Round to nearest 30 min
 return Math.floor(diffMinutes / 30) * 0.5;
};
6.3 Payroll Calculation Service
// services/payroll.service.js
exports.calculateMonthlyPayroll = async (empId, month, year) => {
 const employee = await Employee.findById(empId);
 const attendance = await getMonthAttendance(empId, month, year);
 const leaves = await getApprovedLeaves(empId, month, year);
 const workingDays = getWorkingDaysInMonth(month, year);
 const presentDays = attendance.filter(a => a.status === 'present').length;
 const lopDays = workingDays - presentDays - leaves.length;
 const { basic, allowances, hra } = employee.salary;
 const perDaySalary = (basic + allowances + hra) / workingDays;
 const lopDeduction = lopDays * perDaySalary;
 const otHours = attendance.reduce((sum, a) => sum + (a.otHours || 0), 0);
 const otPay = (basic / (workingDays * 8)) * otHours * 1.5; // 1.5x OT rate
 const pf = basic * 0.12; // 12% employee PF
 const esi = (basic + allowances) < 21000 ? (basic + allowances) * 0.0075 : 0;
 const tds = calculateTDS(basic * 12);
 const gross = basic + allowances + hra + otPay;
 const deductions = lopDeduction + pf + esi + tds;
 const net = gross - deductions;
 return { basic, allowances, hra, otPay, lopDeduction, pf, esi, tds,
 gross, deductions, net, presentDays, lopDays, otHours };
};
6.4 Scheduled Jobs (Cron)
Job Schedule Purpose
dailyOTCalculation Every day at 11:59 PM Calculate OT for all employees who 
clocked out
missingPunchAlert Every day at 11:00 AM Alert employees who forgot to clock in
monthlyPayrollRun 1st of each month at 6 AM Auto-run payroll for previous month
leaveBalanceReset January 1st annually Reset annual leave balances
birthdayAnniversaryAlert Every day at 9 AM Send celebration notifications
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 17
lopCalculation Last day of month Finalize LOP deductions before payroll
attendanceSummaryEmail Every Monday 8 AM Weekly summary email to managers
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 18
7. Complete Feature List (Keka-Parity)
All 60+ features mapped to modules with dev status
7.1 Attendance & Time
Feature Phase Flutter Widget API Endpoint
Biometric clock-in 
(fingerprint)
1 BiometricClockInButton POST /attendance/clock-in
Face ID clock-in 1 BiometricClockInButton POST /attendance/clock-in
Geofence GPS validation 1 GeofenceService POST /attendance/validate￾geofence
Clock-out with OT calc 1 ClockOutSheet POST /attendance/clock-out
Work-from-home punch 2 WFHPunchButton POST /attendance/clock￾in?mode=wfh
Attendance log by month 1 AttendanceListView GET /attendance/me
Status badges 
(Present/Absent/Half)
1 StatusBadge widget Embedded in attendance record
Attendance regularization 
request
1 RegularizeForm POST /attendance/regularize
OT report with daily 
breakdown
1 OTReportScreen GET /attendance/ot-report
Holiday calendar 1 HolidayCalendarScreen GET /leaves/holidays
Gamified attendance 
(streaks)
3 AttendanceStreakWidget GET /attendance/gamification
Shift board view 2 ShiftBoardScreen GET /shifts/board
Shift swap requests 2 ShiftSwapForm POST /shifts/swap-request
Auto late-mark with grace 
period
1 Backend cron CRON: daily-late-mark
7.2 Leave Management
Feature Phase Notes
All 7 leave types 1 Annual, Sick, Maternity, Paternity, Casual, 
Bereavement, Unpaid
Leave balance per type 1 Shown on home screen and leaves screen
Holiday-aware leave apply 1 System skips weekends and holidays in count
Team leave calendar 2 Manager view — who's on leave on which day
Leave approval workflow 1 Manager gets push notification + approves in Alerts
Leave encashment 2 Employee requests cash for unused annual leave
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 19
Comp-off management 2 Work on holiday → earn comp-off day
LOP automation 1 Auto deduct from payroll for unapproved absences
Leave history 1 All past leaves with status and reason
Leave policy admin config 2 Admin sets accrual rules, limits per type
7.3 Payroll
Feature Phase Notes
Payslip breakdown screen 1 Basic + Allowances + Bonus – Deductions = Net
PDF payslip generation 1 Download + Share from mobile
Salary structure builder (admin) 1 Define components per role or employee
Auto TDS / income tax 
calculation
1 Monthly TDS based on projected annual income
PF (Provident Fund) automation 1 12% employee + 12% employer, monthly
ESI calculation 1 0.75% employee for salary < 21,000
LWF (Labour Welfare Fund) 2 State-based, semi-annual deduction
Gratuity management 2 Auto-calculate based on service years
Loans & salary advances 2 Apply, track EMI, view balance
Form 16 generation 2 Annual tax certificate download
Reimbursement tracking 2 Expense claims linked to payroll
Payroll cost analytics (admin) 1 Dept-wise cost breakdown in admin dashboard
7.4 Core HR
Feature Phase Notes
Full employee profile 1 Personal, job, salary, documents, attendance tabs
Employee directory 1 Searchable list with contact, dept, role
Org chart (visual) 2 Interactive hierarchy tree
Onboarding task checklist 2 New joiner task list with deadlines
Exit management 2 Resignation flow, NOC, clearance, F&F settlement
Asset assignment 2 Assign laptop/phone, employee acknowledgment
Document management 2 Store offer letters, contracts, ID proofs
e-Signature support 3 Sign documents digitally in-app
Role-based access control 
(RBAC)
1 Granular permissions matrix per role
Bulk employee CSV upload 1 Admin uploads employee data via CSV
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 20
7.5 Performance Management
Feature Phase Notes
Performance review cycles 2 Quarterly/annual cycles with self + manager review
Goal / OKR setting 2 Employee sets goals, tracks progress
KPI tracking 2 Manager rates KPIs per employee
360-degree feedback 2 Peer, subordinate, and manager feedback
Continuous feedback 3 Anytime feedback — no wait for cycle
1:1 meeting notes 3 Manager-employee meeting record
Probation review form 2 Structured confirmation or extension decision
Appraisal workflow 3 Salary revision linked to performance rating
7.6 Employee Engagement
Feature Phase Notes
Social wall / company feed 2 Posts, announcements, updates by HR/admin
Peer praise & recognition 2 Employees praise each other publicly
Pulse surveys 2 HR sends quick surveys, real-time response analytics
Polls with analytics 2 Vote on decisions, see results
Birthday & anniversary alerts 3 Auto-send greetings to team
HR helpdesk ticketing 2 Employee raises issue, HR tracks resolution
Announcement push notifications 1 HR broadcasts push to all or specific dept
Celebrations (milestones) 3 Work anniversary, promotion announcements
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 21
8. Development Roadmap & Sprint Plan
Phase-by-phase delivery plan for the dev team
8.1 Phase 1 — Core MVP (Weeks 1–6)
Goal: Ship a working attendance + leave + payroll core that employees can use daily.
Week Backend (Node.js) Mobile (Flutter)
Week 
1
Project setup, auth (JWT + biometric), 
employee model, DB seed
Flutter project setup, BLoC structure, splash 
+ login screens
Week 
2
Clock-in/out API, geofence service, 
attendance model
Home screen, biometric clock-in, check-in 
bottom sheet, GPS integration
Week 
3
Attendance log API, OT calculation service, 
regularization
Attendance log screen, status badges, OT 
report screen
Week 
4
Leave APIs (apply, approve, balance), 
holiday calendar, LOP cron
Leaves screen, apply leave form, 
notifications screen
Week 
5
Payroll API, tax calc, payslip PDF gen, 
salary structure
Salary breakup screen, payslip download, 
employee profile
Week 
6
Shift management API, shift change flow, 
push notifications (FCM)
Shift change modal, team view screen, 
settings + logout
8.2 Phase 2 — Growth Features (Weeks 7–16)
Goal: Match Keka's core differentiators. Make managers and HR love it.
Week Features to build
Week 7–8 Team leave calendar, comp-off, WFH punch mode, holiday-aware apply
Week 9–
10
Expense management, travel requests, reimbursement approval workflow
Week 11–
12
Org chart, onboarding workflow, exit management, asset tracking
Week 13–
14
Form 16 generation, gratuity module, loans & salary advances
Week 15–
16
HR helpdesk ticketing, social wall, peer praise, announcement push
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 22
8.3 Phase 3 — Engagement & Intelligence (Weeks 17–28)
Goal: Make employees want to open the app every morning.
Week Features to build
Week 17–
19
Performance review cycles, OKR goal setting, KPI tracking
Week 20–
22
360-degree feedback, probation review forms, continuous feedback
Week 23–
25
Pulse surveys, polls, social wall (full), birthday/anniversary alerts
Week 26–
28
Gamified attendance (streaks + badges), punctuality leaderboard, attendance 
snail/smiley
8.4 Phase 4 — Enterprise Expansion (Month 7+)
Goal: Compete with Keka at enterprise level.
Module Features
Hiring / ATS Job posting, candidate pipeline, interview scheduling, offer letter, 
background check
Learning Management 
(LMS)
Course creation, video upload, assessments, mobile learning, skill mapping
Projects & Timesheets Project tracking, billable hours, milestone tracking, client billing, resource 
allocation
AI Features Anomaly detection in attendance, attrition risk prediction, smart scheduling 
suggestions
Integrations Slack, Microsoft Teams, QuickBooks, Zoho, biometric device APIs, payroll 
bank APIs
Multi-country Payroll Currency support, regional tax laws, multi-entity org structure
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 23
9. Flutter Setup & Key Dependencies
pubspec.yaml and project configuration
9.1 pubspec.yaml — Key Dependencies
name: workflow_app
description: HR & Attendance Management Platform
version: 1.0.0+1
dependencies:
 flutter:
 sdk: flutter
 # State Management
 flutter_bloc: ^8.1.3
 equatable: ^2.0.5
 # Networking
 dio: ^5.3.2
 retrofit: ^4.0.3
 # Local Storage / Cache
 hive: ^2.2.3
 hive_flutter: ^1.1.0
 shared_preferences: ^2.2.1
 # Biometric Auth
 local_auth: ^2.1.7
 # Location / Geofence
 geolocator: ^10.1.0
 google_maps_flutter: ^2.5.0
 # Push Notifications
 firebase_messaging: ^14.7.3
 flutter_local_notifications: ^16.1.0
 firebase_core: ^2.24.2
 # UI Components
 flutter_svg: ^2.0.9
 cached_network_image: ^3.3.0
 shimmer: ^3.0.0
 fl_chart: ^0.65.0
 table_calendar: ^3.0.9
 intl: ^0.18.1
 # File / PDF
 pdf: ^3.10.7
 open_file: ^3.3.2
 path_provider: ^2.1.1
 file_picker: ^6.1.1
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 24
 # Real-time
 socket_io_client: ^2.0.3+1
 # Utilities
 get_it: ^7.6.4 # Dependency injection
 go_router: ^12.1.1 # Navigation
 freezed: ^2.4.5 # Immutable models
 json_annotation: ^4.8.1
 envied: ^0.3.0 # Env variables
9.2 Node.js package.json — Key Dependencies
{
 "dependencies": {
 "express": "^4.18.2",
 "mongoose": "^8.0.0",
 "jsonwebtoken": "^9.0.2",
 "bcryptjs": "^2.4.3",
 "redis": "^4.6.10",
 "firebase-admin": "^12.0.0",
 "node-cron": "^3.0.3",
 "nodemailer": "^6.9.7",
 "multer": "^1.4.5",
 "aws-sdk": "^2.1502.0",
 "pdfkit": "^0.14.0",
 "dayjs": "^1.11.10",
 "joi": "^17.11.0",
 "helmet": "^7.1.0",
 "cors": "^2.8.5",
 "morgan": "^1.10.0",
 "compression": "^1.7.4",
 "express-rate-limit": "^7.1.5",
 "socket.io": "^4.6.1",
 "csv-parser": "^3.0.0",
 "fast-csv": "^4.3.6"
 },
 "devDependencies": {
 "jest": "^29.7.0",
 "supertest": "^6.3.3",
 "nodemon": "^3.0.2",
 "eslint": "^8.55.0"
 }
}
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 25
10. Security & Compliance
Auth, data protection, and statutory compliance
10.1 Authentication & Authorization
• JWT access tokens (15 min expiry) + refresh tokens (7 days) stored in HTTP-only 
cookies
• Biometric auth handled on-device via local_auth — no biometric data sent to server
• RBAC middleware validates role on every protected route: employee < manager < HR < 
admin
• OTP via SMS (Twilio) or email for password reset flows
• Rate limiting: 5 failed login attempts triggers 15-minute lockout
10.2 Data Security
• All passwords hashed with bcrypt (salt rounds: 12)
• MongoDB Atlas encryption at rest + TLS in transit
• AWS S3 bucket policies: signed URLs for document access (expire in 1 hour)
• Sensitive fields (salary, PAN, Aadhaar) encrypted with AES-256 before storage
• GDPR/PDPB compliant: employees can request data export or deletion
10.3 Statutory Compliance Features
Compliance Calculation Auto-generated
PF (Provident Fund) 12% of basic salary (employee) + 13.61% 
(employer)
Monthly challan report
ESI 0.75% employee + 3.25% employer (for 
salary < 21,000)
Monthly ESI report
LWF State-specific, semi-annual deduction State-wise report
TDS / Income Tax Monthly TDS based on projected annual 
income + declarations
Form 16 (annual), Form 24Q 
(quarterly)
Gratuity 15/26 × Basic × Years of service (after 5 
years)
Gratuity register
Bonus 8.33% of basic (Statutory Bonus Act) Bonus register
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 26
11. Web Admin Panel
React-based admin panel for HR and super-admins
11.1 Admin Panel Pages
Page Features
Analytics Dashboard KPI cards, attendance trend, dept hours, top employees, payroll cost
Employee Management List, add, edit, deactivate, bulk CSV upload, export CSV
Attendance Approval Regularization requests, bulk approve, date-range filter
Payroll Generation Run payroll, review, approve, publish payslips, compliance reports
Leave Management Approve/reject, leave balance edit, policy configuration
Shift Management Create/edit shifts, assign to employees, view shift board
Department Management Create org structure, assign HODs, manage locations
Role & Permissions Define custom roles, assign permission matrix
Reports Custom report builder, schedule email reports, export PDF/CSV
Asset Management Assign assets, track status, view acknowledgments
Helpdesk View and resolve employee tickets, assign to HR agents
Settings Company profile, logo, holidays, payroll config, integrations
11.2 Admin Panel Tech Stack
• Framework: React 18 + TypeScript
• State: Redux Toolkit + RTK Query for API caching
• UI: Tailwind CSS + shadcn/ui component library
• Charts: Recharts for analytics visualizations
• Tables: TanStack Table v8 for sortable/filterable employee tables
• Forms: React Hook Form + Zod validation
• PDF: react-pdf for payslip preview in browser
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 27
12. Testing Strategy
Unit, integration, and E2E testing for all layers
12.1 Backend Testing (Jest + Supertest)
Test Type Coverage Target Tools
Unit tests 90%+ for services (payroll calc, OT 
calc, tax calc)
Jest + jest-mock
Integration tests All API endpoints with test DB Supertest + MongoDB Memory 
Server
Cron job tests All 7 scheduled jobs with time mocking Jest fake timers
Load tests 1000 concurrent clock-ins under 500ms k6 / Artillery
12.2 Flutter Testing
Test Type Coverage Target Tools
Unit tests All BLoC classes and use cases flutter_test + bloc_test
Widget tests All screens rendered with mock data flutter_test + mockito
Golden tests Screenshot regression for key screens golden_toolkit
E2E tests Full clock-in → clock-out → payslip flow Patrol / integration_test
13. Deployment & DevOps
CI/CD pipeline, hosting, and infrastructure
13.1 Infrastructure
Component Service Notes
Backend API AWS EC2 (t3.medium) or 
Railway.app
Auto-scaling group for peak hours
Database MongoDB Atlas (M10 cluster) Automated backups, 3-node replica set
Cache Redis Cloud (30MB free / paid 
plan)
Session store + OTP + attendance state
File Storage AWS S3 Payslips, profile photos, documents
Push Notifications Firebase Cloud Messaging Free up to 1M messages/month
Maps Google Maps Platform Geofence validation, cost ~$5/1000 
requests
CDN CloudFront Static assets, faster app load globally
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 28
Monitoring Sentry (errors) + Datadog 
(metrics)
Real-time alerts, performance traces
13.2 CI/CD Pipeline (GitHub Actions)
9. Developer pushes to feature branch
10. GitHub Actions triggers: lint → unit tests → integration tests
11. PR merged to main → build Docker image → push to ECR
12. Staging deploy: auto-deploy to staging environment
13. QA sign-off → manual trigger → production deploy
14. Fastlane builds Flutter iOS + Android → uploads to TestFlight / Play Console
14. WorkFlow vs Keka — Feature Parity Tracker
Module Keka Features WorkFlow Phase 1 WorkFlow Phase 2 WorkFlow 
Phase 4
Attendance 14 features 10 features 14 features 14 
features
Leave 10 features 5 features 10 features 10 
features
Payroll 12 features 6 features 12 features 12 
features
Core HR 10 features 4 features 9 features 10 
features
Performance 8 features 0 features 4 features 8 features
Engagement 8 features 1 feature 5 features 8 features
Hiring / ATS 6 features 0 features 0 features 6 features
LMS / Projects 10 features 0 features 0 features 10 
features
TOTAL 78 features 26 features (33%) 54 features (69%) 78 
features 
(100%)
Competitive advantage: Keka's mobile app is widely criticized for missing features vs web. 
WorkFlow's mobile-first approach — where every feature works on mobile just as well as web — is 
the core differentiator.
WorkFlow HR & Attendance Platform Full Project Development idea
Confidential — WorkFlow Development Team Page 29
15. Summary & Next Steps
What to build first and how to get started
Immediate next steps for the development team
15. Set up GitHub monorepo with /mobile (Flutter), /backend (Node.js), /admin (React) 
folders
16. Configure MongoDB Atlas cluster, Redis Cloud instance, and Firebase project
17. Implement auth first — JWT + biometric — as every other feature depends on it
18. Build clock-in / clock-out as the first complete user journey end-to-end
19. Set up CI/CD pipeline from day one — prevents technical debt accumulation
20. Use the Keka app daily as the UX reference — compare every screen before shipping
21. Design all error and offline states before writing happy-path code
22. Run weekly demos with a real employee and manager to validate UX decisions
WorkFlow Development Team
Flutter + Node.js + MongoDB | Inspired by Keka HR
Build it right. Build it mobile-first.
