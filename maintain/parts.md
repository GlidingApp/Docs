# Parts

## Overview

The Parts Library manages the component structure of aircraft and equipment. Each piece of equipment can have a hierarchy of parts representing its physical structure—from the complete airframe down to individual assemblies and components. The system tracks installation status, service life limits, and critical (MEL) designations for each part.

## Core Concepts

### 1. Parts Hierarchy

Parts are organised in a tree structure with the aircraft as the root component. Each part can have child parts, creating a nested hierarchy that represents the physical structure of the equipment. For aircraft, the system automatically creates the top-level 'Aircraft' part, plus 'Engine' and 'Propeller' parts for motorised configurations. The hierarchy enables tracking component relationships and inheritance of maintenance requirements. Parts can be moved within the hierarchy or transferred to different equipment. The top-level aircraft part automatically accumulates flight hours and launches from the logbook.

### 2. In-Service Lifetime Tracking

Parts with limited service life require tracking of usage against manufacturer limits. Enable **track inservice life** to automatically calculate total launches, flight hours, and motor hours based on flight records and initial values. The system counts flights after the calibration date and adds them to the initial values.

| Parameter | Description | Example |
|-----------|-------------|---------|
| **Initial Date** | Calibration date when the part was last overhauled or installed. System counts all flights after this date toward the part's lifetime. | 2023-01-15 (after 100-hour inspection) |
| **Initial Launches** | Optional. Total launches at the calibration date. System adds flights since calibration date to this value. | 1250 launches (at overhaul) |
| **Initial Hours** | Optional. Total flight hours (in minutes) at calibration date. System adds flight time since calibration date. | 7800 minutes (130 hours at overhaul) |
| **Initial Motor Hours** | Optional. Total motor hours at calibration date for powered aircraft. Uses inspection records for motor time. | 4500 minutes (75 motor hours) |

### 3. Critical Components and MEL

Parts marked as **is MEL** (Minimum Equipment List) are critical for airworthiness. MEL parts display with red tags and affect equipment availability if defective. Use this flag for safety-critical components that ground the aircraft if unserviceable—control cables, canopy latch, tow release, main wheels, airspeed indicator. Non-MEL parts may have defects without grounding the aircraft. The MEL flag helps technicians prioritize repairs and understand grounding implications.

### 4. Part Installation Status

Parts track installation state with **is installed** flag and date_installed/date_uninstalled fields. Uninstalled parts stop accumulating flight hours and launches. The system preserves lifetime totals when parts are removed, enabling accurate tracking when components are refurbished or transferred. When uninstalling a part, the system freezes current totals as new initial values. This ensures lifetime tracking continues accurately when the part is reinstalled on the same or different aircraft. Uninstalled parts remain in the hierarchy but display as 'Not Installed'.

### 5. Workshop and Part Movement

For parts with limited service life that rotate between aircraft (e.g. tow release hooks), the advice would be to create a **Workshop** part (or equipment) to serve as a holding location. Parts can be moved between equipment whilst preserving their complete maintenance history, in-service lifetime, and associated maintenance programmes. Moving parts transfers the part, all child parts, maintenance programmes, and documents to the destination equipment. The lifetime tracking continues seamlessly. This enables clubs to maintain a pool of refurbished components and track their usage across multiple aircraft without losing history.

### 6. Part Documentation

Parts can link to equipment documents, particularly EASA Form-1 (Authorized Release Certificates) and manufacturer data sheets. These document links provide quick access to airworthiness certificates and technical specifications during maintenance planning and workorder execution. Document links are many-to-many relationships, allowing parts to reference multiple documents and documents to apply to multiple parts. Common document types: EASA Form-1 for new/overhauled parts, manufacturer service bulletins, installation records, and certification data sheets.

### 7. Master Parts and Automatic Setup

The system uses **Master Part** designations ('hull', 'engine', 'propeller') for key aircraft components that appear on Certificate Release to Service (CRS) documents. When loading default parts for an aircraft, the system automatically creates these master parts with proper tracking configuration. The 'Load Default Parts' function reads aircraft configuration from registration data and creates the appropriate structure. For pure gliders, only the hull is created. For motorised aircraft, engine and propeller parts are added automatically. This ensures consistent part structure across all equipment.

## Admin actions

### Load default parts for aircraft

**What:** Initialize the parts hierarchy with aircraft, engine, and propeller components based on registration data  
**When:** First-time setup for newly added equipment, or after accidentally deleting parts structure  
**Permission:** "Equipment admin role" or "Can update equipment"

**How:**
1. Navigate in equipment → **Parts** tab
2. Click **Load Parts** button (only visible when parts list is empty)
3. All parts automatically configured with appropriate initial values from registration date
