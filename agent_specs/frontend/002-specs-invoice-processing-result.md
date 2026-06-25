# Frontend Spec: Invoice Processing Result

## Goal
After upload and processing, the user can see what happened to the submitted invoice files.

## Context
Accounting-related import flows require clear traceability. The user must be able to understand whether files were successfully processed, rejected, skipped, or failed.

## Data Sources
- Processing response from backend upload endpoint
- Optional follow-up data fetch for newly created invoice records

## Displayed Information
The result view must display at minimum:
- total number of submitted files
- number of successfully processed files
- number of failed files
- number of skipped or unsupported files if available
- per-file result information if available
- readable error information where available

## User Actions
The user can:
- inspect the processing summary
- inspect file-level results if present
- continue to the invoice list
- retry after a failure if retry is supported from this view

## UI States

### Loading
The final result is not yet available.

### Full Success
All submitted files were processed successfully.

### Partial Success
Some submitted files were processed successfully and some were not.

### Error
The UI could not obtain a reliable processing result.

## Rules
- The result summary must reflect backend truth exactly.
- Partial success must never be shown as full success.
- File-level outcomes must be distinguishable when provided by backend.
- The result view must help the user understand what requires attention next.

## Edge Cases
- backend returns summary only, without per-file details
- backend returns unknown failure reason
- processing result is delayed
- duplicate file detection occurs
- unsupported file type occurs

## Acceptance Criteria
- The user sees a summary after processing completes.
- Full success is visually distinct from partial success.
- Failures are visible when the backend reports them.
- The user can proceed from the result view to the invoice list.
- In the invoice list, imported invoices are shown in import-time order, newest first.

## Derived Test Obligations

### Component Tests
- renders full success summary
- renders partial success summary
- renders error state
- renders per-file results if provided

### Interaction Tests
- continue action opens or refreshes invoice list if implemented

### Integration-Like Frontend Tests
- mocked backend result is rendered faithfully