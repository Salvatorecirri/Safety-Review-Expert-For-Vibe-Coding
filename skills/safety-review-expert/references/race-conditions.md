# Race Conditions & Concurrency Hazards

Race conditions occur when the outcome of a process is unexpectedly dependent on the timing or sequence of other events. In "vibe-coded" apps, these are often missed because the app works perfectly for a single user.

## [1] Check-Then-Act (TOCTOU)
**The Problem**: A value is checked, and then an action is performed based on that check. However, the state changes between the "Check" and the "Act."

- **Check**: Look for `if (exists) { doAction() }` or `if (authorized) { proceed() }` patterns.
- **The Vibe**: "I checked if the user had money before I let them buy it."
- **The Risk**: Two requests hitting at the same time can both pass the check before either has finished the act (e.g., double-spending).
- **Fix**: Use atomic operations or database-level constraints.



## [2] Shared State Access
**The Problem**: Multiple async tasks or threads accessing/modifying the same variable or object simultaneously.

- **Check**: Global variables, singletons, or shared caches that are modified in `async` functions without locking.
- **The Vibe**: "I'll just keep a counter in this global variable for now."
- **The Risk**: Data corruption where increments are lost or state becomes inconsistent.
- **Fix**: Use thread-safe collections, Mutexes, or move state to a dedicated store like Redis.

## [3] Database Concurrency
**The Problem**: Read-Modify-Write cycles without proper isolation.

- **Check**: Code that fetches a row, modifies it in JS/TS, and saves it back.
- **Flag these patterns**:
    ```javascript
    // DANGEROUS: Non-atomic increment
    const user = await db.users.find(id);
    user.balance += 10;
    await db.users.save(user);
    ```
- **Fix**: 
    - **Pessimistic**: `SELECT FOR UPDATE`.
    - **Optimistic**: Version columns (`WHERE version = 5`).
    - **Atomic**: `UPDATE users SET balance = balance + 10 WHERE id = 1`.

## [4] Distributed System Races
**The Problem**: Logic that assumes only one instance of the server is running.

- **Check**: Local file system locks or memory-based locks in a containerized (Docker/K8s) environment.
- **The Risk**: A "lock" on Server A won't stop Server B from performing the same sensitive operation.
- **Fix**: Use distributed locks (e.g., Redlock via Redis).

## [5] Common Patterns to Flag

| Pattern | Vulnerability | Recommended Fix |
| :--- | :--- | :--- |
| `if (!exists(file)) write(file)` | TOCTOU / File overwrite | Use atomic write flags (e.g., `wx`) |
| `if (user.credits > 0) decrement()` | Double Spend | Atomic DB decrement with `CHECK` constraint |
| `Object.assign(sharedObj, update)` | State corruption | Use immutable patterns or Mutexes |

## Investigative Questions for the Agent
- "What happens if two requests hit this code at the exact same microsecond?"
- "Is this operation atomic (all-or-nothing) or can it be interrupted?"
- "Does this logic rely on an 'in-memory' check for a 'persistent' resource?"