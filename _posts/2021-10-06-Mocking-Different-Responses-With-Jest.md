---
layout: post
title: "Mocking Different Responses with Jest"
date: 2021-10-06
categories: blog
---

## Steps

1. Declare a function variable and assign an arrow function to it.
2. Setup your mocks using jest.mock and reference the function variable above.
3. In your beforeAll hook, assign a new arrow function that returns desired mockResponse.

## Example

```javascript
// Imports

// Return empty object.
let mockResponse = () => ({});

jest.mock("local/auth", () => {
    return {
        auth: jest.fn(),
        signInWithEmailAndPassword: jest.fn(() => mockResponse())
    };
});

afterAll(() => {
  jest.resetModules();
});

describe("POST /users/login", () => {
  beforeAll(async () => {
    // Set mocked response
    mockResponse = () => {
      throw new FirebaseError("auth/wrong-password", "mocked firebase error");
    };

    // Call function under test
    await handler(req, res);
  });

  // Tests Below
});
```