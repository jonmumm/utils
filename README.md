# @jonmumm/utils

A collection of TypeScript utilities.

## Installation

```bash
npm install @jonmumm/utils
```

## API Documentation

### `catchErrorTyped`

A typed error handling utility that returns a tuple of `[error, data]`. This pattern helps avoid try/catch blocks and provides better type safety.

#### Signature

```typescript
function catchErrorTyped<T, E extends Error = Error>(
  promise: Promise<T>,
  errorsToCatch?: Array<new (...args: unknown[]) => E>
): Promise<[E | undefined, T | undefined]>;
```

#### Parameters

- `promise`: The promise to handle
- `errorsToCatch` (optional): Array of error classes to catch. If not provided, catches all errors.

#### Returns

Promise resolving to a tuple:

- First element: The caught error (if any) or undefined
- Second element: The successful data (if any) or undefined

## Usage Examples

### Basic Usage

```typescript
import { catchErrorTyped } from "@jonmumm/utils/catchErrorTyped";

async function example() {
  const [error, data] = await catchErrorTyped(fetchUserData(123));

  if (error) {
    console.error("Failed to fetch user:", error.message);
    return;
  }

  // TypeScript knows data is defined here
  console.log("User data:", data);
}
```

### Catching Specific Errors

```typescript
import { catchErrorTyped } from "@jonmumm/utils/catchErrorTyped";

class NotFoundError extends Error {}
class ValidationError extends Error {}

async function example() {
  const [error, data] = await catchErrorTyped(fetchUserData(123), [
    NotFoundError,
    ValidationError,
  ]);

  if (error) {
    if (error instanceof NotFoundError) {
      console.error("User not found");
    } else if (error instanceof ValidationError) {
      console.error("Invalid user ID");
    }
    return;
  }

  console.log("User data:", data);
}
```

Note: Other errors not specified in the `errorsToCatch` array will be thrown normally.
