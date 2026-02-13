# Maintenance Schedule

## Overview

The Maintenance Schedule manages when aircraft maintenance is due and what tasks must be performed. Each maintenance program defines scheduled tasks linked to a specific part (aircraft, engine, propeller, or component) and tracks intervals based on calendar time, launches, flight hours, or motor hours. The system implements EASA M.A.302 requirements for Aircraft Maintenance Programmes (AMP).

## Core Concepts

### 1. ML.A.302 Aircraft Maintenance Programme (AMP)

Maintenance of each aircraft shall be organised in accordance with an **Aircraft Maintenance Programme** (AMP) per EASA ML.A.302. The maintenance program is broken down in the GlidingApp into **maintenance schedules**, referring to one set of scheduled maintenance tasks linked to a specific part (aircraft, engine, propeller, or component). The schedule defines when maintenance is due and what tasks must be performed.

### 2. Schedule Types: Recurring vs Lifetime

Maintenance schedules use two **schedule types**. Recurring programs repeat at regular intervals. Lifetime programs track usage until a component reaches end-of-life limits.

| Parameter | Description | Example |
|-----------|-------------|---------|
| **Recurring** | Maintenance repeats at regular intervals. After completion, the system calculates the next due date/hours/launches by adding the interval to current values. Used for inspections, checks, and periodic servicing. | Annual ARC inspection (12 months), 100-hour inspection |
| **Lifetime** | Maintenance tracks usage until component reaches manufacturer limits. One-time task that doesn't recur. Used for components with fixed service life that must be replaced or overhauled. | Parachute replacement at 12 years |

### 3. Maintenance Intervals: Date, Launches, Hours, Motor Hours

Maintenance can be scheduled on multiple **interval types** simultaneously. The system tracks all enabled intervals and flags the program as due when any interval expires. This supports EASA requirements for 'whichever occurs first' scheduling.

| Parameter | Description | Example |
|-----------|-------------|---------|
| **Date Interval** | Calendar-based interval in months. System calculates next due date by adding months to current date. Set alert period in days before due date. | 12 months for annual ARC |
| **Launches Interval** | Launch-based interval. Requires part with in-service lifetime tracking enabled. System compares current launches against next due value. | 2,000 launches for nose hook replacement |
| **Hours Interval** | Flight time interval in hours. Requires part with in-service lifetime tracking. System accumulates flight time from logbook and compares to limit. | 3,000 hour hull maintenance interval |
| **Motor Hours Interval** | Motor running time for powered aircraft. Requires part tracking motor hours from inspection records. | 30 hour overhaul of propeller |

### 4. Pre-loaded Tasks

Maintenance programs contain pre-defined task lists that specify the work to be performed. Tasks can be hierarchical (grouped under headings) and include descriptions, restrictions (who can sign off), critical flags, and documentation requirements. Tasks are copied from templates or created manually. When adding a maintenance program, select from existing templates (previously created programs on other aircraft) or import from shared drive. Tasks define the scope of work for workorder execution. Tasks can require remarks, photos, or measurements.

## Admin actions

### Import maintenance program from shared drive

**What:** Load pre-built maintenance program templates (MIP, manufacturer AMPs, common inspections) from shared drive folder  
**When:** Initial setup for new aircraft, adding standard inspection programs, implementing EASA Minimum Inspection Programme  
**Permission:** "Can update equipment"

**How:**
1. Navigate to equipment → **Maintenance** tab
2. Click **Import** button
3. Download desired maintenance program file (.txt format) from shared drive folder (link provided in modal)
4. Select downloaded file for import
5. After import: assign each program to correct part, configure intervals, and link documents
6. Review and adapt imported tasks to match your aircraft configuration

### Export maintenance programs to shared drive

**What:** Share maintenance program templates with other clubs via shared drive  
**When:** Contributing useful programs to community, backing up custom programs, sharing manufacturer-approved AMPs  
**Permission:** "Can update equipment"

**How:**
1. Navigate in equipment → **Maintenance** tab
2. Click **Export** button
3. Select programs to export (can select multiple)
4. System generates .txt file containing program structure, tasks, intervals, and settings
5. Upload file to shared drive folder for community access