# JKADB — IMPLEMENTATION STATUS & MASTER TASK LIST

**Document Date:** August 22, 2026  
**Project:** Jammu Kashmir Awami Dast-o-Bazo  
**Status:** PARTIAL IMPLEMENTATION — SIGNIFICANT WORK REMAINING

---

## EXECUTIVE SUMMARY

**Current State:**
- ✅ Next.js + PostgreSQL + Drizzle ORM architecture in place
- ✅ Database schema comprehensively designed
- ✅ Basic citizen pages framework exists
- ✅ Basic admin pages framework exists
- ⚠️ Many features are **UI-ONLY without backend integration**
- ❌ AI integration incomplete (no real Grok API connection)
- ❌ Multiple critical features missing

**Work Remaining:**
- ~60% backend API endpoint implementation
- ~40% frontend-backend integration
- ~50% security hardening
- ~80% AI/Grok real integration
- ~30% professional UI/UX polish
- ~90% testing & validation

**Estimated Implementation Time:** 40-60 hours

---

## PHASE 1: CITIZEN FEATURES — STATUS

### 1.1 Citizen Homepage
**Status:** ⚠️ PARTIAL

**Exists:**
- Basic `CitizenHome.tsx` component
- Basic navigation structure
- Some placeholder UI

**Missing:**
- [ ] Professional hero with "ہمارا کشمیر، ہماری پہچان" styling
- [ ] Proper startup animation (Open Hand)
- [ ] Quick action cards connected to actual routes
- [ ] Announcements integration
- [ ] Theme switching implementation
- [ ] Language switching (English/Urdu RTL)
- [ ] Proper mobile responsiveness

**Task:** Rebuild homepage with professional design system

---

### 1.2 Complaint Submission
**Status:** ⚠️ PARTIAL

**Exists:**
- `/complaint/submit` page
- Basic form component

**Missing:**
- [ ] Multi-step form UI (5 steps)
- [ ] Phone number validation
- [ ] CNIC validation
- [ ] Dynamic location filtering (District → Tehsil → UC → PO)
- [ ] Backend endpoint for location cascading
- [ ] Evidence upload with validation
- [ ] File security (server-side validation, malware check)
- [ ] Draft save/restore functionality
- [ ] Review screen before submission
- [ ] Complaint ID generation (JKADB-2026-XXXXX)
- [ ] Database persistence verification
- [ ] Success screen
- [ ] Urdu support

**API Needed:**
- `POST /api/complaints/submit` — ✅ EXISTS but needs verification
- `GET /api/locations/tehsils?districtId=X` — needs implementation
- `GET /api/locations/union-councils?tehsilId=X` — needs implementation
- `GET /api/locations/post-offices?ucId=X` — ❌ MISSING
- `POST /api/complaints/draft` — ❌ MISSING
- `GET /api/complaints/draft/:token` — ❌ MISSING
- `DELETE /api/complaints/draft/:token` — ❌ MISSING

**Database Tables Needed:**
- `post_offices` table — ❌ MISSING from schema
- Need to add `post_offices` entity

**Task:** Implement complete multi-step complaint submission with real backend

---

### 1.3 Complaint Tracking
**Status:** ⚠️ PARTIAL

**Exists:**
- Basic tracking page placeholder

**Missing:**
- [ ] Complaint ID search
- [ ] Phone verification
- [ ] Secure tracking (IDOR protection)
- [ ] Status timeline display
- [ ] Current status badge with color coding
- [ ] SLA progress indicator
- [ ] Messages display (citizen-facing only)
- [ ] Attachments display
- [ ] Resolution display
- [ ] Feedback submission integration
- [ ] Reopen request button

**API Needed:**
- `GET /api/complaints/track/:complaintId` — ❌ MISSING
- `POST /api/complaints/track/verify` — ❌ MISSING
- Update tracking security

**Task:** Implement secure complaint tracking with timeline

---

### 1.4 Citizen Notifications
**Status:** ❌ MISSING

**Missing:**
- [ ] Notification center page
- [ ] Real-time notification updates
- [ ] Unread badge
- [ ] Notification types implemented:
  - Complaint Submitted
  - Complaint Accepted
  - Complaint Declined
  - Admin Response
  - Status Changed
  - Officer Assigned
  - Request More Information
  - Complaint Resolved
  - Complaint Reopened
  - Announcement
  - Quick Alert

**API Needed:**
- `GET /api/citizen/notifications` — ❌ MISSING
- `PATCH /api/citizen/notifications/:id/read` — ❌ MISSING
- `DELETE /api/citizen/notifications/:id` — ❌ MISSING

**Database Schema:**
- Citizen notification tracking needed (notifications table tracks admin recipients but not citizens)

**Task:** Implement citizen notification system with real delivery tracking

---

### 1.5 Announcements (Citizen View)
**Status:** ⚠️ PARTIAL

**Exists:**
- `/announcements` page
- Basic API route exists

**Missing:**
- [ ] Published announcements display
- [ ] Popup announcement detection & display
- [ ] Scheduling enforcement (show after publishAt)
- [ ] Expiration enforcement (hide after expiresAt)
- [ ] Image banner rendering
- [ ] Urdu support
- [ ] Mobile responsive cards

**API Status:**
- `GET /api/announcements` exists but needs filtering

**Task:** Connect announcements to real database with proper scheduling

---

### 1.6 Help / FAQ
**Status:** ⚠️ PARTIAL

**Exists:**
- `/help` page
- FAQ database schema exists

**Missing:**
- [ ] FAQ display from database
- [ ] Category filtering
- [ ] Search functionality
- [ ] Urdu support
- [ ] Professional accordion UI
- [ ] AI integration hints

**API Needed:**
- `GET /api/faq` exists but verify full functionality

**Task:** Implement professional FAQ display with categories

---

### 1.7 Settings Page
**Status:** ❌ MISSING

**Missing:**
- [ ] Settings page with gear icon
- [ ] Theme toggle (dark/light)
- [ ] Language toggle (English/Urdu)
- [ ] Admin login link (Settings → Admin → Admin Login)
- [ ] Personal preference storage
- [ ] Accessibility settings

**Task:** Create settings page with theme/language controls and admin login access

---

### 1.8 AI Assistant (Citizen)
**Status:** ❌ CRITICAL — NOT IMPLEMENTED

**Missing:**
- [ ] Floating bubble component
- [ ] Chat window UI
- [ ] Real message sending to backend
- [ ] Real Grok API integration (NOT mock!)
- [ ] Temporary session context
- [ ] Conversation clearing on close/reset
- [ ] NO permanent storage of citizen AI chats
- [ ] Language support (English/Urdu with RTL)
- [ ] Rate limiting
- [ ] Error handling when AI unavailable
- [ ] Creator identity in responses

**API Needed:**
- `POST /api/ai/chat` — ❌ MISSING
- `POST /api/ai/reset` — ❌ MISSING
- `GET /api/ai/health` — ❌ MISSING

**Environment Setup Needed:**
- GROK_API_KEY configuration
- AI_PROVIDER setting

