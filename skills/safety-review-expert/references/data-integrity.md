# Data Integrity & Transactional Safety

Data integrity ensures that your database remains a "Source of Truth" even when errors, crashes, or concurrent requests occur. Vibe-coded apps often fail here by assuming every database write will succeed.

## [1] Atomic Transactions
**The Problem**: Performing multiple related database writes as separate commands. If the second one fails, the first one remains, creating "orphaned" or inconsistent data.

- **Check**: Sequential `await db.save()` calls that belong to a single logical action (e.g., creating a User AND an Initial Balance).
- **The Risk**: A user exists but has no account record because the second write timed out.
- **Fix**: Wrap multi-step writes in a single Database Transaction (`BEGIN...COMMIT`).



## [2] Idempotency
**The Problem**: Retrying a failed request results in the action being performed twice.

- **Check**: POST endpoints that create resources or process payments without an `idempotency-key`.
- **The Vibe**: "If the UI shows an error, the user will just click 'Submit' again."
- **The Risk**: Double charges, duplicate orders, or multiple account creations.
- **Fix**: Use an `Idempotency-Key` header. Check if the key exists in cache/DB before processing; if it does, return the previous result.

## [3] Partial Writes & Error Recovery
**The Problem**: Updating an object in memory but failing to persist it, or vice versa.

- **Check**: Logic where an external API is called (like Stripe or Gemini) *after* a database state has been changed, but without a rollback plan if the API fails.
- **Fix**: **DB-First, then Side-Effect.** Or use a "Pending" state in the DB that only flips to "Completed" after the external side-effect succeeds.

## [4] Weak Validation before Persistence
**The Problem**: Relying on "Type Coercion" or database defaults instead of strict schemas.

- **Check**: Saving raw JSON objects directly into a `JSONB` column without structure validation.
- **The Risk**: "Silent" corruption where a field expected to be a `number` is saved as a `string`, causing the app to crash during a later calculation.
- **Fix**: Use Zod or JSON Schema to validate data *immediately* before the `INSERT/UPDATE` command.

## [5] Lost Updates (Concurrent Editing)
**The Problem**: Two users edit the same record at the same time. The last one to click "Save" overwrites the first one's changes without knowing.

- **Check**: UI/API flows that send the entire object back for an update without a version check.
- **Fix**: Use **Optimistic Locking**. Add a `version` or `updated_at` column to the `WHERE` clause: 
  `UPDATE posts SET content = '...' WHERE id = 1 AND version = 5`.



## Checklist for the Agent
- "Is this multi-step process wrapped in a transaction?"
- "If this function crashes at line [X], what state is the database left in?"
- "Does this record have a unique constraint to prevent duplicates during a retry?"
- "Are we using 'Upserts' (Update or Insert) where appropriate to handle concurrency?"