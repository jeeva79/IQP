Testing with Postman/cURL
Test Case 1: Complete User Journey
Step 1: Save User
bashcurl -X POST http://localhost:8080/api/user/save \
-H "Content-Type: application/json" \
-d '{
  "name": "Rajesh Kumar",
  "email": "rajesh@example.com",
  "phone": "9876543210"
}'
Expected Response:
json{
  "success": true,
  "userId": 1,
  "userIdGenerated": "550e8400-e29b-41d4-a716-446655440000",
  "message": "User details saved successfully"
}
Step 2: Register Vehicle
bashcurl -X POST http://localhost:8080/api/vehicle/register \
-H "Content-Type: application/json" \
-d '{
  "userId": "550e8400-e29b-41d4-a716-446655440000",
  "vehicleNumber": "KA01AB1234",
  "vehicleYear": 2020,
  "noOfDrivingAccidents": 0,
  "noOfDrivingViolations": 1,
  "coverageType": "Comprehensive",
  "coverageAmount": 100000.0,
  "coverageDeductibles": 5000.0
}'
Step 3: Get User Details
bashcurl -X GET http://localhost:8080/api/user/show/550e8400-e29b-41d4-a716-446655440000
Step 4: Update User
bashcurl -X PUT http://localhost:8080/api/user/update/550e8400-e29b-41d4-a716-446655440000 \
-H "Content-Type: application/json" \
-d '{
  "email": "rajesh.new@example.com",
  "phone": "9999999999"
}'
Step 5: Get Vehicle Details
bashcurl -X GET http://localhost:8080/api/vehicle/show/KA01AB1234
Step 6: Update Vehicle
bashcurl -X PUT http://localhost:8080/api/vehicle/update/KA01AB1234 \
-H "Content-Type: application/json" \
-d '{
  "noOfDrivingAccidents": 1,
  "noOfDrivingViolations": 2
}'
Step 7: Calculate Insurance Quotes
bashcurl -X GET http://localhost:8080/api/insurance/calculate/1
Step 8: Select Provider
bashcurl -X POST http://localhost:8080/api/insurance/select-provider \
-H "Content-Type: application/json" \
-d '{
  "userId": 1,
  "vehicleId": 1,
  "selectedProvider": "Acko",
  "selectedPlan": "Premium",
  "premiumAmount": 3680.0
}'
Step 9: Upload KYC (Using form-data)
bashcurl -X POST http://localhost:8080/api/kyc/upload/1 \
-F "drivingLicense=@/path/to/dl.pdf" \
-F "rc=@/path/to/rc.pdf" \
-F "aadhar=@/path/to/aadhar.pdf"
Step 10: View All Applications (Admin)
bashcurl -X GET http://localhost:8080/api/admin/allApplications

Validation Test Cases
Test Case 2: Invalid Email
bashcurl -X POST http://localhost:8080/api/user/save \
-H "Content-Type: application/json" \
-d '{
  "name": "Test User",
  "email": "invalid-email",
  "phone": "9876543210"
}'
Expected Response:
json{
  "success": false,
  "message": "Invalid email format"
}
Test Case 3: Invalid Phone Number
bashcurl -X POST http://localhost:8080/api/user/save \
-H "Content-Type: application/json" \
-d '{
  "name": "Test User",
  "email": "test@example.com",
  "phone": "123456"
}'
Expected Response:
json{
  "success": false,
  "message": "Invalid phone number. Must be 10 digits starting with 6-9"
}
Test Case 4: Invalid Vehicle Number
bashcurl -X POST http://localhost:8080/api/vehicle/register \
-H "Content-Type: application/json" \
-d '{
  "userId": "550e8400-e29b-41d4-a716-446655440000",
  "vehicleNumber": "INVALID123",
  "vehicleYear": 2020,
  "noOfDrivingAccidents": 0,
  "noOfDrivingViolations": 1,
  "coverageType": "Comprehensive",
  "coverageAmount": 100000.0,
  "coverageDeductibles": 5000.0
}'
Expected Response:
json{
  "success": false,
  "message": "Invalid vehicle number format. Expected format: KA01AB1234"
}
Test Case 5: Duplicate Email
bash# First user
curl -X POST http://localhost:8080/api/user/save \
-H "Content-Type: application/json" \
-d '{
  "name": "User One",
  "email": "duplicate@example.com",
  "phone": "9876543210"
}'

