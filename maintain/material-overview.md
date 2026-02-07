# Material Overview

## Overview

The material library provides the foundation for aircraft and equipment management in GlidingApp. **Materials** represent all physical assets used in club operations, such as aircraft, winches, towcars, hangars, and other equipment. Each material serves as the central record linking flight logs, technical documentation, maintenance schedules, defect tracking, and work history. The system distinguishes between unlisted aircraft (minimal registration for flight logging) and full material records (complete maintenance tracking with parts, programs, workorders, and documents).

The material library follows a hierarchical data model: **Material** → **Parts** → **Maintenance Programs** → **Workorders** → **Documents**. Each material serves as the root entity containing all related technical records. **Parts** represent individual components tracked within a material (aircraft fuselage, engine, propeller, winch hooks). **Maintenance Programs** define scheduled inspection and overhaul intervals for parts. **Workorders** capture maintenance work performed, linking scheduled programs or defect repairs to completed tasks. **Documents** store certificates, manuals, and technical publications with expiration tracking.

This chapter describes the overview of the Material module, the remaining sub-modules are described in the next chapters.

## Core Concepts

### 1. Material Categories

Materials are classified into three **categories** that determine behavior, visibility, and available features:

| Parameter | Description | Example |
|-----------|-------------|---------|
| **Aircraft** | Flying equipment including tugplanes. Links to flight records and automatically tracks launches and hours. | ASK-21, Pawnee towplane |
| **Rolling** | Ground equipment used for launches or ground operations. Includes winches, towcars, and other motorized equipment. | Winch, retrieve vehicle, ground power unit |
| **Other** | Supporting infrastructure and non-mobile equipment. Buildings, hangars, trailers, and facilities not directly involved in flight operations. | Hangar, workshop, trailer, simulator |

Aircraft materials receive special treatment with configuration fields, motor hour tracking, and flight integration. Rolling and other materials use simplified tracking focused on maintenance intervals and document management without flight-related features.

### 2. Unlisted vs Listed Aircraft in Maintenance Module

Aircraft exist in two states within GlidingApp. **Unlisted aircraft** provide minimal registration for flight logging—only registration, callsign, type, and FLARM ID are captured (for OGN detection). These aircraft appear in flight selection menus but have no maintenance module access. Flight history accumulates but no parts, maintenance programs, workorders, or defects can be tracked.

**Listed aircraft** (full material records) activate the complete maintenance module. All flight history from the unlisted period transfers seamlessly during conversion. The system then enables parts library, maintenance program scheduling, workorder processing, defect tracking, document management, and team assignment. Private aircraft typically start unlisted to minimize administrative overhead for owners managing their own maintenance externally. Club aircraft and tow planes should always be entered as full materials to enable proper maintenance tracking and regulatory compliance.

### 3. Material Teams and Roles

Each material has a dedicated **team** with members assigned specific **roles** controlling access and responsibilities. Team membership operates independently from global user permissions—a club member without global maintenance permissions can be assigned as a material technician for specific equipment.

| Parameter | Description | Example |
|-----------|-------------|---------|
| **Admin/Owner** | Full control over all material aspects including settings, data, team membership, and configuration. Only role that can modify team composition. | Material owner, chief of maintenance |
| **Project Manager** | Manages maintenance programs, creates workorders, oversees hour registration. Primary contact for defect resolution coordination. Cannot modify team or core settings. | Winter maintenance coordinator |
| **Technician** | Responsible for maintenance execution. Must be member of the "technician" group to assign role in. | Licensed aircraft mechanic, A&P |
| **ARC Technician** | Authorized to perform Airworthiness Review Certificate inspections and sign ARC release documents. Special role for Part-ML compliance. | Part-ML certified inspector |
| **Project Member** | Read-mostly access with ability to view information, upload documents, and assist with workorders. Cannot create programs or assign work. | Club member|

Team visibility follows material category rules. Club aircraft teams extend visibility to all members for flight operations. Private aircraft teams restrict visibility to assigned members only unless the "visible to all technicians" flag is enabled.

