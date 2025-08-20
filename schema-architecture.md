Smart Clinic Management System - Architecture Design

Section 1: Architecture Summary

This Smart Clinic Management System is built using Spring Boot and implements a hybrid architecture that combines both MVC and REST controller patterns to serve different types of users and interfaces. 
The system supports three user roles: Patients, Doctors, and Administrators, each with tailored interfaces and functionalities.
The application employs a dual-interface approach where Thymeleaf-based MVC controllers handle server-side rendered dashboards for Admin and Doctor users, providing rich, interactive web interfaces.
Meanwhile, REST controllers serve JSON APIs for modern frontend modules such as Appointments, PatientDashboard, and PatientRecord, enabling mobile app integration and scalable client-server communication.

The data persistence strategy utilizes a polyglot persistence approach with two databases: 
MySQL handles structured relational data including patient records, doctor information, appointments, and administrative data using JPA entities, while MongoDB manages flexible document-based data such as prescriptions using document models. 
This hybrid database design leverages the strengths of both relational and NoSQL databases. 
All requests flow through a centralized service layer that implements business logic and orchestrates data operations across multiple repositories, ensuring clean separation of concerns and maintainable code architecture.

Section 2: Numbered Flow of Data and Control

1. User Interface Interaction: Users interact with the system through either Thymeleaf-rendered web dashboards (AdminDashboard, DoctorDashboard) for administrative tasks or REST-based modules (Appointments, PatientDashboard, PatientRecord) for patient-facing functionalities and mobile applications.

2. Request Routing: Incoming requests are routed to the appropriate controller based on the URL path and HTTP method - Thymeleaf Controllers handle server-side rendering requests while REST Controllers process API calls and return JSON responses.

3. Business Logic Processing: All controllers delegate the core business logic to the Service Layer, which applies business rules, performs validations, coordinates complex workflows (such as checking doctor availability before scheduling appointments), and ensures proper separation between presentation and business concerns.

4. Data Access Coordination: The service layer communicates with the Repository Layer to perform database operations, utilizing MySQL Repositories (Spring Data JPA) for structured relational data and MongoDB Repository (Spring Data MongoDB) for flexible document storage, abstracting database-specific implementation details.

5. Database Operations: Each repository interfaces with its respective database engine - MySQL stores normalized relational data with enforced constraints for entities like patients, doctors, appointments, and admin records, while MongoDB handles variable-structure documents like prescriptions that benefit from schema flexibility.

6. Model Binding and Data Transformation: Retrieved data is mapped into appropriate Java model classes - MySQL data becomes JPA entities annotated with @Entity representing relational table rows, while MongoDB data transforms into document objects annotated with @Document representing BSON/JSON structures.

7. Response Generation: Finally, the processed models are used to generate responses - in MVC flows, models are passed to Thymeleaf templates for server-side HTML rendering, while in REST flows, the same models or Data Transfer Objects (DTOs) are serialized into JSON format and returned as HTTP API responses to client applications.
