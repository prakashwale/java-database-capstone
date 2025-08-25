# Smart Clinic Management System - Database Schema Design

## MySQL Database Design

MySQL will handle structured, relational data that requires ACID properties, referential integrity, and complex queries with joins. This includes core operational data like users, appointments, and system configuration.

### Table: patients
- **id**: INT, Primary Key, Auto Increment
- **first_name**: VARCHAR(50), Not Null
- **last_name**: VARCHAR(50), Not Null
- **email**: VARCHAR(100), Unique, Not Null
- **phone**: VARCHAR(15), Not Null
- **date_of_birth**: DATE, Not Null
- **gender**: ENUM('Male', 'Female', 'Other'), Not Null
- **address**: TEXT
- **emergency_contact_name**: VARCHAR(100)
- **emergency_contact_phone**: VARCHAR(15)
- **password_hash**: VARCHAR(255), Not Null
- **created_at**: TIMESTAMP, Default CURRENT_TIMESTAMP
- **updated_at**: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
- **is_active**: BOOLEAN, Default TRUE

*Design Notes: Email serves as username. Soft delete via is_active flag preserves appointment history. Emergency contact for medical situations.*

### Table: doctors
- **id**: INT, Primary Key, Auto Increment
- **first_name**: VARCHAR(50), Not Null
- **last_name**: VARCHAR(50), Not Null
- **email**: VARCHAR(100), Unique, Not Null
- **phone**: VARCHAR(15), Not Null
- **specialization**: VARCHAR(100), Not Null
- **license_number**: VARCHAR(50), Unique, Not Null
- **years_experience**: INT, Default 0
- **consultation_fee**: DECIMAL(10,2)
- **bio**: TEXT
- **password_hash**: VARCHAR(255), Not Null
- **profile_image_url**: VARCHAR(255)
- **created_at**: TIMESTAMP, Default CURRENT_TIMESTAMP
- **updated_at**: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
- **is_active**: BOOLEAN, Default TRUE

*Design Notes: License number ensures legitimate medical practitioners. Specialization helps patients find right doctor. Profile image for patient-facing interface.*

### Table: admin
- **id**: INT, Primary Key, Auto Increment
- **username**: VARCHAR(50), Unique, Not Null
- **email**: VARCHAR(100), Unique, Not Null
- **first_name**: VARCHAR(50), Not Null
- **last_name**: VARCHAR(50), Not Null
- **password_hash**: VARCHAR(255), Not Null
- **role_level**: INT, Default 1 *(1=Admin, 2=Super Admin)*
- **last_login**: TIMESTAMP
- **created_at**: TIMESTAMP, Default CURRENT_TIMESTAMP
- **is_active**: BOOLEAN, Default TRUE

*Design Notes: Separate admin table for role-based access control. Role levels allow different admin privileges.*

### Table: appointments
- **id**: INT, Primary Key, Auto Increment
- **patient_id**: INT, Foreign Key → patients(id), Not Null
- **doctor_id**: INT, Foreign Key → doctors(id), Not Null
- **appointment_date**: DATE, Not Null
- **appointment_time**: TIME, Not Null
- **duration_minutes**: INT, Default 60
- **status**: ENUM('Scheduled', 'Confirmed', 'Completed', 'Cancelled', 'No-Show'), Default 'Scheduled'
- **appointment_type**: ENUM('Consultation', 'Follow-up', 'Emergency', 'Check-up'), Default 'Consultation'
- **patient_notes**: TEXT *(reason for visit)*
- **created_at**: TIMESTAMP, Default CURRENT_TIMESTAMP
- **updated_at**: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
- **cancelled_at**: TIMESTAMP, Null
- **cancelled_by**: ENUM('Patient', 'Doctor', 'Admin'), Null

*Design Notes: Composite index on (doctor_id, appointment_date, appointment_time) prevents double-booking. Status tracking for appointment lifecycle. Cancellation audit trail.*

