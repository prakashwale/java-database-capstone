##Admin User Stories
Story 1: Admin Login
Title:
As an admin, I want to log into the portal with my username and password, so that I can manage the platform securely.
Acceptance Criteria:

Admin can enter valid username and password on login form
System validates credentials against admin database
Successful login redirects to admin dashboard
Failed login displays appropriate error message
Session is created and maintained for authenticated admin

Priority: High
Story Points: 3
Notes:

Implement secure authentication with password encryption
Consider session timeout for security

Story 2: Admin Logout
Title:
As an admin, I want to log out of the portal, so that I can protect system access when I'm done working.
Acceptance Criteria:

Logout option is visible in admin dashboard
Clicking logout terminates admin session
Admin is redirected to login page after logout
Subsequent access to admin pages requires re-authentication

Priority: High
Story Points: 2
Notes:

Clear all session data on logout
Ensure secure session termination

Story 3: Add Doctor to Portal
Title:
As an admin, I want to add doctors to the portal, so that they can manage their appointments and serve patients.
Acceptance Criteria:

Admin can access doctor registration form
Form includes required fields: name, email, specialization, phone, password
System validates email format and uniqueness
New doctor account is created in database
Doctor receives account creation confirmation
Doctor appears in admin's doctor management list

Priority: High
Story Points: 5
Notes:

Generate secure default password or allow admin to set password
Send welcome email to new doctor with login credentials

Story 4: Delete Doctor Profile
Title:
As an admin, I want to delete a doctor's profile from the portal, so that I can manage system users and remove inactive doctors.
Acceptance Criteria:

Admin can view list of all doctors
Delete option is available for each doctor
System prompts for confirmation before deletion
Deletion removes doctor and associated data
Future appointments with deleted doctor are handled appropriately
Success message confirms deletion

Priority: Medium
Story Points: 4
Notes:

Consider soft delete vs hard delete for audit trail
Handle existing appointments when doctor is deleted

Story 5: Generate Usage Statistics
Title:
As an admin, I want to run a stored procedure in MySQL CLI to get the number of appointments per month, so that I can track usage statistics and monitor system performance.
Acceptance Criteria:

Stored procedure exists in MySQL database
Procedure returns monthly appointment counts
Data includes current and previous months
Results are formatted in readable format
Admin can execute procedure from database interface

Priority: Low
Story Points: 3
Notes:

Create stored procedure: GET_MONTHLY_APPOINTMENT_STATS()
Consider creating web interface for easier access to statistics

Story 6: Manage System Settings
Title:
As an admin, I want to configure system settings, so that I can customize the portal according to clinic requirements.
Acceptance Criteria:

Admin can access system settings page
Settings include clinic hours, appointment duration, system name
Changes are saved to database
Updated settings reflect across the portal immediately
Changes are logged for audit purposes

Priority: Medium
Story Points: 4
Notes:

Settings should have validation rules
Consider backup/restore functionality for settings

##Patient User Stories
Story 1: View Doctors Without Login
Title:
As a patient, I want to view a list of doctors without logging in, so that I can explore options before registering.
Acceptance Criteria:

Public doctors list page is accessible without authentication
List displays doctor names, specializations, and basic information
Contact information is not displayed to non-registered users
Page encourages registration for booking appointments
Search and filter options are available

Priority: Medium
Story Points: 3
Notes:

Hide sensitive doctor information from public view
Include call-to-action for patient registration

Story 2: Patient Registration
Title:
As a patient, I want to sign up using my email and password, so that I can book appointments and manage my health records.
Acceptance Criteria:

Registration form is accessible from homepage
Form includes required fields: name, email, phone, date of birth, password
Email validation ensures unique and valid format
Password meets security requirements
Account is created and confirmation email is sent
New patient can immediately log in after registration

Priority: High
Story Points: 4
Notes:

Implement email verification for security
Password should meet complexity requirements

Story 3: Patient Login
Title:
As a patient, I want to log into the portal, so that I can manage my bookings and access my medical information.
Acceptance Criteria:

Login form accepts email and password
System validates patient credentials
Successful login redirects to patient dashboard
Failed login shows appropriate error message
"Forgot password" option is available

Priority: High
Story Points: 3
Notes:

Implement "remember me" functionality
Consider multi-factor authentication for enhanced security

Story 4: Patient Logout
Title:
As a patient, I want to log out of the portal, so that I can secure my account and protect my medical information.
Acceptance Criteria:

Logout option is visible in patient dashboard
Logout clears patient session completely
Redirection to homepage after logout
Accessing protected pages requires re-authentication

Priority: High
Story Points: 2
Notes:

Ensure complete session cleanup
Auto-logout after period of inactivity

Story 5: Book Appointment
Title:
As a patient, I want to log in and book an hour-long appointment, so that I can consult with a doctor for my health concerns.
Acceptance Criteria:

Patient can select preferred doctor from available list
Calendar shows doctor's available time slots
Appointment duration is set to one hour
Patient can select date and time for appointment
Booking confirmation is displayed after successful scheduling
Patient receives email confirmation of appointment

Priority: High
Story Points: 6
Notes:

Validate appointment conflicts before confirmation
Consider appointment buffer time between patients

Story 6: View Upcoming Appointments
Title:
As a patient, I want to view my upcoming appointments, so that I can prepare accordingly and manage my schedule.
Acceptance Criteria:

Dashboard displays list of all upcoming appointments
Each appointment shows doctor name, date, time, and location
Appointments are sorted by date and time
Patient can view appointment details
Options to reschedule or cancel are available
Past appointments are accessible in separate section

Priority: Medium
Story Points: 3
Notes:

Include appointment reminders functionality
Provide easy access to doctor contact information


##Doctor User Stories
Story 1: Doctor Login
Title:
As a doctor, I want to log into the portal, so that I can manage my appointments and access patient information securely.
Acceptance Criteria:

Login form accepts doctor credentials (email/username and password)
System validates doctor authentication
Successful login redirects to doctor dashboard
Failed login displays security-appropriate error message
Session is maintained for the duration of the visit

Priority: High
Story Points: 3
Notes:

Implement secure authentication for medical data access
Consider role-based access control

Story 2: Doctor Logout
Title:
As a doctor, I want to log out of the portal, so that I can protect patient data and maintain medical confidentiality.
Acceptance Criteria:

Logout option is prominently displayed in doctor interface
Logout immediately terminates doctor session
All cached patient data is cleared
Redirection to secure login page
Subsequent access requires re-authentication

Priority: High
Story Points: 2
Notes:

Ensure HIPAA compliance in session management
Implement auto-logout for idle sessions

Story 3: View Appointment Calendar
Title:
As a doctor, I want to view my appointment calendar, so that I can stay organized and manage my daily schedule effectively.
Acceptance Criteria:

Calendar displays daily, weekly, and monthly views
Appointments show patient name, time, and appointment type
Calendar is interactive and allows navigation between dates
Color coding distinguishes different appointment types
Current date and time are clearly highlighted

Priority: High
Story Points: 4
Notes:

Include integration with external calendar systems
Provide print functionality for daily schedules

Story 4: Mark Unavailability
Title:
As a doctor, I want to mark my unavailability, so that patients can only see and book my available time slots.
Acceptance Criteria:

Doctor can select dates and times to mark as unavailable
Unavailable slots are blocked from patient booking system
Doctor can add reason for unavailability (optional)
Recurring unavailability patterns can be set
Changes are immediately reflected in patient booking interface
Doctor can remove unavailability blocks when needed

Priority: Medium
Story Points: 5
Notes:

Consider emergency override functionality for urgent cases
Notify patients if existing appointments conflict with unavailability

Story 5: Update Profile Information
Title:
As a doctor, I want to update my profile with specialization and contact information, so that patients have up-to-date information when selecting their healthcare provider.
Acceptance Criteria:

Profile form includes specialization, contact details, bio, and qualifications
Changes are saved immediately to database
Updated information appears on patient-facing doctor listings
Profile photo upload functionality is available
Required fields are validated before saving
Profile change history is maintained for admin oversight

Priority: Medium
Story Points: 4
Notes:

Validate medical credentials and specializations
Consider admin approval workflow for significant changes

Story 6: View Patient Details
Title:
As a doctor, I want to view patient details for upcoming appointments, so that I can be prepared and provide personalized medical care.
Acceptance Criteria:

Doctor can access patient list for scheduled appointments
Patient details include medical history, previous visits, and contact information
Information is displayed in easy-to-read format
Search functionality allows quick patient lookup
Patient privacy is maintained with secure data access
Recent test results and medications are highlighted

Priority: High
Story Points: 4
Notes:

Ensure HIPAA compliance in patient data access
Include emergency contact information for patients
