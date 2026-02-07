# DTO Overview

## Overview

A Declared Training Organisation (DTO) is the legal framework under EASA and UK SFCL regulations for any club conducting gliding instruction. Every club operating under national association rules functions as a DTO, making the club legally responsible for maintaining instructor qualifications, verifying medical certificates, tracking student progression, and retaining training records. The GlidingApp implements the infrastructure needed to meet these regulatory requirements automatically through flight administration.

The system monitors instructor recency from logged flight instruction, flags medical certificates requiring verification, generates permanent student records at program completion, and provides chief instructors with oversight dashboards. These features integrate with flight administration, training progression cards, and the rostering system to create an automated compliance framework requiring minimal manual intervention.

## Core Concepts

### 1. DTO Regulatory Framework

The system implements SFCL DTO.GEN.220 requirements for maintaining student records, license information, and medical certificate expiry dates. Each club operates as a DTO with full responsibility for training quality and regulatory compliance. The infrastructure provides:

- Instructor recency tracking from flight administration data
- Medical certificate verification workflow with digital signatures
- Student progression tracking through competency-based programs
- Automated 3-year record retention with protected file exports
- Chief instructor oversight dashboards with compliance alerts

### 2. Instructor Recency Monitoring

DTOs must ensure all instructors maintain valid rolling recency per SFCL.360 (FI(S)) and UK SFCL.365 (BI(S)). The system automatically calculates recency from logged flights where the instructor is designated as PIC on flights marked FIS (Flight Instruction). Recency calculations include automated weekly email alerts to chief instructors when recency approaches expiration.

See [Rolling Recency](/pages/comply/rolling-recency) for detailed recency calculation rules and admin workflows.

### 3. Medical Certificate Verification

Medical verification follows a three-stage workflow: pilot uploads certificate and enters validity dates → chief instructor reviews and digitally signs verification → system stores verification history for 4 years. The verification applies to:

| Pilot Category   | Verification Required | Configurable       |
| ---------------- | --------------------- | ------------------ |
| Instructors      | Always                | No                 |
| Solo students    | Always                | No                 |
| All other pilots | Optional              | Yes (club setting) |

Clubs can enable **Medical Must Check All** in their club settings to require verification for all members. This is a club policy decision beyond minimum regulatory requirements. The system sends automated email alerts to chief instructors when new medical certificates require verification. Once verified, the medical verification record includes the date checked, verifying instructor name, and certificate validity period.

Medical verification history is retained for 3 years to satisfy the DTO.GEN.220 minimum requirement. This history is visible to chief instructors through the medical verification modal and provides an audit trail for regulatory inspections.

### 4. Student Training Records

DTO.GEN.220 requires maintaining complete training records for 3 years after program completion or discontinuation. When a chief instructor marks a DTO training program as completed or discontinued, the system:

1. Generates an Excel export containing all competency achievements with dates, scores, and instructor names
2. Stores the export file in the student's **Files** tab with read-only protection
3. Records program start date, completion/discontinuation date, and the instructor who performed the action
4. Prevents deletion of read-only files by students or non-admin users

The Excel export serves as the permanent record satisfying regulatory retention requirements. The file remains accessible to the student and all instructors with DTO permissions. If a student account is deactivated, their files remain accessible through the admin interface.

### 5. DTO Aircraft Designation

Aircraft can be designated as DTO training aircraft using the **in_dto** flag, which affects automatic flight type assignment. By default, all two-seater gliders are marked as DTO aircraft. When an instructor flies as PIC on a DTO aircraft with any second occupant, the flight automatically receives FIS (Flight Instruction) designation.

FIS-designated flights count toward instructor recency calculations and contribute to student training records.

### 6. Training Program Structure

Training programs follow EASA/UK SFCL syllabus requirements and can be imported from national associations (BGA, KNVvL, LVZC) or created from community templates. Programs are structured with categories containing ordered competencies. 

The system tracks program version for curriculum updates and links programs to EASA training types for compliance reporting. Instructors mark competencies as achieved, and specific competencies can trigger automatic recency activation when they link to license privileges. See [DTO Progression Cards](/pages/train/dto-progression-cards) for complete training program documentation.

## Admin Actions

### Verify Medical Certificate

**What:** Review and digitally sign medical certificate verification  
**When:** After a member uploads a medical certificate or updates validity dates. The system sends an automated email to chief instructors alerting them of pending verifications.  
**Permission:** "Sees DTO instructor pages" (typically chief instructors only)

**How:**
1. Navigate to the student/instructor profile
2. The orange medical tag **Check** button indicates pending verification
3. Click the medical verification button to open the **Medical Verification** modal
4. Review the validity dates (adjust if needed using the date range picker)
5. If a medical file is attached, click the link to review the uploaded certificate
6. Once the verification has been saved, the history of medical expiration dates is shown   

### Review DTO Compliance Dashboard

**What:** Monitor instructor recency status, pending medical verifications, and training program completions  
**When:** Weekly compliance review. Before scheduling instructor duty rosters. When preparing for regulatory audits.  
**Permission:** "Sees DTO instructor pages"

**Available actions:**
- **View instructor recency status:** Green/amber/red indicators show recency compliance for all instructors with upcoming expiration dates
- **See pending medical verifications:** List of pilots awaiting chief instructor signature on medical certificates
- **Monitor training programs:** Students approaching completion, discontinued programs requiring record export


### Designate DTO Aircraft

**What:** Configure which aircraft trigger automatic FIS designation for instructor flights  
**When:** Adding new aircraft to fleet. Excluding recreational two-seaters from training designation. 
**Permission:** "Can update equipment"

**How:**
1. Navigate to **Equipment** → **Aircraft** → select aircraft
2. Locate **DTO Aircraft** checkbox (in_dto flag)
3. **Enabled (default for club two-seaters):** Any flight with instructor as PIC and second occupant automatically receives FIS designation
4. **Disabled:** Flights require manual FIS designation or are not counted toward instructor recency
5. Setting affects automatic recency calculations and training record generation