### Table: doctor_availability
- **id**: INT, Primary Key, Auto Increment
- **doctor_id**: INT, Foreign Key → doctors(id), Not Null
- **day_of_week**: INT, Not Null *(0=Sunday, 1=Monday, ..., 6=Saturday)*
- **start_time**: TIME, Not Null
- **end_time**: TIME, Not Null
- **is_available**: BOOLEAN, Default TRUE
- **created_at**: TIMESTAMP, Default CURRENT_TIMESTAMP

*Design Notes: Defines doctor's weekly schedule. Allows doctors to set their working hours for each day.*

### Table: doctor_unavailable_dates
- **id**: INT, Primary Key, Auto Increment
- **doctor_id**: INT, Foreign Key → doctors(id), Not Null
- **unavailable_date**: DATE, Not Null
- **start_time**: TIME, Null *(if null, entire day is blocked)*
- **end_time**: TIME, Null
- **reason**: VARCHAR(255) *(vacation, conference, emergency)*
- **created_at**: TIMESTAMP, Default CURRENT_TIMESTAMP

*Design Notes: Handles specific dates when doctor is unavailable. Supports partial day blocking with time ranges.*

### Table: clinic_settings
- **id**: INT, Primary Key, Auto Increment
- **setting_key**: VARCHAR(100), Unique, Not Null
- **setting_value**: TEXT, Not Null
- **description**: TEXT
- **updated_by**: INT, Foreign Key → admin(id)
- **updated_at**: TIMESTAMP, Default CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP

*Design Notes: Flexible key-value store for system configurations like clinic hours, appointment duration, contact info.*

### Database Relationships and Constraints

**Foreign Key Constraints:**
- appointments.patient_id → patients.id (ON DELETE CASCADE)
- appointments.doctor_id → doctors.id (ON DELETE RESTRICT)
- doctor_availability.doctor_id → doctors.id (ON DELETE CASCADE)
- doctor_unavailable_dates.doctor_id → doctors.id (ON DELETE CASCADE)

**Indexes for Performance:**
- appointments: (doctor_id, appointment_date, appointment_time)
- appointments: (patient_id, appointment_date)
- patients: (email), (phone)
- doctors: (email), (specialization)

**Business Rules Enforced:**
- No overlapping appointments for same doctor
- Appointment time must be within doctor's availability
- Patient cannot book multiple appointments at same time

---

## MongoDB Collection Design

MongoDB will handle flexible, document-based data that doesn't fit well into rigid table structures. This includes medical prescriptions with varying formats, patient feedback, system logs, and communication records.

### Collection: prescriptions

```json
{
  "_id": "ObjectId('650abc123456789012345678')",
  "appointmentId": 125,
  "patientId": 45,
  "doctorId": 12,
  "patientName": "John Smith",
  "doctorName": "Dr. Sarah Johnson",
  "prescriptionDate": "2024-03-15T10:30:00Z",
  "medications": [
    {
      "name": "Amoxicillin",
      "dosage": "500mg",
      "frequency": "3 times daily",
      "duration": "7 days",
      "instructions": "Take with food",
      "quantity": 21,
      "refillsAllowed": 0
    },
    {
      "name": "Ibuprofen",
      "dosage": "400mg",
      "frequency": "Every 6-8 hours as needed",
      "duration": "5 days",
      "instructions": "Take with food or milk",
      "quantity": 20,
      "refillsAllowed": 1
    }
  ],
  "clinicalNotes": "Patient reported mild throat infection. Prescribed antibiotics for bacterial treatment.",
  "diagnosis": "Acute pharyngitis",
  "followUpRequired": true,
  "followUpDays": 7,
  "pharmacy": {
    "name": "HealthPlus Pharmacy",
    "address": "123 Main Street, San Francisco, CA",
    "phone": "+1-555-0123",
    "faxNumber": "+1-555-0124"
  },
  "digitalSignature": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
  "status": "Active",
  "createdAt": "2024-03-15T10:30:00Z",
  "updatedAt": "2024-03-15T10:30:00Z"
}
```