**Critical:** Grok API key MUST NEVER be exposed client-side

**Task:** Build complete AI assistant with real Grok backend integration

---

## PHASE 2: ADMIN FEATURES — STATUS

### 2.1 Admin Authentication
**Status:** ⚠️ PARTIAL

**Exists:**
- Admin login page exists
- Basic authentication API
- Session management

**Missing:**
- [ ] Move admin login to Settings → Admin → Admin Login
- [ ] Remove direct homepage admin button
- [ ] Password hashing verification (bcryptjs)
- [ ] Session security (secure cookies)
- [ ] Rate limiting on login attempts
- [ ] Brute-force protection
- [ ] Account lockout after N failed attempts
- [ ] Force password change on first login
- [ ] Password reset flow
- [ ] Session expiration
- [ ] Logout functionality

**API Status:**
- `POST /api/admin/login` exists
- `GET /api/admin/me` exists
- `POST /api/admin/logout` exists

**Missing APIs:**
- `POST /api/admin/forgot-password` — ❌ MISSING
- `POST /api/admin/reset-password` — ❌ MISSING
- `POST /api/admin/change-password` — ❌ MISSING

**Task:** Strengthen authentication with security hardening

---

### 2.2 Multiple Admin Accounts
**Status:** ❌ MISSING

**Missing:**
- [ ] Admin user creation interface
- [ ] Role assignment (super_admin, district_admin, reviewer, complaint_officer)
- [ ] Admin editing/disabling
- [ ] Password reset by super admin
- [ ] Force password change
- [ ] Account status management
- [ ] Last login tracking
- [ ] Activity audit trail
- [ ] Session revocation

**API Needed:**
- `POST /api/admin/users` — Create admin
- `GET /api/admin/users` — List admins
- `PATCH /api/admin/users/:id` — Update admin
- `DELETE /api/admin/users/:id` — Disable admin (soft delete)
- `POST /api/admin/users/:id/reset-password` — Reset password
- `POST /api/admin/sessions/:id/revoke` — Revoke sessions

**Permission Checks Needed:**
- Only super_admin can create other admins
- Only super_admin can change roles
- Users can only see data they have permission for

**Task:** Implement admin management system with RBAC

---

### 2.3 Role-Based Access Control (RBAC)
**Status:** ⚠️ PARTIAL

**Schema Exists:**
- `user_role` enum defined
- Roles: super_admin, district_admin, reviewer, complaint_officer, citizen

**Missing:**
- [ ] Granular permissions system
- [ ] Permission database schema
- [ ] Admin-to-permission mapping
- [ ] Server-side permission enforcement on ALL endpoints
- [ ] Permission checking middleware
- [ ] UI element hiding based on permissions (non-security)
- [ ] Permissions UI in admin panel
- [ ] Audit logging of permission changes

**Permissions Needed:**
- View complaints
- Search complaints
- View CNIC
- Reply to complaints
- Assign complaints
- Reassign complaints
- Transfer complaints
- Change complaint status
- Change priority
- Resolve complaints
- Reopen complaints
- Export data
- Manage admins
- Manage officers
- Manage departments
- Manage categories
- Manage announcements
- View analytics
- Manage reports
- View audit logs
- Manage system settings

**Database Schema Needed:**
- `permissions` table
- `admin_permissions` table (admin → permission mapping)
- `role_permissions` table (role → permission mapping)

**Task:** Build complete permissions system with database and enforcement

---

### 2.4 Request Dashboard
**Status:** ⚠️ PARTIAL

**Exists:**
- `/admin/dashboard` page
- `/admin/complaints` page

**Missing:**
- [ ] Request-first layout (not just dashboard)
- [ ] Real counts from database:
  - Total
  - New
  - Unread
  - Read
  - Answered
  - Unanswered
  - Pending
  - Accepted
  - Declined
  - Resolved
  - Critical
  - Overdue
- [ ] Clickable cards that filter requests
- [ ] Request list by status
- [ ] Requests sorted properly
- [ ] Real-time or frequent updates
- [ ] SLA status indication
- [ ] Priority highlighting

**API Needed:**
- `GET /api/admin/dashboard/counts` — ✅ EXISTS but verify correctness
- Endpoint should return all above counts
- Must filter by admin's permission scope

**Database Queries Needed:**
- Count queries for each status
- Permission-aware filtering
- Date-based filtering

**Task:** Ensure dashboard queries return REAL database counts, not mock data

---

### 2.5 Request Management Center
**Status:** ⚠️ PARTIAL

**Exists:**
- `/admin/complaints` page
- Basic table structure

**Missing:**
- [ ] Multiple view tabs:
  - All Requests
  - New Requests
  - Unread Requests
  - Read Requests
  - Answered Requests
  - Unanswered Requests
  - Pending Requests
  - Accepted Requests
  - Declined Requests
  - Resolved Requests
  - Reopened Requests
  - Urgent Requests
  - Critical Requests
  - Overdue Requests
  - Archived Requests
- [ ] Each tab shows REAL count from database
- [ ] Clicking tab filters request list
- [ ] Professional table layout
- [ ] Server-side pagination
- [ ] Per-page selection (10, 25, 50)
- [ ] Request detail modal/page

**API Needed:**
- `GET /api/admin/complaints?status=NEW&limit=25&offset=0` — ✅ EXISTS
- Verify filtering by status works
- Verify pagination works
- Verify real database counts

**Task:** Verify all request view tabs return real database data

---

### 2.6 Request Search & Filters
**Status:** ⚠️ PARTIAL

**Exists:**
- Basic search field in complaints page

**Missing:**
- [ ] Search by:
  - Request ID
  - Complaint ID
  - Citizen Name
  - Father Name
  - Phone
  - Email
  - CNIC (if authorized)
  - Category
  - Subcategory
  - District
  - Tehsil
  - Union Council
  - Post Office
  - Status
  - Priority
  - Department
  - Officer
  - Date range
  - Keywords in description
- [ ] Advanced filter panel
- [ ] Multi-select filters (can combine)
- [ ] Date range picker
- [ ] Clear Filters button
- [ ] Filter persistence (URL params)
- [ ] Search highlighting

**API Needed:**
- `GET /api/admin/complaints/search?q=...&filters=...` — Needs implementation

**Database Query Optimization Needed:**
- Full-text search indexes
- Filter query optimization
- Permission-aware search

**Task:** Implement advanced search and multi-filter system

---

### 2.7 Request Sorting
**Status:** ❌ MISSING

**Missing:**
- [ ] Sort by:
  - Newest
  - Oldest
  - Recently Updated
  - Highest Priority
  - SLA Deadline
  - Most Overdue
  - Category
  - Status
- [ ] Click column header to sort
- [ ] Sort direction toggle (↑↓)
- [ ] Default sort preservation

**API Needed:**
- Update `/api/admin/complaints` to support sort parameter
- `GET /api/admin/complaints?sort=created_at&order=DESC`

**Task:** Add sorting to request list

---

### 2.8 Request Detail Page
**Status:** ⚠️ PARTIAL

