# SFCL.125 Authorisation & Flight Signing

## Overview

UK SFCL / EASA regulations present two important compliance requirements for gliding clubs that Gliding.App helps to address. This document explains the optional compliance features available and how to configure them for your club.


## Legal background

The first legal item is **SFCL.125**, which states that pilots without a valid license must be authorised to fly and must fly under the supervision of an instructor. This applies to both student pilots flying **solo**, as well as licensed pilots who have run out of **recency** (e.g., insufficient hours or starts for a particular launch method, completing their flights under supervision of an instructor). There are multiple means to comply with SFCL.125. One way is to ensure that the instructor on duty authorises pilots via a pre-flight briefing. Gliding.App provides a specific means of compliance by allowing instructors to authorise pilots during the daily registration process. 

The second legal item combines pilot privileges and logbook requirements. **SFCL.115** states that pilots can only act as Pilot in Command if they hold a license and fulfill all recency requirements. Logbook regulations **SAO.GEN.160** require the signature of a Pilot in Command for each flight. Since student pilots and non-recent license holders cannot legally act as Pilot in Command (they act as Student Pilot in Command), the supervising instructor must sign their flights in the logbook.

## The Two-Step Compliance Process

Gliding.App provides two separate features to help clubs comply:

1. **Daily authorisation** - Instructors authorise pilots to fly on a given day
2. **Flight signing** - Instructors sign individual flights in the pilot's logbook after completion

Both features work together to provide a complete audit trail for regulatory compliance.

### 1. Daily Authorisation

Daily authorisation is a digital system where instructors authorise specific pilots to fly on a given day. This authorisation:

- Must be given **on the day of flying** (cannot be given in advance)
- Can include **limitations** (e.g., "Max wind 15kts, local flights only")
- Is **documented on both registration and flight levels** for audit trails
- Can be **revoked** by the authorising instructor if circumstances change

> **Note**: Daily authorisation is **optional**. Many clubs have alternative procedures in place to comply with SFCL.125 requirements. For example, a club can run a process in which the instructor on duty authorises pilots via a pre-flight briefing. Gliding.App provides an additional means of compliance, but it is not mandatory.

As a club administrator, you can enable this feature in your club settings, as well as setting your **default Limitations** (pre-defined limitation text that instructors can quickly select).

Two permission controls are in place that determine which groups can authorise and which groups require authorisation. By default, instructors are given the permission to grant authorisations. Solo pilots are assigned the requirement that they must receive authorisation. 

Built into the system is that licensed pilots who have run out of recency also require authorisation. A warning is automatically generated in the launch computer if a non-authorised or non-recent pilot wants to fly. 

Authorisation can only be granted on the same day of flying and only applies to that day. Authorisations cannot be granted for future days. Instructors can revoke authorisations if weather or other circumstances change.

An audit trail is created. Past authorisations are visible in both the daily report, as well as on flight level. 

### 2. Flight Signing

Instructors can sign flights for student pilots and licensed pilots flying under supervision (e.g., due to a lack of recency). Instructors can sign flights individually using their PIN code, or via the bulk signing tool in the daily report.

An audit trail is created for each flight, showing when and who signed the flight. Notes created during the signing process are automatically copied into the pilot notes section for efficiency.
