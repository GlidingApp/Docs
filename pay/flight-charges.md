# Flight Charges

## Overview

**Flight Charges** automatically calculate and apply billing for flight operations based on configurable products. Each product defines which flights to charge (via filters), which members to charge (based on paying member assignment and group membership), and how much to charge (via pricing rules). The system runs calculations on flight arrival, and nightly to accommodate same-day flight corrections.

## Core Concepts

### 1. Flight Charge Flow

**Flight charge flow** applies products to flights through three sequential filters. First, the **account holder filter** determines whether the product charges user accounts or voucher accounts. For user accounts, the system identifies the paying member from the flight (or splits 50/50 for dual flights) and checks membership type and group restrictions. Second, the **flight characteristics filter** evaluates flight properties (start method, aircraft category, departure time, altitude, duration, training flags) using AND-logic across all defined filters. Third, the **price calculation** applies matching pricing rules to determine the charge amount. The system evaluates all active products nightly and creates ledger entries for each matching flight-product combination.

### 2. Product Types

**Products** are billing configurations that define what to charge, who to charge, and how much to charge. GlidingApp supports **periodic** (recurring membership fees) and **flight-based** (automatic charges tied to flight data). Flight-based products are the core of this module and execute nightly via scheduled task. 

### 3. Group-Based Pricing

**Group-based pricing** enables differentiated rates for member categories such as juniors, students, or regular members. Clubs with such pricing can duplicate flight products and apply different group filters to each copy. The system supports filtering down on **membership** or **groups** (filtered or excluded), admin's can choose either or both depending on their specific need. 

### 4. Flight Filters

**Flight filters** determine which flights match a product. Each filter specifies a **field** (flight property), **operator** (comparison type), and **value** (comparison target). Multiple filters combine with AND logic—all filters must match for the product to apply. Filters can target flight data (date, start method, training flags, voucher presence) or aircraft properties (category, classification, number of seats). Common patterns include excluding voucher flights (as they are priced separately), selecting launch method or filtering by aircraft category.

### 5. Flight Pricing Rules

**Flight pricing rules** calculate the charge amount for matching flights. Each rule has a **base price** (fixed component) and **price per unit** (variable component multiplied by flight metrics). The **unit** parameter determines what to multiply by: "start" (per launch), "minute" (flight duration), "motor" (motor runtime for TMG), "100m" or "100f" (altitude in meters/feet). Rules can include optional filters to create conditional pricing (e.g., different rates before/after specific times or above/below altitude thresholds). 

Pay attention when adding multiple flight pricing rules. When multiple rules are applied, multiple prices may be added to the flight.

In a 'simple' glider hire scenario with a cap, you must make sure that the flight pricing rules are mutually exclusive. For example, the first rule charges a base price per minutes under 180 minutes. The second rule charges a flat price for flights over 180 minutes.

There are scenario's where you'd like to add multiple prices, for example for TMG hires. A first rule charge is a flat initial hire. The second rule charges a base price per minute. 

### 6. Two-Flight Aerotow Model

The GlidingApp **Two-flight aerotow model** represents each aerotow as separate flight records for the glider and the tug. The glider flight has start method is "tow" and the tug flight has start method is "tmg-a" (touring motor glider) or "sep-a" (single-engine piston). Clubs implementing aerotow charges must create products that filter for tug flights (start method in ['tmg-a', 'sep-a']) and typically restrict to tow aircraft (category = 'tow'). 

The paying member assignment synchronizes between connected flights, ensuring the glider pilot gets charged for their tow service. This architecture enables billing for tow service (charged to glider pilot), plus independent tracking of tow aircraft operations.

:::info Migrating to Two-Flight Aerotow Billing
Clubs migrating from single-record systems typically import only glider "tow" flights initially. For financial implementation, create a product that charges tug flights (tmg-a/sep-a start method) at the desired rate structure. 
:::

### 7. Nightly Calculation Cycle

**Nightly calculation cycle** runs flight charge calculations via scheduled task every night. This delayed processing accounts for same-day flight corrections made during the evening. The system processes all active products, evaluates filters against recent flights, and creates or updates ledger entries. When flights are modified after initial calculation, the system automatically recalculates charges for affected flights unless the ledger entry was manually overridden. The calculation respects the product's **date last run** to avoid duplicate processing. 

### 8. Manual Price Override

**Manual price override** enables admins to adjust automatically calculated charges for special circumstances. The ledger entry stores two amounts: **amount calculated** (original automatic calculation) and **amount** (final billable amount). When an admin modifies the amount field, the system preserves the calculated value and marks the entry as manually overridden. Members viewing their account see both the original and adjusted amounts when overrides exist. Manual overrides prevent automatic recalculation when the source flight is updated, maintaining the admin's adjustment regardless of subsequent flight changes.

### 9. Testing Products

**Product testing** validates filter logic and pricing calculations without affecting production ledgers. The system provides two testing approaches that operate independently from test mode's member visibility controls. 

**Single product testing** evaluates one product in isolation, showing the last 15 matching flights from the past 30 days with calculated charges and detailed pricing breakdowns. Expand each flight to view the pricing rule components (base price, per-unit calculations, rule descriptions). 

**Full day testing** simulates nightly calculation for a complete flying day, processing all active products against all flights from the selected date. This reveals product interactions, missing coverage, and duplicate charges. The full day test displays flights grouped by product with total charges per flight and expandable line-item details. 

Use single product testing to validate filter logic and pricing rules during initial configuration. Use full day testing to verify complete system behavior before production deployment.

### 10. Calculate Historic Flights

**Historic flight calculation** enables retrospective product application by modifying the **date last run** timestamp. When a product's date last run is set to a past date, the nightly calculation processes all flights between that date and the current date on the next execution cycle. This enables testing product configurations against historical data, recalculating charges after pricing adjustments, or backfilling charges for newly created products. 

Set date last run to the desired starting point via product edit interface. You can then wait for the nightly calculation, or trigger a re-run of the calculations via the button **Execute calculation**.

The system creates ledger entries for all matching flights in the date range. Historic calculation respects existing manual overrides—flights with manually adjusted charges are not recalculated. 


### 11. Reset Product

**Product reset** deletes all ledger entries created by a specific product, enabling clean configuration iteration during implementation. The reset function **only operates while test mode is active**.

To reset a product, navigate to product detail view, scroll to the danger zone section, and click **Reset Product**. The system displays a confirmation modal requiring explicit confirmation before deletion. Upon confirmation, all ledger entries with the product's ID are permanently removed and member balances recalculate excluding the deleted charges. 

Once test mode is disabled and the financial system is live for members, product reset becomes unavailable to prevent accidental deletion of production billing data.