**Exists:**
- `/admin/complaints/[id]` page structure

**Missing:**
- [ ] Complete request information display:
  - Complaint/Application ID
  - Date/Time
  - Citizen Name
  - Father Name
  - Phone
  - Email
  - Masked CNIC
  - District
  - Tehsil
  - Union Council
  - Post Office
  - More Detail
  - Category
  - Subcategory
  - Description
  - Attachments
  - Priority
  - Current Status
  - Department
  - Officer
  - SLA info
  - Timeline
  - Status History
  - Messages
  - Internal Notes
  - Resolution info
  - Feedback info
  - Audit information
- [ ] Professional two-column layout (main + sidebar)
- [ ] All data read from database
- [ ] No hard-coded information

**API Needed:**
- `GET /api/admin/complaints/:id` — ✅ EXISTS
- Verify it returns all required fields
- Verify permission checks

**Task:** Verify request detail displays complete real data

---

### 2.9 Admin Response System
**Status:** ⚠️ PARTIAL

**Exists:**
- Message/response structure exists

**Missing:**
- [ ] Rich text editor for responses
- [ ] Response templates dropdown
- [ ] Save as draft
- [ ] Send response button
- [ ] Edit response (if permitted)
- [ ] Delete response (if permitted)
- [ ] Request more information action
- [ ] Copy/paste/select functionality
- [ ] Attachment support in responses
- [ ] Timestamp tracking

**API Needed:**
- `POST /api/admin/complaints/:id/reply` — ✅ EXISTS
- `PATCH /api/admin/complaints/:id/message/:msgId` — ❌ MISSING
- `DELETE /api/admin/complaints/:id/message/:msgId` — ❌ MISSING

**Task:** Enhance response UI and verify backend

---

### 2.10 Accept / Decline System
**Status:** ⚠️ PARTIAL

**Exists:**
- Basic structure

**Missing:**
- [ ] Accept button with confirmation
- [ ] Decline button with required reason
- [ ] Status update to database
- [ ] Status history entry
- [ ] Audit logging
- [ ] Notification to citizen
- [ ] Notification to officers
- [ ] Prevent invalid transitions
- [ ] Timestamp recording

**API Needed:**
- `POST /api/admin/complaints/:id/accept` — ❌ MISSING
- `POST /api/admin/complaints/:id/decline` — ❌ MISSING

**Database Transaction Needed:**
- Atomic update of status, history, audit log, notification

**Task:** Implement complete accept/decline workflow with side effects

---

### 2.11 Response Templates
**Status:** ❌ MISSING

**Missing:**
- [ ] Template management page
- [ ] Create template button
- [ ] Template list with actions
- [ ] Edit template
- [ ] Delete template
- [ ] English/Urdu support
- [ ] Use template dropdown in reply
- [ ] Insert template with variables
- [ ] Preview template

**API Needed:**
- `GET /api/admin/response-templates` — ❌ MISSING
- `POST /api/admin/response-templates` — ❌ MISSING
- `PATCH /api/admin/response-templates/:id` — ❌ MISSING
- `DELETE /api/admin/response-templates/:id` — ❌ MISSING

**Database Schema Needed:**
- `response_templates` table

**Task:** Add response templates feature with database

---

### 2.12 Request Edit History
**Status:** ❌ MISSING

**Missing:**
- [ ] Show edit history for changeable fields:
  - Category
  - Subcategory
  - Priority
  - Department
  - Officer
  - Status
  - Location data
  - Internal notes
- [ ] Original Value → New Value → Changed By → Date/Time
- [ ] Immutable edit history
- [ ] Audit trail integration

**Database Schema Needed:**
- `request_edit_history` table with:
  - complaintId
  - fieldName
  - previousValue
  - newValue
  - changedBy (admin ID)
  - changedAt
  - reason

**API Needed:**
- Track all edits through audit system
- `GET /api/admin/complaints/:id/edit-history` — ❌ MISSING

**Task:** Implement edit history tracking

---

### 2.13 Messaging System
**Status:** ⚠️ PARTIAL

**Exists:**
- Messages table structure

**Missing:**
- [ ] Secure message thread display
- [ ] Citizen can see officer replies only
- [ ] Admin/officer internal notes NEVER visible to citizen
- [ ] Message timestamps
- [ ] Read/unread status
- [ ] Attachment support in messages
- [ ] Message search within complaint
- [ ] Quote/reply functionality
- [ ] Mark as read

**API Needed:**
- Update message endpoints to separate citizen-visible from internal

**Permission Checks Needed:**
- Filter messages based on message_type:
  - Citizens see: citizen, officer, admin messages (NOT internal_note)
  - Admins see: ALL messages including internal_note
  - System messages shown to all

**Task:** Implement message filtering with permission checks

---

### 2.14 Resolution & Feedback
**Status:** ⚠️ PARTIAL

**Exists:**
- Feedback schema exists
- Basic feedback form

**Missing:**
- [ ] Resolution description entry
- [ ] Resolution date setting
- [ ] Responsible officer recording
- [ ] Official response upload
- [ ] Optional evidence upload
- [ ] Status transition to "resolved"
- [ ] Citizen notification
- [ ] Display resolution to citizen
- [ ] Feedback rating (Yes/Partially/No)
- [ ] Feedback comment
- [ ] Reopen request option from feedback
- [ ] Timeline update

**API Needed:**
- `POST /api/admin/complaints/:id/resolve` — ❌ MISSING
- `POST /api/complaints/:id/feedback` — ✅ EXISTS but verify

**Task:** Implement complete resolution and feedback workflow

---

### 2.15 Reopen System
**Status:** ⚠️ PARTIAL

**Exists:**
- Status "reopened" exists in enum

**Missing:**
- [ ] Reopen button in resolved complaints
- [ ] Reason for reopen
- [ ] Requesting party tracking
- [ ] Previous resolution preserved
- [ ] New status set appropriately
- [ ] Reassign to officer
- [ ] New SLA calculation
- [ ] Citizen notification
- [ ] Complete history preservation
- [ ] Audit logging

**API Needed:**
- `POST /api/admin/complaints/:id/reopen` — ❌ MISSING

**Database Fields Needed:**
- reopen_count (complaints table)
- previous_resolution_id

**Task:** Implement reopen workflow

---

### 2.16 Duplicate Detection
**Status:** ❌ MISSING

**Missing:**
- [ ] Automatic duplicate detection on submission
- [ ] Similar description matching
- [ ] Similar location matching
- [ ] Same category matching
- [ ] Similar time matching
- [ ] Suggest possible duplicates to admin
- [ ] Link duplicates
- [ ] Merge duplicates (optional)
- [ ] Mark as duplicate without deleting
- [ ] Show linked complaints in request detail

**Algorithm Needed:**
- Fuzzy text matching
- Location distance/similarity
- Time window (e.g., within 1 hour)
- Scoring system to rank likelihood

**API Needed:**
- `POST /api/admin/complaints/:id/check-duplicates` — ❌ MISSING
- `POST /api/admin/complaints/:id/mark-duplicate/:duplicateId` — ❌ MISSING

