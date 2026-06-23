# Frontend Spec: Upload Error Handling

## Goal
The upload flow must communicate failures clearly so that users understand whether the problem occurred during file transfer, validation, or invoice processing.

## Context
Importing invoices is a trust-sensitive workflow. Users must be able to distinguish total failure from partial failure and must not lose visibility into successful imports when some files fail.

## Data Sources
- Backend upload error responses
- Backend processing result with file-level errors when available
- Local UI error state

## Displayed Information
The UI must display at minimum:
- transport or network failure message when upload cannot complete
- processing or validation error message when backend rejects one or more files
- per-file failure information when available
- distinction between complete failure and partial failure

## User Actions
The user can:
- inspect what failed
- retry the upload
- change the selected files and try again

## UI States

### Transport Error
The upload did not successfully reach or complete against the backend.

### Processing Error
The backend received files but one or more files failed validation or processing.

### Partial Failure
Some files were processed successfully while others were rejected, skipped, or failed.

## Rules
- Transport-level problems must be visually distinguishable from processing-level problems.
- Partial failure must preserve visibility into successful files.
- Error messages should be understandable to non-technical users whenever possible.
- Retry must not require a full page reload.
- The frontend must not collapse all failures into one vague generic message if more specific information exists.

## Edge Cases
- timeout during upload
- backend internal error without detailed explanation
- unsupported file type
- file can be uploaded but not parsed as an invoice
- duplicate file is rejected by backend
- one failed retry follows an earlier partial success

## Acceptance Criteria
- The user can distinguish transport failure from processing failure.
- Partial failure is distinct from total failure.
- Retry remains available after failure.
- Successful files remain visible in the outcome even if some files fail.

## Derived Test Obligations

### Component Tests
- renders transport error
- renders processing error
- renders partial failure

### Interaction Tests
- retry action starts a new upload attempt

### Integration-Like Frontend Tests
- mocked network failure shows transport error
- mocked processing failure shows processing error
- mocked mixed result shows partial-failure UI