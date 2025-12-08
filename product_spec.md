<frontmatter>
  layout: default.md
  title: Product Specification (MVP)
</frontmatter>

# Product Specification (MVP)

## Feature List (High Level)

1.  **Authentication**: Secure login via Email and Phone OTP.
2.  **Group Management**: Create and manage static groups of friends (e.g., "Lunch Buddies").
3.  **OCR Receipt Parsing**: Auto-extract line items, prices, and tax info from receipt photos.
4.  **Interactive Expense Assignment**: Drag-and-drop or tap interface to assign items to group members.
5.  **Smart Splitting**: Support for equal splits, quantity-based splits, and percentage splits.
6.  **Automatic Tax Calculation**: Distribute GST and Service Charge proportionally across items.
7.  **Settlement Calculation**: Generate a clear "Who owes Who" summary.
8.  **Sharing**: Share settlement details or payment links via text/external apps.

## Functional Requirements

### 1. Authentication
*   **FR-01**: System MUST allow users to input Email or Phone Number.
*   **FR-02**: System MUST generate and send a One-Time Password (OTP) to the provided contact.
*   **FR-03**: System MUST validate the OTP to authenticate the user.

### 2. Group Management
*   **FR-04**: Users MUST be able to create a named group.
*   **FR-05**: Users MUST be able to manually add member names to a group (MVP: no need for real user linking, just names).
*   **FR-06**: Group membership IS static for a specific bill but editable for future bills.

### 3. OCR Receipt Parsing
*   **FR-07**: Users MUST be able to take a photo or upload an image of a receipt.
*   **FR-08**: System MUST extract the following for each line item: `Item Name`, `Quantity`, `Unit Price`.
*   **FR-09**: System MUST detect and extract `GST` and `Service Charge` (if present) from the receipt footer.

### 4. Expense Assignment & Editing
*   **FR-10**: Users MUST be able to assign a line item to one or multiple group members.
*   **FR-11**: Users MUST be able to edit recognized text (Name, Price, Qty) if OCR fails.
*   **FR-12**: Users MUST be able to add new manual items or delete existing ones.
*   **FR-13**: Users MUST be able to split a single item (e.g., "Shares") into multiple sub-items.

### 5. Calculation & Results
*   **FR-14**: System MUST calculate total cost per person: $\sum (\text{Item Price} \times \text{Share}) + \text{Proportional Tax}$.
*   **FR-15**: System MUST generate a settlement path (e.g., "Alice pays Bob $15").
*   **FR-16**: System MUST allow users to export the summary as text to clipboard.

## Non-Functional Requirements

*   **NFR-01 (Platform)**: App MUST run on both iOS and Android (React Native).
*   **NFR-02 (Performance)**: OCR processing SHOULD take less than 5 seconds on average.
*   **NFR-03 (Currency)**: System MUST support Singapore Dollars (SGD) for MVP.
*   **NFR-04 (Usability)**: UI MUST be responsive and handle various screen sizes.

## Use Cases

### UC-1: Use Receipt to Split Bill
**Actor**: User (Payer)
1.  User creates a group "Friday Dinner".
2.  User taps "Scan Receipt" and takes a photo.
3.  App displays list of extracted items.
4.  User assigns "Steak" to Bob, "Salad" to Alice and Self.
5.  User reviews "Who owes Who" summary.
6.  User copies summary and pastes into group chat.

## User Stories

### US-1: Quick Authentication
> **As a** new user,
> **I want** to log in using just my phone number and an OTP,
> **So that** I don't have to remember another password.

*   **AC-1**: Entering phone number sends SMS.
*   **AC-2**: Entering correct code logs user in.
*   **AC-3**: Invalid code shows error message.

### US-2: Digitize Receipt
> **As a** payer,
> **I want** to photograph the receipt and have the app type out the items,
> **So that** I don't have to manually enter data.

*   **AC-1**: Camera interface opens successfully.
*   **AC-2**: Image is sent to OCR engine.
*   **AC-3**: Resulting list contains at least 90% accuracy on standard receipts.
*   **AC-4**: GST and Service Charge are identified separately from line items.

### US-3: Split Shared Items
> **As a** user,
> **I want** to split a shared appetizer among 3 people,
> **So that** we all pay our fair share.

*   **AC-1**: Selecting an item allows selecting multiple assignees.
*   **AC-2**: Cost is divided equally among selected assignees.
*   **AC-3**: Tax/Service charge for that item is also divided equally.

### US-4: Settlement Summary
> **As a** group member,
> **I want** to see exactly how much I owe and for what,
> **So that** I can verify the calculation.

*   **AC-1**: Summary screen shows total amount owed per person.
*   **AC-2**: Tapping a person shows their specific list of items.
*   **AC-3**: A "Settlement" view shows the simplest set of comparisons (Netting).