**Database Schema Needed:**
- `duplicate_links` table
- `complaint_matches` table (for detection)

**Task:** Implement duplicate detection system

---

### 2.17 Departments Management
**Status:** ⚠️ PARTIAL

**Exists:**
- Departments schema
- Basic department page

**Missing:**
- [ ] Create department form
- [ ] Edit department
- [ ] Disable/enable toggle
- [ ] Assign officers to department
- [ ] Configure SLA
- [ ] Assign responsible areas
- [ ] Performance metrics
- [ ] Department-category mapping
- [ ] Audit department changes

**API Needed:**
- `POST /api/admin/departments` — ✅ EXISTS
- `PATCH /api/admin/departments/:id` — ❌ MISSING
- `DELETE /api/admin/departments/:id` — ❌ MISSING (soft delete)
- `POST /api/admin/departments/:id/assign-officers` — ❌ MISSING

**Database Schema Needed:**
- `department_officers` junction table
- `department_categories` junction table

**Task:** Complete department management system

---

### 2.18 Officers Management
**Status:** ⚠️ PARTIAL

**Exists:**
- Officers page structure

**Missing:**
- [ ] Officer list with workload
- [ ] Create officer account
- [ ] Edit officer information
- [ ] Assign to department
- [ ] Assign to area
- [ ] Disable/enable
- [ ] View assigned complaints
- [ ] View officer performance:
  - Open cases
  - Resolved cases
  - Average resolution time
  - SLA compliance %
  - Reopen rate
- [ ] Workload visualization
- [ ] Activity audit trail

**API Needed:**
- `POST /api/admin/officers` — ❌ MISSING
- `GET /api/admin/officers` — ✅ EXISTS
- `PATCH /api/admin/officers/:id` — ❌ MISSING
- `GET /api/admin/officers/:id/performance` — ❌ MISSING
- `GET /api/admin/officers/:id/complaints` — ❌ MISSING

**Database Schema Needed:**
- `officer_areas` junction table
- `officer_performance` materialized view/table

**Task:** Implement complete officers management

---

### 2.19 Assignments Management
**Status:** ⚠️ PARTIAL

**Exists:**
- Basic assignment fields

**Missing:**
- [ ] Assign complaint modal/form
- [ ] Select department first
- [ ] Select officer from department
- [ ] Reason for assignment
- [ ] SLA configuration
- [ ] Reassign functionality
- [ ] Transfer to different department
- [ ] Auto-assignment rules (if configured)
- [ ] Workload balancing
- [ ] Audit logging

**API Needed:**
- `POST /api/admin/complaints/:id/assign` — ❌ MISSING
- `POST /api/admin/complaints/:id/reassign` — ❌ MISSING
- `POST /api/admin/complaints/:id/transfer` — ❌ MISSING
- `GET /api/admin/officers/:id/workload` — ❌ MISSING

**Task:** Implement assignment system with workload awareness

---

### 2.20 SLA Management
**Status:** ⚠️ PARTIAL

**Exists:**
- SLA schema exists
- Basic SLA page

**Missing:**
- [ ] SLA configuration per category
- [ ] SLA configuration per department
- [ ] SLA configuration per priority
- [ ] Override SLA for specific complaint
- [ ] SLA deadline display
- [ ] SLA progress bar
- [ ] Time remaining indicator
- [ ] Approaching deadline warning (visual)
- [ ] Overdue highlighting
- [ ] SLA extension with reason
- [ ] Audit SLA changes

**API Needed:**
- `GET /api/admin/sla-configurations` — ❌ MISSING
- `POST /api/admin/sla-configurations` — ❌ MISSING
- `PATCH /api/admin/complaints/:id/sla/extend` — ❌ MISSING

**Database Schema Needed:**
- Add `sla_extended` field to complaints
- Add `sla_extension_reason` field

**Task:** Implement complete SLA tracking and management

---

### 2.21 Escalation System
**Status:** ⚠️ PARTIAL

**Exists:**
- Escalation schema exists

**Missing:**
- [ ] Automatic escalation on SLA breach
- [ ] Manual escalation option
- [ ] Escalation levels:
  - Level 1: Complaint Officer → Reviewer
  - Level 2: Reviewer → District Admin
  - Level 3: District Admin → Super Admin
- [ ] Escalation reason/notes
- [ ] Automatic notification of escalated level
- [ ] Escalation audit trail
- [ ] Display escalation status in request
- [ ] Configure escalation timing per priority
- [ ] Override escalation manual

**API Needed:**
- `POST /api/admin/complaints/:id/escalate` — ❌ MISSING
- `GET /api/admin/escalations` — ❌ MISSING
- Background job for automatic escalation

**Database Schema Additions:**
- `escalation_triggers` table (config)
- Add escalation fields to complaints

**Background Job Needed:**
- Hourly/half-hourly check for SLA breaches
- Automatic escalation trigger
- Notification sending

**Task:** Implement complete escalation system with background jobs

---

### 2.22 Announcements Management
**Status:** ⚠️ PARTIAL

**Exists:**
- Announcements table and basic page

**Missing:**
- [ ] Create announcement form
- [ ] Edit announcement
- [ ] Delete announcement (archive)
- [ ] English/Urdu title and description
- [ ] Banner image upload
- [ ] Start date selection
- [ ] End date selection
- [ ] Priority setting
- [ ] Popup toggle
- [ ] Draft/scheduled/published/expired status
- [ ] Preview before publish
- [ ] Schedule announcement
- [ ] Archive old announcements
- [ ] Publish immediately option

**API Needed:**
- `POST /api/admin/announcements` — ❌ MISSING
- `PATCH /api/admin/announcements/:id` — ❌ MISSING
- `DELETE /api/admin/announcements/:id` — ❌ MISSING (soft delete)
- `GET /api/admin/announcements/:id/preview` — ❌ MISSING

**Task:** Implement announcement management UI and backend

---

### 2.23 Quick Alerts
**Status:** ❌ MISSING

**Missing:**
- [ ] Quick Alerts management page
- [ ] Create quick alert form
- [ ] Edit quick alert
- [ ] Activate/Deactivate toggle
- [ ] Schedule alert (start/end time)
- [ ] Alert message (title + text)
- [ ] Priority level
- [ ] Display mode (banner/popup/badge)
- [ ] Currently active alert display
- [ ] Preview
- [ ] Audit logging

**Database Schema Needed:**
- `quick_alerts` table

**API Needed:**
- `GET /api/admin/quick-alerts` — ❌ MISSING
- `POST /api/admin/quick-alerts` — ❌ MISSING
- `PATCH /api/admin/quick-alerts/:id` — ❌ MISSING
- `GET /api/public/quick-alerts/active` — ❌ MISSING (for citizen display)

**Citizen Display Needed:**
- Show active quick alert at top of page
- Show popup if configured
- Show badge in navigation

**Task:** Add Quick Alerts feature with database and UI

---