*Design Notes: Flexible medication array supports varying prescription complexity. Embedded pharmacy object reduces lookups. Digital signature for legal compliance.*

### Collection: patient_feedback

```json
{
  "_id": "ObjectId('650def123456789012345678')",
  "appointmentId": 125,
  "patientId": 45,
  "doctorId": 12,
  "ratings": {
    "overall": 4.5,
    "doctorProfessionalism": 5,
    "waitTime": 4,
    "facilityCleanlines": 5,
    "staffFriendliness": 4
  },
  "textReview": "Dr. Johnson was very thorough and explained everything clearly. The wait time was reasonable and staff was helpful.",
  "recommendToFriend": true,
  "anonymous": false,
  "feedbackDate": "2024-03-15T14:30:00Z",
  "tags": ["professional", "thorough", "friendly"],
  "improvement_suggestions": [
    "Could use more parking spaces",
    "Online check-in would be helpful"
  ],
  "wouldReturnToDoctor": true,
  "verified": true,
  "moderationStatus": "Approved"
}
```

*Design Notes: Structured ratings with flexible text feedback. Tags enable sentiment analysis. Moderation workflow for quality control.*

### Collection: communication_logs

```json
{
  "_id": "ObjectId('650ghi123456789012345678')",
  "type": "appointment_reminder",
  "patientId": 45,
  "doctorId": 12,
  "appointmentId": 125,
  "communicationMethod": "email",
  "recipientEmail": "john.smith@email.com",
  "subject": "Appointment Reminder - Tomorrow at 10:30 AM",
  "content": "Dear John, this is a reminder for your appointment with Dr. Sarah Johnson tomorrow...",
  "sentAt": "2024-03-14T18:00:00Z",
  "deliveryStatus": "delivered",
  "readStatus": "opened",
  "responseTracking": {
    "confirmationRequested": true,
    "patientConfirmed": true,
    "confirmedAt": "2024-03-14T19:15:00Z"
  },
  "metadata": {
    "emailProvider": "SendGrid",
    "messageId": "14c5d75ce93.dfd.64b469cdff.20030203@your-domain.com",
    "campaignId": "appointment_reminders_march_2024"
  }
}
```

*Design Notes: Comprehensive communication tracking. Supports multiple communication methods. Delivery and engagement analytics.*

### Collection: system_audit_logs

```json
{
  "_id": "ObjectId('650jkl123456789012345678')",
  "timestamp": "2024-03-15T10:25:30Z",
  "userId": 12,
  "userType": "doctor",
  "action": "view_patient_record",
  "resourceType": "patient",
  "resourceId": 45,
  "ipAddress": "192.168.1.100",
  "userAgent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36",
  "sessionId": "sess_abc123def456",
  "details": {
    "patientName": "John Smith",
    "recordsAccessed": ["medical_history", "prescriptions", "appointments"],
    "reasonForAccess": "scheduled_appointment"
  },
  "complianceFlags": {
    "hipaaCompliant": true,
    "auditRequired": true,
    "sensitiveDataAccessed": true
  },
  "geolocation": {
    "country": "US",
    "state": "CA",
    "city": "San Francisco"
  }
}
```

*Design Notes: Comprehensive audit trail for compliance. HIPAA-focused tracking. Geolocation for security monitoring.*

## Database Integration Strategy

**Data Synchronization:**
- MySQL stores authoritative user and appointment data
- MongoDB references MySQL IDs for data consistency
- Scheduled sync jobs update denormalized data in MongoDB

**Query Patterns:**
- Use MySQL for transactional operations and complex joins
- Use MongoDB for flexible document retrieval and full-text search
- Implement caching layer for frequently accessed data

**Backup and Recovery:**
- MySQL: Regular automated backups with point-in-time recovery
- MongoDB: Replica sets with automated failover
- Cross-database referential integrity checks via application logic

**Scalability Considerations:**
- MySQL: Read replicas for query performance
- MongoDB: Sharding by patientId for horizontal scaling
- Connection pooling for both databases