# Second user with same email
curl -X POST http://localhost:8080/api/user/save \
-H "Content-Type: application/json" \
-d '{
  "name": "User Two",
  "email": "duplicate@example.com",
  "phone": "9876543211"
}'
Expected Response:
json{
  "success": false,
  "message": "Email already registered"
}

Postman Collection Setup
Environment Variables

base_url: http://localhost:8080
user_id: (Set after user creation)
vehicle_id: (Set after vehicle registration)
quote_id: (Set after provider selection)

Collection Structure
Bike Insurance APIs
├── User Management
│   ├── Save User
│   ├── Show User
│   └── Update User
├── Vehicle Management
│   ├── Register Vehicle
│   ├── Show Vehicle
│   └── Update Vehicle
├── Insurance
│   ├── Calculate Quotes
│   └── Select Provider
├── KYC
│   └── Upload Documents
└── Admin
    └── Get All Applications

Sample Test Data
Valid Users
json{
  "name": "Priya Sharma",
  "email": "priya@example.com",
  "phone": "9123456789"
}

{
  "name": "Amit Patel",
  "email": "amit@example.com",
  "phone": "8234567890"
}
Valid Vehicles
json{
  "userId": "user-id-here",
  "vehicleNumber": "MH12DE5678",
  "vehicleYear": 2019,
  "noOfDrivingAccidents": 0,
  "noOfDrivingViolations": 0,
  "coverageType": "Third Party",
  "coverageAmount": 50000.0,
  "coverageDeductibles": 2500.0
}

{
  "userId": "user-id-here",
  "vehicleNumber": "DL03CA9012",
  "vehicleYear": 2022,
  "noOfDrivingAccidents": 1,
  "noOfDrivingViolations": 2,
  "coverageType": "Comprehensive",
  "coverageAmount": 150000.0,
  "coverageDeductibles": 7500.0
}

Common Error Codes

400 Bad Request: Invalid input data or validation failure
404 Not Found: Resource not found
413 Payload Too Large: File size exceeds 5MB
500 Internal Server Error: Server-side error


Directory Structure for KYC Files
Create the following directory before running:
project-root/
└── uploads/
    └── kyc/
        ├── DL_*.pdf
        ├── RC_*.pdf
        └── AADHAR_*.pdf
Make sure the application has read/write permissions on this directory.






spring:
  application:
    name: bike-insurance-system

  datasource:
    url: jdbc:postgresql://localhost:5432/bike_insurance_db
    username: Tony
    password: your_password_here  # Replace with your actual PostgreSQL password
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: update
    properties:
      hibernate:
        dialect: org.hibernate.dialect.PostgreSQLDialect
        format_sql: true
        jdbc:
          lob:
            non_contextual_creation: true
    show-sql: true
    database-platform: org.hibernate.dialect.PostgreSQLDialect

  servlet:
    multipart:
      enabled: true
      max-file-size: 5MB
      max-request-size: 15MB
      file-size-threshold: 2KB

server:
  port: 8080  # Changed from 5432 (that's PostgreSQL port!)
  servlet:
    context-path: /

# Logging Configuration
logging:
  level:
    root: INFO
    com.bikeinsurance: DEBUG
    org.springframework.web: INFO
    org.hibernate.SQL: DEBUG
    org.hibernate.type.descriptor.sql.BasicBinder: TRACE

# Jackson Configuration
  jackson:
    serialization:
      fail-on-empty-beans: false
    default-property-inclusion: non_null

# Error Handling
  error:
    include-message: always
    include-binding-errors: always
    include-stacktrace: on_param
    include-exception: false

# HikariCP Configuration (Connection Pool)
  datasource:
    hikari:
      maximum-pool-size: 10
      minimum-idle: 5
      connection-timeout: 30000
      idle-timeout: 600000
      max-lifetime: 1800000













      -- =====================================================