### 2.24 Public Contact Information
**Status:** ❌ MISSING

**Missing:**
- [ ] Contact Information management page
- [ ] Add/edit/delete contact entries
- [ ] Enable/disable contacts
- [ ] Reorder contacts
- [ ] Contact types:
  - Main phone
  - Alternative phone
  - Email
  - Support email
  - Support phone
  - Office address
  - WhatsApp
  - Emergency contact
  - Website/social links
- [ ] Audit all changes

**Database Schema Needed:**
- `public_contacts` table

**API Needed:**
- `GET /api/admin/public-contacts` — ❌ MISSING
- `POST /api/admin/public-contacts` — ❌ MISSING
- `PATCH /api/admin/public-contacts/:id` — ❌ MISSING
- `GET /api/public/contacts` — ❌ MISSING (for citizen display)

**Citizen Display Needed:**
- Show on Help page
- Show in footer
- Show in About page

**Task:** Add public contact management

---

### 2.25 Notifications (Admin)
**Status:** ⚠️ PARTIAL

**Exists:**
- Notifications schema
- Basic notification structure

**Missing:**
- [ ] Admin notification center UI
- [ ] Notification bell with unread count
- [ ] Notification list with tabs:
  - All
  - Unread
  - Critical
  - System
  - Requests
  - SLA
  - AI
- [ ] Mark as read
- [ ] Delete notification
- [ ] Notification click → navigate to relevant complaint
- [ ] Real-time updates (polling or WebSocket)
- [ ] Notification types implemented:
  - New Request
  - New Unread Request
  - Critical Request
  - New Citizen Reply
  - SLA Warning
  - Overdue Request
  - Escalation
  - New Admin Account
  - System Alert
  - AI API Error
  - Failed Notification

**API Needed:**
- `GET /api/admin/notifications` — ✅ EXISTS
- `PATCH /api/admin/notifications/:id/read` — ❌ MISSING
- `DELETE /api/admin/notifications/:id` — ❌ MISSING
- `GET /api/admin/notifications/unread-count` — ❌ MISSING

**Task:** Complete admin notification center with real-time updates

---

## PHASE 3: ANALYTICS & REPORTING

### 3.1 Analytics Dashboard
**Status:** ❌ MISSING

**Missing:**
- [ ] Analytics page with filters:
  - Date range
  - District
  - Tehsil
  - Department
  - Category
- [ ] Request statistics:
  - Received
  - Resolved
  - Pending
  - Overdue
  - Reopened
- [ ] Performance metrics:
  - Average resolution time
  - SLA compliance %
  - Reopen rate
- [ ] Distribution charts:
  - By category
  - By district
  - By department
  - By status
- [ ] Citizen satisfaction:
  - Feedback ratings
  - Satisfaction %
- [ ] AI metrics:
  - Total requests
  - Success rate
  - Response time
- [ ] Charts/graphs (recharts)

**API Needed:**
- `GET /api/admin/analytics/requests?filters=...` — ❌ MISSING
- `GET /api/admin/analytics/performance?filters=...` — ❌ MISSING
- `GET /api/admin/analytics/distribution?filters=...` — ❌ MISSING
- `GET /api/admin/analytics/satisfaction?filters=...` — ❌ MISSING
- `GET /api/admin/analytics/ai-metrics?filters=...` — ❌ MISSING

**Database Queries Needed:**
- Aggregation queries with filtering
- Performance optimization (materialized views or caching)

**Task:** Implement analytics dashboard with real database queries

---

### 3.2 Reports & Export
**Status:** ❌ MISSING

**Missing:**
- [ ] Report builder UI
- [ ] Select report type:
  - Complaint reports
  - Department reports
  - Officer reports
  - Location reports
  - SLA reports
  - Feedback reports
- [ ] Configure report:
  - Date range
  - Filters
  - Column selection
  - Sort order
- [ ] Export formats:
  - CSV
  - Excel
  - PDF
- [ ] Download report
- [ ] Email report (optional)
- [ ] Schedule report (optional)
- [ ] Saved reports (optional)

**API Needed:**
- `POST /api/admin/reports/generate` — ❌ MISSING
- `POST /api/admin/reports/export` — ❌ MISSING

**Library Needed:**
- csv library for CSV export
- xlsx library for Excel export
- html2pdf or similar for PDF export

**Permission Checks Needed:**
- Sensitive exports require audit logging
- Certain users cannot export CNIC data

**Task:** Implement report builder with export functionality

---

### 3.3 Audit Logs
**Status:** ⚠️ PARTIAL

**Exists:**
- Audit log schema
- Basic API

**Missing:**
- [ ] Audit log viewer UI
- [ ] Filter by:
  - Actor (admin)
  - Action
  - Target type
  - Date range
  - IP address (optional)
- [ ] Display:
  - Timestamp
  - Admin name/role
  - Action
  - Target
  - Previous value
  - New value
- [ ] Immutable logs (cannot delete)
- [ ] Search within logs
- [ ] Export audit logs

**API Needed:**
- `GET /api/admin/audit-logs?filters=...` — ✅ EXISTS
- Verify filtering and sorting

**Permission Checks Needed:**
- Most users cannot view audit logs
- Only super admin and specific reviewers

**Database Optimization:**
- Index on actor_id, created_at, action
- Archive old logs

**Task:** Implement audit log viewer UI and verify permissions

---

## PHASE 4: SYSTEM & SECURITY

### 4.1 System Health
**Status:** ❌ MISSING

**Missing:**
- [ ] System Health page
- [ ] Status indicators:
  - Database connection
  - Backend API
  - AI/Grok service
  - File storage
  - Notification service
  - Background jobs
  - Backups
  - API uptime
- [ ] Last successful backup time
- [ ] Application error count
- [ ] Basic uptime tracking
- [ ] Status refresh button
- [ ] Status history

**API Needed:**
- `GET /api/admin/system-health` — ❌ MISSING

**Health Checks Needed:**
- Database ping
- Grok API test
- Storage accessibility
- Notification service test
- Background job queue status

**Task:** Implement system health monitoring

---

### 4.2 Background Jobs
**Status:** ❌ MISSING

**Missing:**
- [ ] Background job queue (Bull or similar)
- [ ] Jobs needed:
  - SLA checks (hourly/half-hourly)
  - Automatic escalation
  - Scheduled announcements
  - Quick alert scheduling
  - Image processing/compression
  - File scanning
  - Report generation
  - Notification sending
  - Cleanup of old data
  - Database backups
- [ ] Job monitoring page
- [ ] Retry logic
- [ ] Failure handling

**Library Needed:**
- Bull or BullMQ for job queue
- Redis for queue storage

**Task:** Set up background job system

---

### 4.3 Database Backups
**Status:** ❌ MISSING

**Missing:**
- [ ] Automated PostgreSQL backups
- [ ] Backup retention policy
- [ ] Backup verification
- [ ] Restore procedure documentation
- [ ] Disaster recovery plan
- [ ] Backup storage (separate from main DB)
- [ ] Restore testing
- [ ] Backup monitor in System Health

