# Financial plan

## Overview

The GlidingApp **Financial plan** establishes member billing, payment collection, and accounting integration for clubs transitioning to automated financial management. The implementation process involves configuring products (flight charges and periodic fees), enabling payment methods (bank transfers or online payments), and establishing accounting workflows. GlidingApp's test mode enables risk-free configuration validation before exposing the financial system to members.

## Core Concepts

### 1. Balance-Based Accounting

**Balance-based accounting** maintains a running ledger for each member, aggregating all financial transactions into a single balance figure. Flight charges, periodic membership fees, and manual adjustments create negative entries that reduce the balance. Payments (bank transfers or online transactions) create positive entries that increase the balance. The system calculates current balance by summing all ledger entries for the account holder. This single-ledger approach eliminates reconciliation between multiple accounting records—GlidingApp's ledger balance is the authoritative financial state. Members view their balance, transaction history, and payment options via the account page. Administrators view all member balances in the accounts overview, with search, filter, and export capabilities.

### 2. Test Mode

**Test mode** isolates financial system configuration from member visibility during setup and validation. When test mode is active, all administrative functions operate normally: products calculate nightly, charges appear in ledger entries, administrators view balances and reports, and test product features work as expected. However, member-facing displays hide financial information: account pages show no balance, transaction history remains inaccessible, payment buttons disappear, and balance reminder emails do not send. This enables administrators to configure products, import starting balances, validate calculations against historical data, and refine pricing rules through multiple iterations without confusing members or generating false debt notifications. Disable test mode in **Financial** → **Settings** → toggle **Test Mode** off to make the financial system visible to members.

### 3. Payment Methods

**Payment methods** determine how members fund their accounts. GlidingApp supports two collection approaches with different administrative overhead and member convenience trade-offs. **Bank transfers** require members to transfer funds directly to the club's bank account using reference numbers. Administrators import bank statements monthly or quarterly via **Ledgers** → **Import Bank Statement**, supporting MT940 (European standard), OFX (international standard), and CSV formats. The import allows administrators to reconcile transactions to member accounts using name matching suggestions. 

