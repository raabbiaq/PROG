# Frontend Spec: Invoice List

## Goal
The invoice list shows all known invoices, with the newest imported invoices displayed at the top.

## Context
This screen confirms that uploaded invoice files were successfully processed into visible business records. In milestone 1, manual upload is the primary intake mechanism, so the list must make newly imported invoices easy to notice immediately after processing.

## Data Sources
- Backend REST endpoint for invoice list
- Optional refresh after successful upload and processing

## Displayed Information
Each invoice row must display at minimum:
- invoice identifier or invoice number if available
- source filename
- invoice date if available
- total amount if available
- currency if available
- current status if available within milestone scope
- import timestamp if available for display or internal ordering

## User Actions
The user can:
- view all invoices known to the system
- inspect newly imported invoices
- refresh the list if a refresh action exists

## UI States

### Loading
The invoice list is being fetched.

### Success
The invoice list is displayed.

### Empty
No invoices are available.

### Error
The invoice list could not be loaded.

## Rules
- The invoice list must be ordered by import timestamp in descending order.
- The newest imported invoices must appear at the top of the list.
- "Newest" refers strictly to system import time.
- The list must not be ordered by invoice date, file creation time, or filename except as the explicit tie-breaker defined below.
- If multiple invoices share the same import timestamp, they must be ordered alphabetically by source filename.
- The ordering must be deterministic and stable across repeated renders of the same data.
- Empty state must be visually distinct from error state.
- The list must reflect backend truth and must not fabricate missing invoice fields.

## Edge Cases
- multiple invoices imported in the same batch
- multiple invoices share the same import timestamp
- optional invoice fields are missing
- duplicate upload is rejected and does not create a new invoice entry
- refresh occurs before backend persistence is fully visible

## Acceptance Criteria
- After a successful upload, newly imported invoices appear above older imported invoices.
- Invoice ordering follows import timestamp descending.
- If two or more invoices share the same import timestamp, they are shown in alphabetical order by source filename.
- The UI does not reorder invoices by invoice date or by file metadata other than the defined filename tie-breaker.
- Loading, empty, success, and error states are visibly distinct.
- The list remains usable even when some optional invoice fields are missing.

## Derived Test Obligations

### Component Tests
- renders invoice rows
- renders loading state
- renders empty state
- renders error state
- renders invoices ordered by descending import timestamp
- renders equal-import-timestamp invoices ordered alphabetically by source filename

### Interaction Tests
- refresh triggers a reload if implemented

### Integration-Like Frontend Tests
- mocked post-upload fetch shows newly imported invoices at the top
- invoices with identical import timestamps are shown in alphabetical filename order
- invoice date does not affect ordering