### 4. Aircraft Configuration Fields

Aircraft materials include specialized fields integrating with flight operations and maintenance scheduling:

| Parameter | Description | Example |
|-----------|-------------|---------|
| **Material Visibility** | Club aircraft (category "club" or "tow") are visible to all members for flight operations. Private aircraft are only visible for team. The "visible to all technicians" flag overrides category rules to show material to all users with technician permissions. | Club, Private |
| **Launch Method** | Primary launch method: winch, aerotow, self-launch, bungee, car, TMG, SEP, or other. When flights auto-register at launch point or via OGN detection, this method applies by default. | winch, aerotow |
| **Motor Format** | Display format for motor hours: decimal (123.5), hh:mm (12:30), or hh:mm:ss (12:30:45). Controls input format in flight logging and daily inspections. | decimal |
| **Check-in** | Enables member check-in functionality linking to daily registration. Members can prepare flights in the grid before takeoff. | Enabled/Disabled |

### 5. Daily Inspections

Aircraft can require **daily inspections** before flight operations. The system enforces inspection completion and tracks who performed each inspection. Inspections consist of a customizable checklist. 

Pilots sign the inspection with their four-digit PIN, added as security verification measure. Inspection validity is per-day. For non-Hobbs configurations, motor hours can be captured during the inspection process rather than per-flight. Performing inspections requires the "May perform daily inspections" permission (generally all licensed glider pilots have this permission but a club may change this). The inspection list displays in the aircraft status view showing current validity and last inspector.

### 6. Archive and Export

Materials can be **archived** to create complete backup exports of all associated data. Archives include flights, parts, maintenance programs with all tasks, workorders with task completion records and uploaded files, defects with messages and attachments, documents with all versions, and hour registration records. The system generates a ZIP file with all uploaded files organized by type.

Archiving serves multiple purposes: annual record retention for regulatory compliance, material decommissioning documentation, or ownership transfer packages. You can **schedule ** a backup to send the sends archive files that night to a configured email. 

## Admin Actions

### Add Unlisted Aircraft

**What:** Register a basic aircraft for flight logging without activating the maintenance module  
**When:** Setting up private aircraft where owners manage maintenance externally, or temporarily adding guest aircraft to flight records  
**Permission:** "Can update equipment" (usually all admin members)

**How:**
1. Navigate to **Equipment** → **Overview**
2. Click the **Unlisted Aircraft** drawer button
3. Click **Add New**
4. Enter data and **Save**

The aircraft immediately appears in flight logging selection menus. Flight history accumulates but no maintenance tracking is available until conversion to a full material record.

### Convert Unlisted Aircraft to Material with Maintenance Module

**What:** Upgrade a basic aircraft registration to a full material record with maintenance tracking capabilities  
**When:** Private aircraft owner wants to track maintenance digitally, or club decides to manage guest aircraft maintenance in the system  
**Permission:** "Can update equipment"

**How:**
1. Navigate to **Equipment** → **Overview** → **Unlisted Aircraft** drawer
2. Locate the aircraft in the list
3. Click **Convert to Material** button

After conversion, the material gains access to Parts, Maintenance Programs, Workorders, Defects, and Documents tabs. All historical flights remain linked and continue contributing to hour/launch totals.

### Add New Material (Aircraft, Winch, or Other)

**What:** Create a new material record with full maintenance module from the start  
**When:** Adding club aircraft, purchasing new equipment, or setting up maintenance tracking for rolling/other materials  
**Permission:** "Can update equipment"

**How:**
1. Navigate to **Equipment** → **Overview**
2. Click **Add** button
3. Select category: **Aircraft**, **Rolling**, or **Other**
4. Enter data and **Save**

The system creates a material with full maintenance module enabled. Proceed to the **Data** tab to configure team members, daily inspection requirements, and initial hour values for existing equipment. For new equipment, initial values remain at zero.