**Infrastructure Needed:**
- PostgreSQL backup script
- Backup storage location
- Scheduled cron job

**Task:** Set up automated backups and recovery

---

### 4.4 Data Retention Policies
**Status:** ❌ MISSING

**Missing:**
- [ ] Define retention periods for:
  - Complaints (likely permanent)
  - Messages (permanent)
  - Notifications (90 days?)
  - AI conversation logs (7 days max)
  - Audit logs (2-3 years)
  - Temporary files (7 days)
  - System logs (30-90 days)
- [ ] Implement cleanup jobs
- [ ] Ensure CNIC never retained longer than necessary
- [ ] Ensure private documents deleted appropriately

**Task:** Define and implement data retention policy

---

### 4.5 Security Hardening
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] SQL Injection prevention (Drizzle + parameterized queries)
- [ ] XSS prevention (React sanitization + CSP headers)
- [ ] CSRF protection (CSRF tokens)
- [ ] IDOR prevention (permission checks on all data access)
- [ ] Path traversal prevention (file upload validation)
- [ ] Malicious file upload prevention (MIME type + content validation)
- [ ] Rate limiting (brute force protection)
- [ ] Session hijacking prevention (secure cookies, HTTPS)
- [ ] Privilege escalation prevention (server-side permission checks)
- [ ] Security headers:
  - Content-Security-Policy
  - X-Frame-Options
  - X-Content-Type-Options
  - Strict-Transport-Security
  - X-XSS-Protection
- [ ] HTTPS enforcement
- [ ] Error message sanitization (no stack traces to users)
- [ ] Logging without passwords/keys
- [ ] API key protection

**Libraries Needed:**
- helmet for security headers
- express-rate-limit
- bcryptjs for password hashing (✅ already in deps)

**Task:** Implement comprehensive security hardening

---

## PHASE 5: AI INTEGRATION — CRITICAL

### 5.1 AI Service Architecture
**Status:** ❌ NOT IMPLEMENTED

**Missing:**
- [ ] AIService abstraction layer
- [ ] Provider pattern (Grok, OpenAI, others)
- [ ] Configuration loading
- [ ] Error handling
- [ ] Request/response logging
- [ ] Rate limiting
- [ ] Timeout handling
- [ ] Retry logic
- [ ] Circuit breaker pattern
- [ ] Cost tracking (optional)

**File Needed:**
- `src/lib/services/ai-service.ts`
- `src/lib/ai/grok-provider.ts`
- `src/lib/ai/types.ts`

**Environment Setup:**
- `GROK_API_KEY` (never expose in frontend!)
- `AI_PROVIDER` = "grok"
- `AI_REQUEST_TIMEOUT` = 30000
- `AI_RATE_LIMIT_RPM` = 60

**Task:** Build AI service layer with Grok provider

---

### 5.2 AI Backend API
**Status:** ❌ NOT IMPLEMENTED

**Missing:**
- [ ] Endpoint: `POST /api/ai/chat`
  - Accept message
  - Return response
  - Track request metrics
  - Rate limit per session
  - Handle errors gracefully
- [ ] Endpoint: `POST /api/ai/reset`
  - Clear temporary conversation context
- [ ] Endpoint: `GET /api/ai/health`
  - Check Grok API connectivity
  - Return status
- [ ] Session management:
  - Generate session token
  - Store temporary context
  - Clear context on reset
  - Never persist full conversations
- [ ] Metrics collection:
  - Request count
  - Success/failure counts
  - Response time
  - Error types
  - Rate limit events

**API Routes Needed:**
- `src/app/api/ai/chat/route.ts`
- `src/app/api/ai/reset/route.ts`
- `src/app/api/ai/health/route.ts`

**Database Schema Additions:**
- `ai_request_metrics` table
- Maybe `ai_sessions` table for temporary context

**CRITICAL SECURITY:**
- API key NEVER sent to frontend
- All Grok calls ONLY from backend
- Rate limiting per session/IP
- Timeout handling
- Error responses don't expose backend details

**Task:** Implement complete AI backend with Grok integration

---

### 5.3 AI Frontend Component
**Status:** ❌ NOT IMPLEMENTED

**Missing:**
- [ ] Floating button component (JKADB AI)
- [ ] Chat window (compact, professional)
- [ ] Message input with send button
- [ ] Message display (user vs assistant)
- [ ] Typing indicator
- [ ] Error message display
- [ ] Loading states
- [ ] Language toggle (English/Urdu RTL)
- [ ] Close button
- [ ] Minimize button
- [ ] Reset conversation button
- [ ] Mobile responsive
- [ ] Accessibility (keyboard navigation, screen readers)

**Components Needed:**
- `src/components/AIAssistant/AIBubble.tsx`
- `src/components/AIAssistant/AIChatWindow.tsx`
- `src/components/AIAssistant/AIMessage.tsx`
- `src/hooks/useAIChat.ts`

**States to Handle:**
- Idle
- Loading
- Error
- Connected
- Disconnected
- Rate limited

**Task:** Build professional AI assistant UI component

---

### 5.4 AI Knowledge Base
**Status:** ❌ NOT IMPLEMENTED

**Missing:**
- [ ] AI system prompt that:
  - Explains JKADB workflow
  - Lists categories
  - Explains statuses
  - Provides FAQ content
  - Guides users to features
  - Includes contact information
  - Identifies creator (Hozafa Mehmood)
- [ ] Dynamic prompt injection from:
  - Categories
  - FAQ items
  - Departments
  - Statuses
  - Application configuration
- [ ] Instruction that AI:
  - Should NOT invent government decisions
  - Should NOT make up policy
  - Should direct users to official features
  - Should acknowledge limitations
- [ ] Language support (English + Urdu translations)

**Implementation:**
- Build system prompt from database values
- Update prompt cache when config changes
- Pass to Grok in each request

**Task:** Create dynamic AI knowledge base

---

### 5.5 AI Testing
**Status:** ❌ MISSING

**Real Integration Test:**
1. Open JKADB as citizen
2. Click AI bubble
3. Ask: "What can you help me with?"
4. Verify request reaches backend
5. Verify backend calls Grok
6. Verify Grok response returns
7. Verify response displays in UI
8. Ask second question
9. Verify context maintained
10. Close chat
11. Verify context cleared
12. Reopen chat
13. Verify clean conversation

**Test Scenarios:**
- Grok API unavailable → graceful error
- Rate limited → show appropriate message
- Timeout → show error
- Invalid API key → show error
- Session expired → reset conversation
- Mobile → bubble positioned well
- RTL → Urdu displays correctly

**Task:** Test complete AI workflow

---

### 5.6 AI Health Monitoring
**Status:** ❌ MISSING

**Missing:**
- [ ] Admin → System → AI section showing:
  - Connected/Disconnected status
  - Total requests today
  - Success rate %
  - Failed requests
  - Rate limit status
  - Average response time
  - Last error (if any)
  - API availability status
  - Current quota usage (if available)
- [ ] Real-time monitoring
- [ ] Historical data
- [ ] Alert on failures