**Online payments** integrate with Mollie payment provider, enabling members to pay directly from their account page via activated payment methods (e.g. Pay by Bank, PayPal, Ideal). Please note that transaction costs will apply, refer to [Mollie pricing](https://www.mollie.com/pricing) for current transaction fees. UK clubs benefit from Mollie's Pay by Bank option, which offers lower transaction fees than card payments.

**Direct Debit** can be activated in the account. Either [SEPA](https://docs.mollie.com/docs/sepa-direct-debit) for EURO based payments, or [BACS](https://docs.mollie.com/docs/bacs) for pound based payments. 

### 4. Financial Reports

**Financial reports** export financial data for external accounting system integration. Reports generate as Excel files from **Financial** → **Settings** → **Reports** → select a year, month, week or date range → **Download**. Each report contains user valance, flights, vouchers, transactions and journal exports (summarized and detailed).

#### Export to Xero

Besides the Excel report, GlidingApp exports a **Xero Journal summary**: a CSV file in Xero's manual journal import format. It does not export each transaction. Instead, a period is summarized into balanced double-entry journals, where the amounts are summed per account and tax rate. A week with hundreds of flights, payments and fees therefore becomes a few dozen lines in Xero. The Excel export contains the journal worksheets which includes a check table that proves total debits equal total credits, and that the member mutations in the journal match the user_balances worksheet. This provides a complete auditable datatrail.

Lines are booked to the ledger accounts of your own chart of accounts. Revenue and expense lines use the **Ledger account** of each product, and the system accounts (member receivables, bank, payment clearing, payments in transit, VAT payable, VAT receivable and manual adjustments) use the **Default ledger accounts** card in **Financial** → **Settings**. Note: Each of these default accounts needs a ledger code before the export can be downloaded. Products without a ledger account are exported with an empty account code, which Xero rejects on import, so fill this in for every product.

The values are exported **excluding VAT**. Each revenue or expense line carries the net amount and the name of the Xero tax rate, such as "20% (VAT on Income)", "Zero Rated Income", "Exempt Income" or "No VAT" (and the "Expenses" variants for declarations). Xero calculates and books the VAT itself. The member receivable is exported as two rows, a **Total net amount** and a **Total tax amount**, so that the journal balances once Xero has added the VAT. This means the rows in the file do not sum to zero by themselves; they sum to the VAT that Xero adds on import. The tax rate names must match the tax rates in your Xero organisation exactly, and are translated into the language of your club.

To import the file in Xero, open **Reporting** → **All reports** → **Journal Report** → **Go to manual journals** → **Import**, select the file, click **Import** and then **Complete Import**. The journals are created as drafts. The **Amounts are** setting is not part of the CSV file, so check on a draft that it reads **Tax Exclusive**; if it shows **No Tax** or **Tax Inclusive**, change it to **Tax Exclusive** before posting, otherwise Xero cannot apply the tax rates and the amounts will not balance. Review the drafts and post them.

#### Period closure

After the import into Xero, close the exported period in GlidingApp. Go to **Financial** → **Settings** → **Close transactions** and set **Closed until** to the day after the last exported day. Transactions dated before this date can then no longer be added or edited, and flights before this date are no longer recalculated. You can also use **Automatically close after** to move the closing date forward every night. Closing the period matters because the exported figures are now also in your accounting system. If a transaction was edited after the export, GlidingApp and your accounting software (like Xero) would no longer agree, and the next export would not reproduce what you already booked. If a transaction in a closed period must be corrected, do not edit it. Credit the amount directly instead, which creates a new transaction in the current period, and this correction flows into the next export.

### 5. Balance Reminders

**Balance reminders** notify members when their account requires payment attention. The system evaluates all member balances during nightly task execution (Monday mornings) and sends emails to members whose balance falls below the configured threshold (send_balance_min_amount). Reminder emails include current balance formatted in club currency, direct link to account page for payment, and customizable header message (send_balance_email_header) explaining payment instructions and club contact information. The reminder system respects test mode—emails do not send while payment_testmode = true, preventing confusion during financial system setup. Administrators enable automatic reminders in **Financial** → **Settings** → toggle **Send Balance Reminders** → set **Minimum Balance for Reminder** (e.g., -50 for members owing more than 50). Members receive at most one reminder per week regardless of balance changes.

### 6. SEPA/BACS Direct Debit

**SEPA/BACS direct debit** automates payment collection by charging member bank accounts directly when balances reach configured thresholds. The system processes direct debits through Mollie's SEPA/BACS mandate infrastructure, which handles European banking regulations (including United Kingdom), authorization workflows, and failure recovery. Direct debit integrates with the existing balance-based accounting system—successful charges create positive ledger entries identical to manual payments, maintaining unified financial records.

The implementation requires Mollie profile configuration (SEPA/BACS direct debit must be enabled on your Mollie profile), member mandate activation (each member authorizes direct debit via one-time bank authentication), and settlement scheduling based on club preferences. The two-phase processing workflow creates pending charges with advance notification before submitting to Mollie, allowing administrative review and member preparation. Failed payments trigger automatic retry logic with status tracking and administrative alerts.

#### Direct Debit Modes

Configure settlement timing via **Financial** → **Settings** → **Direct Debit Mode**:

| Mode | Description | Use Case |
|------|-------------|----------|
| **Monthly** | Processes all negative balances on the 1st of each month | Predictable monthly billing cycle, suitable for clubs with regular membership fees and consistent flying activity |
| **Balance** | Processes daily when member balance drops below threshold (e.g., -100) | Cash flow optimization for clubs requiring frequent settlement, prevents large outstanding balances |
| **Manual** | Administrator triggers direct debit per member via account page | Full control over timing, useful during initial implementation or for members with irregular activity |

#### Processing Workflow

Direct debit processing follows a four-step workflow with built-in safety delays:

**Step 1: Mandate Activation**  
Member navigates to **My Account** → **Activate Direct Debit** → completes one-time payment (minimum payment amount) via Mollie using iDeal, Bancontact, PayByBank (United Kingdom). This first payment establishes the SEPA mandate with their bank. The payment amount is credited to their account immediately. Bank details (last 3 digits of IBAN) are stored for member reference. Mandate status updates to "valid" upon successful authorization.

**Step 2: Scheduled Creation**  
Nightly task evaluates eligible members (valid mandate + balance meets threshold for configured mode). System creates scheduled ledger entries, that will be processed 1 day later. Members receive an advance notification email, with invoice PDF attachment detailing, that a direct debit is pending. Email uses future tense ("will be charged tomorrow") to set expectations. This delay allows administrative intervention—administrators to cancel scheduled direct debits if that is required.

**Step 3: Mollie Processing**  
Following night, system processes pending ledger entries created. Payment enters Mollie's SEPA processing queue, typically requiring 1-2 business days for bank settlement (SEPA standard allows up to 48 hours). During this period, payment status is "pending" in Mollie.

#### Chargeback Monitoring

Members retain right to dispute SEPA direct debits for 8 weeks after settlement (European banking regulation). The system monitors processed transactions via weekly chargeback detection task, which queries Mollie API for status changes on all direct debit payments from the previous 8 weeks. Detected chargebacks reverse the ledger entry, add descriptive remark, and immediately notify administrators via email. Administrators should contact affected members to resolve payment disputes and may suspend direct debit access for members with repeated chargebacks.

### 7. Declarations

**Declarations** let members claim expenses they made for the club, such as materials or travel costs. Administrators first create one or more products of type **Declaration** in **Financial** → **Products**, which serve as categories. Optionally they enable **Attachment mandatory** per product, so members must add a receipt. Members submit a declaration via **My Account** → **Declaratie indienen** by choosing a category and entering amount, remark and an optional evidence file. While the declaration is pending, the member can still change or delete it. The submitted amount shows in the ledger as pending with a balance impact of zero. Once an administrator approves it, the amount is added as a positive ledger entry and the balance increases. 

## Admin Actions

### Verify Mollie Direct Debit Configuration

**What:** Confirm SEPA direct debit is enabled on Mollie profile  
**When:** Required before enabling direct debit for members. Rerun if changing Mollie profiles or after initial setup.  
**Permission:** "Edit financial"

**How:**
1. **Financial** → **Settings** → **Direct Debit** section
2. Click **Verify Mollie Profile**
3. Observe status tag: "Activated" (green) confirms ready, "Not Enabled" (orange) requires Mollie dashboard configuration, "Not Available" (red) indicates Mollie account limitations
4. If not activated, visit Mollie dashboard → **Profiles** → **Payment Methods** → enable "SEPA Direct Debit"

### Manually Trigger Direct Debit

**What:** Create direct debit for specific member outside automatic schedule  
**When:** Manual mode enabled, or to collect payment immediately for member with valid mandate regardless of mode  
**Permission:** "Edit financial"

**How:**
1. **Financial** → **Account Holders** → search for member
2. Click member name → view account detail page
3. Verify mandate status shows "valid" and balance is negative
4. Click **Create Direct Debit** button
5. System creates pending ledger following standard two-phase workflow
6. Direct debit processes following night's task execution

**Bulk trigger:**
1. **Financial** → **Account Holders** → use checkboxes to select multiple members
2. Click **Trigger Direct Debit** for selected (button shows eligibility count)
3. System creates pending ledgers for all eligible members (valid mandate + negative balance)

### Handle Failed Payments

**What:** Investigate and resolve direct debit failures  
**When:** After receiving administrative alert email for failed payment  
**Permission:** "Edit financial"

**How:**
1. Review failure notification email for member name, amount, and Mollie error message
2. Navigate to member account page via email link
3. Common resolutions:
   - **Insufficient funds**: Contact member about balance and manually retry
   - **Closed/invalid account**: Member must reactivate mandate with updated bank details
   - **Revoked mandate**: Investigate member intent, may require reactivation or alternative payment arrangement

### Respond to Chargebacks

**What:** Process disputed direct debit transactions  
**When:** After receiving chargeback detection notification (automatic weekly monitoring)  
**Permission:** "Edit financial"

**How:**
1. Review chargeback notification email showing member name, amount, and dispute date
2. Ledger entry automatically reversed (amount set to 0), member balance reflects chargeback
3. Contact member to understand dispute reason and resolve underlying issue
4. Options: request voluntary payment via alternative method, provide documentation to support original charge, or write off amount if dispute is justified
5. Monitor member for repeated chargeback patterns (may indicate systemic billing disagreement requiring mandate suspension)

### Review Declarations

**What:** Approve or reject expense declarations submitted by members  
**When:** After receiving the nightly email about declarations waiting for review  
**Permission:** "Edit financial"

**How:**
1. **Financial** → **Declarations**. If no declaration product exists yet, use the information box to add one first
2. Click a pending declaration to view the member's remark and evidence file
3. Adjust the amount if required, then click **Approve** to book the amount on the member's account, or **Reject** to set the amount to 0

### Export a Journal to Xero

**What:** Download the journal summary of a period and import it into Xero  
**When:** After each period (for example weekly or monthly) that you want to book in Xero  
**Permission:** "Edit financial"

**How:**
1. **Financial** → **Settings** → **Default ledger accounts**: fill in the ledger code for each default account, and make sure every product has a **Ledger account**
2. **Financial** → **Settings** → **Reports**: select year, month, week or a date range
3. Click **Xero Journal summary** to download the CSV file. If a default ledger code is missing, an error lists the accounts to complete
4. In Xero, import the file via **Journal Report** → **Go to manual journals** → **Import**, check that **Amounts are** is **Tax Exclusive**, and post the drafts
5. In GlidingApp, set **Closed until** under **Close transactions** to the day after the exported period