-- BIKE INSURANCE SYSTEM - POSTGRESQL DATABASE SETUP
-- =====================================================

-- Drop database if exists (BE CAREFUL - This deletes all data!)
-- DROP DATABASE IF EXISTS bike_insurance_db;

-- Create database
CREATE DATABASE bike_insurance_db
    WITH 
    ENCODING = 'UTF8'
    LC_COLLATE = 'en_US.UTF-8'
    LC_CTYPE = 'en_US.UTF-8'
    TEMPLATE = template0;

-- Connect to the database
\c bike_insurance_db;

-- =====================================================
-- TABLE 1: USERS
-- =====================================================
CREATE TABLE users (
    id BIGSERIAL PRIMARY KEY,
    user_id VARCHAR(255) NOT NULL UNIQUE,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL UNIQUE,
    phone VARCHAR(15) NOT NULL,
    CONSTRAINT chk_email CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$'),
    CONSTRAINT chk_phone CHECK (phone ~ '^[6-9][0-9]{9}$')
);

CREATE INDEX idx_user_id ON users(user_id);
CREATE INDEX idx_email ON users(email);

-- =====================================================
-- TABLE 2: VEHICLES
-- =====================================================
CREATE TABLE vehicles (
    id BIGSERIAL PRIMARY KEY,
    user_id VARCHAR(255) NOT NULL,
    vehicle_number VARCHAR(20) NOT NULL UNIQUE,
    vehicle_year INTEGER NOT NULL,
    no_of_driving_accidents INTEGER NOT NULL DEFAULT 0,
    no_of_driving_violations INTEGER NOT NULL DEFAULT 0,
    coverage_type VARCHAR(50) NOT NULL,
    coverage_amount DOUBLE PRECISION NOT NULL,
    coverage_deductibles DOUBLE PRECISION NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_vehicle_user FOREIGN KEY (user_id) 
        REFERENCES users(user_id) 
        ON DELETE CASCADE 
        ON UPDATE CASCADE,
    CONSTRAINT chk_vehicle_year CHECK (vehicle_year >= 1900 AND vehicle_year <= EXTRACT(YEAR FROM CURRENT_DATE)),
    CONSTRAINT chk_accidents CHECK (no_of_driving_accidents >= 0),
    CONSTRAINT chk_violations CHECK (no_of_driving_violations >= 0),
    CONSTRAINT chk_coverage_type CHECK (coverage_type IN ('Comprehensive', 'Third Party')),
    CONSTRAINT chk_coverage_amount CHECK (coverage_amount > 0),
    CONSTRAINT chk_coverage_deductibles CHECK (coverage_deductibles > 0)
);

CREATE INDEX idx_vehicle_number ON vehicles(vehicle_number);
CREATE INDEX idx_vehicle_user_id ON vehicles(user_id);

-- =====================================================
-- TABLE 3: INSURANCE QUOTES
-- =====================================================
CREATE TABLE insurance_quotes (
    id BIGSERIAL PRIMARY KEY,
    user_id VARCHAR(255) NOT NULL,
    vehicle_user_id VARCHAR(255),
    selected_provider VARCHAR(100) NOT NULL,
    selected_plan VARCHAR(50) NOT NULL,
    premium_amount DOUBLE PRECISION NOT NULL,
    status VARCHAR(50) NOT NULL DEFAULT 'PENDING_KYC',
    applied_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_quote_user FOREIGN KEY (user_id) 
        REFERENCES users(user_id) 
        ON DELETE CASCADE 
        ON UPDATE CASCADE,
    CONSTRAINT fk_quote_vehicle FOREIGN KEY (vehicle_user_id) 
        REFERENCES vehicles(user_id) 
        ON DELETE CASCADE 
        ON UPDATE CASCADE,
    CONSTRAINT chk_premium CHECK (premium_amount > 0),
    CONSTRAINT chk_status CHECK (status IN ('PENDING_KYC', 'KYC_UPLOADED', 'APPROVED', 'REJECTED')),
    CONSTRAINT chk_plan CHECK (selected_plan IN ('Basic', 'Premium'))
);

