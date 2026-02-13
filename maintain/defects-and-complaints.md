# Defects and Complaints

## Overview

The defects and complaints system provides structured issue tracking for equipment and aircraft. **Defects** will ground aircraft immediately until repaired, while **complaints** allow technical review before action. Visual observations and general notes complete the tracking system without affecting operational status. Each issue maintains an audit trail from initial report through technician assignment, status transitions, and final resolution with complete message history and photo documentation.

The system integrates with material status calculations to automatically ground materials with open defects. Technicians manage issues through status transitions (reported, deferred, resolved, rejected) with each change creating audit messages. Email notifications alert assigned technicians and project teams. Defects can link to workorders for repair documentation, creating automatic references that satisfy EASA compliance requirements. The message thread supports collaboration between reporters, technicians, and project managers.

## Core Concepts

### 1. Defect Tracking Enable/Disable

Materials control defect tracking through the **enable defects** setting in the general configuration. When enabled, users can submit issues and technicians manage them through the complete lifecycle. When disabled, the defects tab is hidden and no issues can be reported for that material. 

### 2. Equipment Status Override

Equipment **status** is automatically calculated based on open defects and maintenance state, but can be manually overridden through the **manual_status** field:

| Parameter | Description | Example |
|-----------|-------------|---------|

| **Automatic (empty)** | System determines status based on open defects. Equipment with open defects are "not operational", otherwise "in service". | in service, not operational |
| **in maintenance** | Manual override marking equipment as in maintenance. Overrides system calculation. Equipment remains available for planning but marked as undergoing work. | Winter maintenance period |
| **not operational** | Manual override marking equipment as not operational. Prevents use regardless of defect state. Use for extended downtime or decommissioning. | Awaiting parts, annual inspection |


### 3. Defect Categories

Issues are classified into four **categories** that determine workflow, visibility, and material grounding:

| Parameter | Description | Example |
|-----------|-------------|---------|
| **defect** | Airworthiness defect grounding the aircraft immediately. Equipment status becomes "not operational" until resolved. Requires workorder or external repair documentation for closure. | Cracked canopy, control system malfunction, engine failure |
| **complaint** | Operational complaint requiring technical attention but not immediately grounding the equipment. Can be deferred with technician approval. EASA ML.A.403 allows recording observations not considered defects. | Noisy bearing, sluggish controls, minor wear |
| **visual** | Cosmetic or visual issue not affecting airworthiness or operation. Tracked for maintenance planning but doesn't affect equipment status. | Paint damage, interior wear, minor scratches |
| **general** | General observation, suggestion, or non-technical issue. Used for documentation or communication without technical implications. | Manual is updated, general usage |

Only defects ground aircraft automatically. Complaints, visual issues, and general observations do not affect operational status. Technicians can reclassify categories after initial report, with changes logged in the message thread. Per EASA ML.A.403, complaints and observations provide a mechanism for recording performance issues not constituting airworthiness defects.

### 4. Defect Lifecycle and Status Transitions

Defects progress with each transition creating audit trail entries:

| Parameter | Description | Example |
|-----------|-------------|---------|
| **reported** | Initial state when issue is submitted. Defects in this state ground the equipment. Awaits technician review and action. | Newly submitted defect |
| **deferred** | Technician has reviewed and approved deferral. Only available for complaints, not defects. Equipment remains operational with documentation. | Minor issue deferred to winter maintenance |
| **resolved** | Issue has been repaired and closed. Requires workorder reference for defects. Equipment returns to service. Archive state. | Repaired in workorder 2026-003 |
| **rejected** | Issue determined to not require action. Requires explanation message. Does not affect equipment status. Archive state. | Not reproducible, user error, duplicate report |

All status transitions require technician or project team member permissions. Each transition creates a message documenting the change, user, and timestamp. Defects can be reopened from resolved or rejected states if problems recur. 

### 5. Defect Assignment and Notifications

**Defects are assigned to teammembers** for resolution tracking. All project team members on the equipment also receive notifications for all new defects.

Equipment can specify a **default assignee** who automatically receives new defects when no explicit assignment is made. The assignment field can be changed at any time to redirect responsibility. 

## Admin Actions

### Add Defect or Complaint

**What:** Report a new issue, defect, complaint, or observation for equipment  
**When:** Discovering equipment problems during operations, post-flight inspections, or routine checks  
**Permission:** Authenticated user (all logged-in users can report issues)

**How:**
1. Navigate to equipment → **Status** tab
2. Click **Add Report** button
3. Select equipment and enter issue details
4. Attach photos if needed to document the issue
5. Click **Save**

The system creates the defect in "reported" state and sends email notification to the assigned technician and all project team members. If the category is "defect", the equipment status immediately changes to "not operational".

### Modify Defect Details (as Technician)

**What:** Update defect category, assignment, or lifecycle after initial report  
**When:** Reclassifying issue severity, reassigning to different specialist, correcting equipment reference, or changing status  
**Permission:** Technician (flying or non-flying) or equipment project team member

**How:**
1. Open defect detail page
2. Click **Edit** button
3. **To change category:** Select new category (Defect/Complaint/Visual/General)—system logs the change in message thread
4. **To reassign technician:** Select different technician from dropdown—triggers notification email to new assignee
5. **To change lifecycle:** Use status buttons to defer, resolve, or reject
6. Click **Save**

Category changes create automatic audit messages documenting the reclassification. When changing from complaint to defect, the equipment grounds immediately if status is still "reported". Reassignment notifications include defect details and link to the issue.