**API Needed:**
- `GET /api/admin/ai/metrics` — ❌ MISSING
- `GET /api/admin/ai/health` — ❌ MISSING

**Database Schema Needed:**
- Metrics stored in `ai_request_metrics`

**Task:** Implement AI health monitoring

---

## PHASE 6: LOCATIONS MANAGEMENT

### 6.1 Districts
**Status:** ⚠️ PARTIAL

**Exists:**
- Schema defined
- Some data population

**Missing:**
- [ ] Admin management page
- [ ] Create district
- [ ] Edit district
- [ ] Delete district (soft delete)
- [ ] Enable/disable
- [ ] Sort order management
- [ ] Urdu name support
- [ ] Code field

**API:**
- `GET /api/locations/districts` — ✅ EXISTS
- `POST /api/admin/locations/districts` — ❌ MISSING
- `PATCH /api/admin/locations/districts/:id` — ❌ MISSING

**Task:** Implement district management CRUD

---

### 6.2 Tehsils
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] Admin management page
- [ ] CRUD operations
- [ ] Cascade from district selection
- [ ] Enable/disable

**API:**
- `GET /api/locations/tehsils` — ✅ EXISTS but verify filtering
- `POST /api/admin/locations/tehsils` — ❌ MISSING

**Task:** Implement tehsil management

---

### 6.3 Union Councils
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] Similar CRUD as above
- [ ] Filter by tehsil
- [ ] Filter by district

**API:**
- `GET /api/locations/union-councils` — ✅ EXISTS
- Post Office filtering needed

**Task:** Implement union council management

---

### 6.4 Post Offices
**Status:** ❌ MISSING FROM SCHEMA

**Critical Missing:**
- [ ] Database table for post offices
- [ ] Schema definition in Drizzle
- [ ] Relationships to tehsil/UC
- [ ] Management CRUD
- [ ] API endpoints

**Schema Needed:**
```typescript
export const postOffices = pgTable("post_offices", {
  id: uuid("id").primaryKey().defaultRandom(),
  unionCouncilId: uuid("uc_id").references(() => unionCouncils.id),
  tehsilId: uuid("tehsil_id").references(() => tehsils.id),
  districtId: uuid("district_id").references(() => districts.id),
  nameEn: varchar("name_en", { length: 200 }).notNull(),
  nameUr: varchar("name_ur", { length: 200 }),
  code: varchar("code", { length: 50 }),
  isActive: boolean("is_active").default(true).notNull(),
  sortOrder: integer("sort_order").default(0),
  createdAt: timestamp("created_at").defaultNow().notNull(),
  updatedAt: timestamp("updated_at").defaultNow().notNull(),
});
```

**Migration Needed:**
- Create new migration for post_offices table
- Index on union_council_id

**Task:** ADD POST OFFICES TABLE AND MANAGEMENT

---

### 6.5 Constituencies/LA Areas
**Status:** ⚠️ PARTIAL

**Exists:**
- Schema defined

**Missing:**
- [ ] Management CRUD
- [ ] LA designation (LA-14 Bagh (1) format)

**Task:** Implement constituency/LA management

---

### 6.6 General Areas
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] Management CRUD
- [ ] Relationships

**Task:** Implement area management

---

## PHASE 7: CATEGORIES & SUBCATEGORIES

### 7.1 Categories
**Status:** ⚠️ PARTIAL

**Exists:**
- Schema
- Basic page

**Missing:**
- [ ] Create category
- [ ] Edit category
- [ ] Delete category (soft delete)
- [ ] Enable/disable
- [ ] Reorder (sort_order)
- [ ] Icon selection
- [ ] Translate to Urdu
- [ ] Audit logging

**API:**
- `GET /api/categories` — ✅ EXISTS
- `POST /api/admin/categories` — ❌ MISSING
- `PATCH /api/admin/categories/:id` — ❌ MISSING

**Task:** Implement category management

---

### 7.2 Subcategories
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] Similar CRUD as categories
- [ ] Always linked to parent category
- [ ] Filter by category

**API:**
- `GET /api/categories/:id/subcategories` — ❌ MISSING
- `POST /api/admin/categories/:id/subcategories` — ❌ MISSING

**Task:** Implement subcategory management

---

## PHASE 8: UTILITIES & UX

### 8.1 Startup Animation
**Status:** ❌ MISSING

**Missing:**
- [ ] Splash screen component
- [ ] Animation sequence:
  1. Open Hand icon
  2. JKADB text
  3. Jammu Kashmir Awami Dast-o-Bazo (English)
  4. جموں کشمیر عوامی دست و بازو (Urdu)
  5. From: MAJOR FORCE Narakot
  6. Built by: Hozafa Mehmood
  7. Fade to main app
- [ ] Professional, fast, smooth
- [ ] Non-intrusive
- [ ] Duration: 2-3 seconds

**Component:**
- `src/components/SplashScreen.tsx` (already exists, needs implementation)

**Task:** Implement professional startup animation

---

### 8.2 Theme System
**Status:** ⚠️ PARTIAL

**Exists:**
- next-themes in dependencies
- Theme provider likely needed

**Missing:**
- [ ] Dark/light theme support
- [ ] Persist theme preference
- [ ] System preference detection
- [ ] Apply to all components
- [ ] Color scheme consistency

**Implementation:**
- Set up ThemeProvider wrapper
- Define CSS variables for colors
- Ensure good contrast in both themes

**Task:** Implement complete theme system

---

### 8.3 Language Support (Urdu/English)
**Status:** ⚠️ MINIMAL

**Missing:**
- [ ] i18n library setup (next-i18next or similar)
- [ ] Urdu translations for ALL text
- [ ] RTL layout support
- [ ] Language toggle button
- [ ] Persist language preference
- [ ] Dynamic language switching
- [ ] RTL forms
- [ ] RTL tables
- [ ] RTL chat
- [ ] Urdu font support (proper Noto Sans Urdu)

**Library Needed:**
- next-i18next or similar

**Task:** Implement comprehensive i18n with Urdu RTL

---

### 8.4 Responsive Design
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] Mobile-first approach
- [ ] Test on:
  - iPhone (390px)
  - Tablet (768px)
  - Desktop (1024px+)
- [ ] Touch-friendly buttons
- [ ] Appropriate font sizes
- [ ] Forms optimized for mobile
- [ ] Tables responsive (scroll/collapse)
- [ ] Navigation mobile-friendly
- [ ] No horizontal scroll issues
- [ ] AI bubble positioned safely
- [ ] Camera upload support
- [ ] Low bandwidth support (lazy loading)

**Task:** Audit and improve mobile responsiveness

---

### 8.5 Accessibility
**Status:** ❌ MISSING

**Missing:**
- [ ] Keyboard navigation
- [ ] Visible focus states
- [ ] Semantic HTML
- [ ] ARIA labels
- [ ] Screen reader testing
- [ ] Color contrast compliance
- [ ] Text sizing options
- [ ] Reduced motion support
- [ ] Focus management in modals