CREATE INDEX idx_quote_user ON insurance_quotes(user_id);
CREATE INDEX idx_quote_status ON insurance_quotes(status);
CREATE INDEX idx_quote_applied_at ON insurance_quotes(applied_at);

-- =====================================================
-- TABLE 4: KYC DOCUMENTS
-- =====================================================
CREATE TABLE kyc (
    id BIGSERIAL PRIMARY KEY,
    insurance_quote_id BIGINT NOT NULL,
    driving_license_path VARCHAR(255) NOT NULL,
    rc_path VARCHAR(255) NOT NULL,
    aadhar_path VARCHAR(255) NOT NULL,
    verification_status VARCHAR(50) NOT NULL DEFAULT 'PENDING',
    uploaded_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT fk_kyc_quote FOREIGN KEY (insurance_quote_id) 
        REFERENCES insurance_quotes(id) 
        ON DELETE CASCADE 
        ON UPDATE CASCADE,
    CONSTRAINT chk_verification_status CHECK (verification_status IN ('PENDING', 'VERIFIED', 'REJECTED', 'NOT_UPLOADED'))
);

CREATE INDEX idx_kyc_quote ON kyc(insurance_quote_id);
CREATE INDEX idx_kyc_status ON kyc(verification_status);

-- =====================================================
-- TABLE 5: ADMIN USERS
-- =====================================================
CREATE TABLE admin_users (
    id BIGSERIAL PRIMARY KEY,
    admin_id VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    email VARCHAR(100) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT chk_admin_email CHECK (email ~* '^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}$')
);

CREATE INDEX idx_admin_id ON admin_users(admin_id);

-- =====================================================
-- INSERT SAMPLE DATA (Optional)
-- =====================================================

-- Sample Admin User (password should be hashed in production)
INSERT INTO admin_users (admin_id, password, email) VALUES
('admin001', 'admin123', 'admin@bikeinsurance.com');

-- Sample User 1
INSERT INTO users (user_id, name, email, phone) VALUES
('550e8400-e29b-41d4-a716-446655440000', 'Rajesh Kumar', 'rajesh@example.com', '9876543210');

-- Sample User 2
INSERT INTO users (user_id, name, email, phone) VALUES
('550e8400-e29b-41d4-a716-446655440001', 'Priya Sharma', 'priya@example.com', '9123456789');

-- Sample Vehicle 1
INSERT INTO vehicles (user_id, vehicle_number, vehicle_year, no_of_driving_accidents, 
                     no_of_driving_violations, coverage_type, coverage_amount, coverage_deductibles) 
VALUES
('550e8400-e29b-41d4-a716-446655440000', 'KA01AB1234', 2020, 0, 1, 'Comprehensive', 100000.00, 5000.00);

-- Sample Vehicle 2
INSERT INTO vehicles (user_id, vehicle_number, vehicle_year, no_of_driving_accidents, 
                     no_of_driving_violations, coverage_type, coverage_amount, coverage_deductibles) 
VALUES
('550e8400-e29b-41d4-a716-446655440001', 'MH12DE5678', 2019, 0, 0, 'Third Party', 50000.00, 2500.00);

-- Sample Insurance Quote
INSERT INTO insurance_quotes (user_id, vehicle_user_id, selected_provider, selected_plan, 
                             premium_amount, status) 
VALUES
('550e8400-e29b-41d4-a716-446655440000', '550e8400-e29b-41d4-a716-446655440000', 
 'Acko', 'Premium', 3680.00, 'PENDING_KYC');

-- =====================================================
-- USEFUL QUERIES FOR VERIFICATION
-- =====================================================

-- View all users
-- SELECT * FROM users;

-- View all vehicles with user details
-- SELECT v.*, u.name, u.email 
-- FROM vehicles v 
-- JOIN users u ON v.user_id = u.user_id;

-- View all insurance quotes with details
-- SELECT iq.*, u.name, u.email, v.vehicle_number
-- FROM insurance_quotes iq
-- JOIN users u ON iq.user_id = u.user_id
-- LEFT JOIN vehicles v ON iq.vehicle_user_id = v.user_id;

