# Workorders

## Overview

Workorders organize maintenance execution by grouping scheduled maintenance programs, defect repairs, and ad-hoc tasks into a single work package. When maintenance is due or during yearly maintenance cycles, technicians create a workorder to document work performed, track task completion, and generate the Certificate Release to Service (CRS) required by EASA regulations.

## Core Concepts

### 1. Workorder Execution Workflow

When maintenance is due or during yearly maintenance cycles, one or multiple maintenance schedules and defects/complaints can be selected to finalize in a workorder. The workorder groups all related maintenance tasks and defect repairs into a single execution package. After completion, a Certificate Release to Service (CRS) is generated according to ML.A.801 and stored in the documents library. Workorders connect scheduled maintenance programs, defects, and ad-hoc tasks into a unified work package. Each workorder has a state (active or completed), tracks task completion progress, and maintains audit trails of who performed and checked each task. The system ensures EASA compliance by requiring proper sign-offs and generating formal release certificates.

### 2. Task Hierarchy and Sign-off

Workorder tasks are hierarchical with parent tasks containing subtasks. Each task can be signed off individually by technicians with appropriate restrictions. **Critical tasks** require dual sign-off (technician signs, inspector checks). Tasks can require remarks or photo documentation. Tasks inherit from maintenance program templates and can be modified per workorder. Restrictions control who can sign off (everyone, technicians, POM technicians). Critical tasks (safety-related) require a second person to verify. Tasks track who signed, when, and any remarks or photos uploaded during execution.

### 3. Certificate Release to Service (CRS)

Upon workorder completion, the system generates a **Certificate Release to Service (CRS)** per EASA M.A.801(e).5. The CRS documents all work performed, parts installed, maintenance programs completed, defects resolved, and limitations. Technicians digitally sign the CRS with their AML (Aircraft Maintenance License) number and pincode. The CRS is a legally binding document certifying the aircraft is airworthy following maintenance. It references completed maintenance programs, resolved defects, installed parts, and any limitations. The PDF is automatically generated from workorder data and stored in the equipment documents library with the signing technician's name, AML number, and timestamp.

### 4. Airworthiness Review Certificate (ARC)

ARC inspections are special workorders for annual airworthiness reviews per EASA M.A.901. ARCs verify overall aircraft airworthiness, not specific maintenance tasks. The system generates **Airworthiness Review Certificates** signed by authorized inspectors with their AML number. ARC workorders have simplified task structures focusing on overall aircraft condition review rather than detailed maintenance tasks. Only authorized ARC inspectors can complete and sign ARCs. The generated ARC certificate documents the review date, inspector details, and any findings or limitations discovered during the review.

### 5. Task Restrictions and Permissions

Tasks have **restriction** settings controlling who can sign them off. Restrictions range from 'everyone' (pilot-owner maintenance) to 'technicians' to 'POM technicians' (Part-ML certified). Critical tasks require dual sign-off with check permission.

| Parameter | Description | Example |
|-----------|-------------|---------|
| **Everyone** | Any member can sign off. Used for pilot-owner maintenance tasks allowed under Part-ML (cleaning, minor adjustments, daily inspections). | Clean canopy, check tire pressure, daily pre-flight inspection |
| **Technicians** | Requires technician role. Used for maintenance requiring technical knowledge but not Part-ML certification. | Replace winch cable, adjust control cables, minor repairs |
| **POM Technicians** | Requires Part-ML certified technician. Used for airworthiness-critical maintenance requiring licensed technician per EASA regulations. | Annual inspection sign-off, control system rigging, structural repairs |

### 6. Workorder Documents and Limitations

Workorders can link to equipment documents (maintenance manuals, service bulletins, parts documentation) for reference during execution. When tasks are deferred or work is incomplete, **limitations** must be documented explaining what was not completed and any operating restrictions. Document links provide technicians quick access to procedures and specifications. Limitations are mandatory when completing a workorder with unsigned tasks - they document what was deferred, why, and any restrictions on aircraft operation until the work is completed. Limitations appear on the CRS and affect equipment availability status.

## Admin actions

### Complete workorder and generate CRS

**What:** Finalize workorder, sign off remaining tasks or defer with limitations, generate Certificate Release to Service  
**When:** All maintenance tasks completed, defects resolved, ready to return aircraft to service  
**Permission:** "POM technicians" (for aircraft) or "Technicians"

**How:**
1. Navigate to equipment → **Workorders** tab → Select workorder
2. Click **Complete** button (only enabled for technicians with appropriate restriction)
3. Review task completion status: signed tasks (green check), critical tasks checked (double check), unsigned tasks
4. For unsigned tasks: either sign them off individually, or choose to sign all remaining tasks, or defer with limitations
5. If deferring tasks: document **Limitations** explaining what was not completed and any operating restrictions
6. For connected maintenance programs: confirm maintenance is complete and set next due date (system adds interval to current values)
7. For connected defects: confirm defects are resolved and ready to close
8. System validates all critical tasks have dual sign-off, generates completion data
9. Workorder state changes to completed, tasks locked from further editing
10. Ready for CRS signing step

### Sign Certificate Release to Service (CRS)

**What:** Digitally sign CRS with AML/Brevet number and pincode to certify aircraft airworthiness  
**When:** Workorder completed, all tasks signed off or deferred with limitations, ready to release aircraft to service  
**Permission:** "POM technicians" (AML number required for AMP), "Technicians" (for non-aviation)

**How:**
1. Navigate to equipment → **Workorders** tab → Select completed workorder
2. Click **Sign CRS** button (only enabled after workorder completion)
3. Read certification statement: 'I certify that specified work has been completed per Part-ML and aircraft can be released to service'
4. Check certification checkbox to confirm
5. System displays your AML number (Aircraft Maintenance License) or Brevet number from profile
6. Enter 4-digit pincode for authentication and finalise signing
7. System generates CRS PDF with workorder details, tasks completed, parts installed, defects resolved, limitations, lifetime totals