**Task:** Implement WCAG 2.1 AA compliance

---

### 8.6 Error Handling
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] No raw error messages to users
- [ ] User-friendly error messages
- [ ] Error boundaries in React
- [ ] Logging errors without exposing details
- [ ] Retry mechanisms
- [ ] Fallback UI
- [ ] Error reference IDs for support

**Task:** Implement comprehensive error handling

---

### 8.7 Loading States
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] Skeleton loaders for:
  - Tables
  - Cards
  - Request details
  - Dashboard
- [ ] Progress indicators
- [ ] Spinner animations
- [ ] Prevent interaction during load

**Task:** Add skeleton loaders throughout app

---

### 8.8 Pagination
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] Server-side pagination for large lists
- [ ] Especially for:
  - Complaints
  - Requests
  - Users
  - Admins
  - Officers
  - Messages
  - Audit logs
  - Notifications
  - Reports
- [ ] Do not load thousands of records to browser
- [ ] Pagination controls (prev, next, page numbers)
- [ ] Per-page selection (10, 25, 50)
- [ ] Total record count
- [ ] Current page indicator

**Task:** Implement server-side pagination throughout

---

## PHASE 9: FILE MANAGEMENT

### 9.1 Evidence Upload
**Status:** ⚠️ PARTIAL

**Missing:**
- [ ] File type validation (JPG, JPEG, PNG, PDF only)
- [ ] File size limits (e.g., 10MB per file, 50MB total)
- [ ] Server-side validation
- [ ] Secure filename handling
- [ ] Private storage (not accessible via predictable URLs)
- [ ] Authorization checks on download
- [ ] Image preview
- [ ] Image compression
- [ ] Malware scanning (if available)
- [ ] No executable files
- [ ] MIME type validation
- [ ] Mobile camera upload support

**Storage:**
- Local file system OR cloud storage (AWS S3, etc.)
- Secure directory outside web root
- Organized by complaint ID

**API Needed:**
- `POST /api/complaints/:id/upload` — ❌ MISSING
- `GET /api/complaints/:id/attachment/:attachmentId` — ❌ MISSING (with auth)
- `DELETE /api/complaints/:id/attachment/:attachmentId` — ❌ MISSING (with auth)

**Database Schema:**
- `attachments` table exists
- Ensure proper relationships and security fields

**Task:** Implement secure evidence upload/download

---

## FINAL CHECKLIST — COMPLETE FEATURE INVENTORY

### MUST-IMPLEMENT BEFORE COMPLETION

**Citizen Workflow:**
- [ ] Homepage (professional)
- [ ] Multi-step complaint submission
- [ ] Dynamic location selection
- [ ] Evidence upload (secure)
- [ ] Draft save/restore
- [ ] Complaint tracking (secure)
- [ ] Notifications (real)
- [ ] Announcements (published)
- [ ] Quick Alerts (active)
- [ ] Help/FAQ (database-driven)
- [ ] About page
- [ ] Settings (theme/language)
- [ ] AI Assistant (REAL Grok integration)

**Admin Workflow:**
- [ ] Admin login (via Settings)
- [ ] Admin authentication (secure)
- [ ] Multiple admin accounts
- [ ] Role-based permissions
- [ ] Request dashboard (REAL counts)
- [ ] Request list by status
- [ ] Request search & filters
- [ ] Request sorting
- [ ] Request detail page
- [ ] Admin response system
- [ ] Accept/Decline workflow
- [ ] Response templates
- [ ] Request edit history
- [ ] Messaging (with privacy)
- [ ] Resolution & feedback
- [ ] Reopen system
- [ ] Duplicate detection
- [ ] Departments management
- [ ] Officers management
- [ ] Assignments
- [ ] SLA tracking
- [ ] Escalations (auto + manual)
- [ ] Announcements management
- [ ] Quick Alerts management
- [ ] Public Contact management
- [ ] Admin notifications
- [ ] Analytics (REAL data)
- [ ] Reports & export
- [ ] Audit logs viewer
- [ ] System health monitor
- [ ] Locations management (all)
- [ ] Categories & subcategories

**Security & System:**
- [ ] Secure authentication
- [ ] RBAC enforcement
- [ ] IDOR prevention
- [ ] SQL injection prevention
- [ ] XSS prevention
- [ ] CSRF protection
- [ ] File upload security
- [ ] API key protection
- [ ] Session security
- [ ] Rate limiting
- [ ] Error handling
- [ ] Audit logging (immutable)
- [ ] Data retention policy
- [ ] Backups & recovery
- [ ] System health monitoring
- [ ] Background jobs
- [ ] Notifications (real delivery)

**AI Integration:**
- [ ] AI service architecture
- [ ] Grok API integration (REAL)
- [ ] AI backend API
- [ ] AI frontend component
- [ ] Knowledge base (dynamic)
- [ ] Session management
- [ ] Metrics collection
- [ ] Health monitoring
- [ ] Rate limiting
- [ ] Error handling
- [ ] No client-side API key
- [ ] No permanent transcripts

**Polish & UX:**
- [ ] Startup animation
- [ ] Professional design system
- [ ] Theme system
- [ ] Language/Urdu support (RTL)
- [ ] Mobile responsive
- [ ] Accessibility (WCAG)
- [ ] Skeleton loaders
- [ ] Error boundaries
- [ ] Loading states
- [ ] Empty states
- [ ] Confirmation dialogs
- [ ] Toast notifications
- [ ] Pagination (server-side)
- [ ] Browser compatibility

---

## PRIORITY ORDER FOR IMPLEMENTATION

### CRITICAL PATH (Do First)
1. Post Offices schema + migration
2. Admin authentication hardening
3. RBAC database schema + enforcement
4. Real complaint submission + database persistence
5. Admin request dashboard (REAL counts)
6. Request detail + admin actions (REAL backend)
7. SLA calculation
8. Escalations (background job)
9. Notifications system
10. AI service + Grok integration
11. Security hardening

### IMPORTANT (Do Next)
12. Announcements + Quick Alerts
13. Assignments + Department management
14. Analytics + Reports
15. Audit logs viewer
16. System health + Backups
17. File upload security

### NICE TO HAVE (Last)
18. Duplicate detection
19. Data retention jobs
20. Advanced optimizations
21. Polish & design refinements

---

## IMPLEMENTATION NOTES

**Database:**
- All schema changes need migrations
- Foreign keys required
- Proper indexing for performance
- Transactions for multi-step operations

**Backend:**
- Every feature needs API endpoints
- Server-side permission checks on ALL endpoints
- Proper error handling & validation
- Audit logging for important actions
- No hard-coded data

**Frontend:**
- No mock data in production pages
- All data from backend/database
- Permission-based UI (non-security feature)
- Proper loading/error states
- Mobile responsive
- RTL support for Urdu

**Testing:**
- Real end-to-end flows
- Security testing
- Permissions verification
- Database persistence
- No fake functionality

---

**Generated:** August 22, 2026  
**Next Step:** Begin Phase 1 — Critical Backend Implementation