-- View KYC status
-- SELECT k.*, iq.selected_provider, u.name
-- FROM kyc k
-- JOIN insurance_quotes iq ON k.insurance_quote_id = iq.id
-- JOIN users u ON iq.user_id = u.user_id;

-- =====================================================
-- ADMIN QUERIES
-- =====================================================

-- Get all applications with user, vehicle, and KYC info
-- SELECT 
--     iq.id,
--     u.name AS user_name,
--     u.email AS user_email,
--     v.vehicle_number,
--     iq.selected_provider,
--     iq.selected_plan,
--     iq.premium_amount,
--     COALESCE(k.verification_status, 'NOT_UPLOADED') AS kyc_status,
--     iq.applied_at
-- FROM insurance_quotes iq
-- JOIN users u ON iq.user_id = u.user_id
-- LEFT JOIN vehicles v ON iq.vehicle_user_id = v.user_id
-- LEFT JOIN kyc k ON k.insurance_quote_id = iq.id
-- ORDER BY iq.applied_at DESC;

-- =====================================================
-- DATABASE STATISTICS
-- =====================================================

-- Count records in each table
-- SELECT 'Users' AS table_name, COUNT(*) AS record_count FROM users
-- UNION ALL
-- SELECT 'Vehicles', COUNT(*) FROM vehicles
-- UNION ALL
-- SELECT 'Insurance Quotes', COUNT(*) FROM insurance_quotes
-- UNION ALL
-- SELECT 'KYC Documents', COUNT(*) FROM kyc
-- UNION ALL
-- SELECT 'Admin Users', COUNT(*) FROM admin_users;

-- =====================================================
-- CLEANUP QUERIES (Use with caution!)
-- =====================================================

-- Delete all data (keeps table structure)
-- TRUNCATE TABLE kyc CASCADE;
-- TRUNCATE TABLE insurance_quotes CASCADE;
-- TRUNCATE TABLE vehicles CASCADE;
-- TRUNCATE TABLE users CASCADE;
-- TRUNCATE TABLE admin_users CASCADE;

-- Reset sequences
-- ALTER SEQUENCE users_id_seq RESTART WITH 1;
-- ALTER SEQUENCE vehicles_id_seq RESTART WITH 1;
-- ALTER SEQUENCE insurance_quotes_id_seq RESTART WITH 1;
-- ALTER SEQUENCE kyc_id_seq RESTART WITH 1;
-- ALTER SEQUENCE admin_users_id_seq RESTART WITH 1;

-- =====================================================
-- POSTGRESQL SPECIFIC FUNCTIONS
-- =====================================================

-- Function to get total premium by provider
CREATE OR REPLACE FUNCTION get_total_premium_by_provider(provider_name VARCHAR)
RETURNS DOUBLE PRECISION AS $$
DECLARE
    total DOUBLE PRECISION;
BEGIN
    SELECT SUM(premium_amount) INTO total
    FROM insurance_quotes
    WHERE selected_provider = provider_name;
    RETURN COALESCE(total, 0);
END;
$$ LANGUAGE plpgsql;

-- Usage: SELECT get_total_premium_by_provider('Acko');

-- =====================================================
-- VIEW FOR ADMIN DASHBOARD
-- =====================================================

CREATE OR REPLACE VIEW admin_dashboard AS
SELECT 
    iq.id,
    u.name AS user_name,
    u.email AS user_email,
    u.phone AS user_phone,
    v.vehicle_number,
    v.vehicle_year,
    iq.selected_provider,
    iq.selected_plan,
    iq.premium_amount,
    COALESCE(k.verification_status, 'NOT_UPLOADED') AS kyc_status,
    iq.status AS application_status,
    iq.applied_at
FROM insurance_quotes iq
JOIN users u ON iq.user_id = u.user_id
LEFT JOIN vehicles v ON iq.vehicle_user_id = v.user_id
LEFT JOIN kyc k ON k.insurance_quote_id = iq.id
ORDER BY iq.applied_at DESC;

-- Usage: SELECT * FROM admin_dashboard;

-- =====================================================
-- END OF SCRIPT
-- =====================================================




