---
name: yup-1
description: >
  Yup schema validation patterns.
  Trigger: When creating or updating Yup schemas for form validation, API request validation, or data transformation.
license: Apache-2.0
metadata:
  author: ozzy-team
  version: "1.0"
  scope: [root, ui]
  auto_invoke: "Creating Yup schemas"
---

## Basic Schemas

```typescript
import * as yup from 'yup';

// Primitives
const stringSchema = yup.string();
const numberSchema = yup.number();
const booleanSchema = yup.boolean();
const dateSchema = yup.date();

// Common validators
const emailSchema = yup.string().email();
const urlSchema = yup.string().url();
const uuidSchema = yup.string().uuid();

// With constraints
const nameSchema = yup.string().min(1).max(100);
const ageSchema = yup.number().integer().positive().max(150);
const priceSchema = yup.number().min(0);
```

## Object Schemas

```typescript
const userSchema = yup.object({
  id: yup.string().uuid().required(),
  email: yup.string().email('Invalid email address').required('Email is required'),
  name: yup.string().min(1, 'Name is required'),
  age: yup.number().integer().positive().notRequired(),
  role: yup.string().oneOf(['admin', 'user', 'guest']),
  metadata: yup.object().optional(),
});

type User = yup.InferType<typeof userSchema>;

// Parsing
const user = await userSchema.validate(data);  // Throws on error
const result = await userSchema.validate(data, { abortEarly: false })
  .then(value => ({ success: true, data: value }))
  .catch(err => ({
    success: false,
    errors: err.inner.reduce((acc: Record<string, string>, curr) => {
      if (curr.path) acc[curr.path] = curr.message;
      return acc;
    }, {})
  }));
```

## Arrays and Objects

```typescript
// Arrays
const tagsSchema = yup.array(yup.string()).min(1).max(10);
const numbersSchema = yup.array(yup.number()).min(1);

// Objects with dynamic keys
const scoresSchema = yup.object().shape({}, [
  // All keys must be numbers
  ['*', yup.number()]
]);

// Nested objects
const addressSchema = yup.object({
  street: yup.string().required(),
  city: yup.string().required(),
  zipCode: yup.string().matches(/^\d{5}(-\d{4})?$/, 'Invalid ZIP code')
});
```

## Conditional Validation

```typescript
const orderSchema = yup.object({
  isPaid: yup.boolean(),
  paymentMethod: yup.string().when('isPaid', {
    is: true,
    then: schema => schema.required('Payment method is required when order is paid'),
    otherwise: schema => schema.notRequired()
  }),
  creditCardNumber: yup.string().when('paymentMethod', {
    is: 'credit_card',
    then: schema => schema.required('Credit card number is required')
  })
});
```

## Transformations

```typescript
// Trim strings automatically
const trimmedString = yup.string().transform(value => 
  value ? value.trim() : value
);

// Parse numbers from strings
const numberFromString = yup.string().transform(value => 
  value === '' ? undefined : Number(value)
).typeError('Must be a number');

// Format dates
const formattedDate = yup.date().transform(value => {
  if (typeof value === 'string') {
    return new Date(value);
  }
  return value;
});
```

## Custom Validations

```typescript
const passwordSchema = yup.string()
  .min(8, 'Password must be at least 8 characters')
  .matches(/[A-Z]/, 'Must contain at least one uppercase letter')
  .matches(/[0-9]/, 'Must contain at least one number');

const confirmPasswordSchema = yup.string()
  .oneOf([yup.ref('password')], 'Passwords must match');

// Async validation
const uniqueEmailSchema = yup.string()
  .email()
  .test('unique-email', 'Email already exists', async (value) => {
    if (!value) return true;
    const exists = await checkEmailExists(value);
    return !exists;
  });
```

## Formik Integration

```typescript
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as yup from 'yup';

const validationSchema = yup.object({
  email: yup.string().email('Invalid email').required('Required'),
  password: yup.string().min(8, 'Too short!').required('Required'),
});

function LoginForm() {
  return (
    <Formik
      initialValues={{ email: '', password: '' }}
      validationSchema={validationSchema}
      onSubmit={(values) => {
        // Handle form submission
      }}
    >
      {({ errors, touched }) => (
        <Form>
          <Field name="email" type="email" />
          {errors.email && touched.email && <div>{errors.email}</div>}
          
          <Field name="password" type="password" />
          {errors.password && touched.password && <div>{errors.password}</div>}
          
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  );
}
```

## Error Messages

```typescript
// Custom error messages
const schema = yup.object({
  username: yup
    .string()
    .min(3, 'Username must be at least 3 characters')
    .max(30, 'Username must be at most 30 characters')
    .required('Username is required'),
  
  age: yup
    .number()
    .typeError('Age must be a number')
    .positive('Age must be positive')
    .integer('Age must be an integer')
    .required('Age is required'),
});

// Custom error messages for specific validation types
const customMessages = {
  mixed: {
    default: '${path} is invalid',
    required: '${path} is a required field',
  },
  string: {
    email: 'Please enter a valid email',
    min: '${path} must be at least ${min} characters',
    max: '${path} must be at most ${max} characters',
  },
};

yup.setLocale(customMessages);
```

## Testing Schemas

```typescript
import * as yup from 'yup';

describe('User Schema', () => {
  const userSchema = yup.object({
    name: yup.string().required(),
    email: yup.string().email().required(),
  });

  it('should validate a valid user', async () => {
    const user = { name: 'John', email: 'john@example.com' };
    await expect(userSchema.isValid(user)).resolves.toBe(true);
  });

  it('should reject invalid email', async () => {
    const user = { name: 'John', email: 'invalid-email' };
    await expect(userSchema.validate(user)).rejects.toThrow('email must be a valid email');
  });
});
```

## Common Patterns

### Partial Validation
```typescript
// For PATCH requests or partial updates
const partialUserSchema = userSchema.partial();
```

### Lazy Validation
```typescript
const conditionalSchema = yup.lazy(value => {
  switch (typeof value) {
    case 'number':
      return yup.number().min(0);
    case 'string':
      return yup.string().min(1);
    default:
      return yup.mixed();
  }
});
```

### Transform and Validate
```typescript
const trimmedSchema = yup.string()
  .transform(value => value?.trim())
  .min(1, 'Cannot be empty')
  .max(100, 'Too long');
```
