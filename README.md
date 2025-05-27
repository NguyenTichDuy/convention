# Naming Conventions Guide

## Overview

This document establishes comprehensive naming conventions for our React TypeScript codebase to ensure consistency, readability, and maintainability across the entire development team. 

### Purpose
- **Standardize** naming patterns across components, functions, variables, and files
- **Improve** code readability and developer experience
- **Reduce** cognitive load when reading and maintaining code
- **Enable** faster onboarding for new team members
- **Facilitate** easier code reviews and collaboration
- **Ensure** consistent patterns that scale with project growth

### Why Naming Matters
Good naming conventions are crucial for:
- **Code Maintainability** - Clear names make future modifications easier
- **Team Collaboration** - Consistent patterns help team members understand each other's code
- **Debugging Efficiency** - Descriptive names make error tracking faster
- **Knowledge Transfer** - Self-documenting code reduces dependency on original authors
- **Scalability** - Consistent patterns prevent technical debt as the codebase grows

### Tech Stack Context
*React 17 + TypeScript 5 + Ant Design 4 + Nx Monorepo*

## Table of Contents
1. [Core Principles](#core-principles)
2. [Variables](#variables)
3. [Functions & Event Handlers](#functions--event-handlers)
4. [React Components](#react-components)
5. [Custom Hooks](#custom-hooks)
6. [Types & Interfaces](#types--interfaces)
7. [Constants & Enums](#constants--enums)
8. [Testing](#testing)
9. [Quick Reference](#quick-reference)

---

## Core Principles

### S-I-D-C-T Pattern
Every name must be:
- **Short** - Quick to type and remember
- **Intuitive** - Reads naturally, close to common speech  
- **Descriptive** - Reflects what it does/possesses efficiently
- **Consistent** - Follows established patterns across the codebase
- **Testable** - Clear inputs/outputs that make testing straightforward

#### Examples

```typescript
// ✅ S-I-D-C-T compliant
const calculateUserSubscriptionRenewalDate = (
  subscription: Subscription, 
  renewalPeriod: RenewalPeriod
): Date => {
  // Short: Concise but complete
  // Intuitive: Reads like natural language
  // Descriptive: Clear what it calculates
  // Consistent: Follows A/HC/LC pattern (A=calculate, HC=User, LC=SubscriptionRenewalDate)
  // Testable: Clear inputs and expected output
};

const validateUserEmailFormat = (email: string): boolean => {
  // Short: Not verbose
  // Intuitive: Clear validation purpose
  // Descriptive: Specifies email format validation
  // Consistent: Follows A/HC/LC pattern (A=validate, HC=User, LC=EmailFormat)
  // Testable: Boolean return, string input
};

const handleUserProfileSubmission = (profileData: UserProfileForm): void => {
  // Short: Appropriately sized
  // Intuitive: Clear event handling
  // Descriptive: Specific to profile submission
  // Consistent: Follows A/HC/LC pattern (A=handle, HC=User, LC=ProfileSubmission)
  // Testable: Clear input type, void return
};

// ❌ Violates S-I-D-C-T principles
const calc = (s: any, r: any): any => { }; // Not intuitive, descriptive, or testable
const processUserStuff = (data: any): any => { }; // Not descriptive or testable
const doThing = (): void => { }; // Violates all principles
const getUserInfo = (id: string): any => { }; // Inconsistent return type, not testable
```

#### S-I-D-C-T in Testing Context

```typescript
// ✅ Function name makes test self-documenting
describe('calculateUserSubscriptionRenewalDate', () => {
  it('should return correct renewal date for monthly subscription', () => {
    const subscription = createMockSubscription({ type: 'monthly' });
    const renewalPeriod = RenewalPeriod.MONTHLY;
    
    const result = calculateUserSubscriptionRenewalDate(subscription, renewalPeriod);
    
    expect(result).toBeInstanceOf(Date);
    expect(result.getMonth()).toBe(nextMonthExpected);
  });
});

// ✅ Boolean validation function - easy to test
describe('validateUserEmailFormat', () => {
  it('should return true for valid email format', () => {
    const validEmail = 'user@example.com';
    
    const result = validateUserEmailFormat(validEmail);
    
    expect(result).toBe(true);
  });
});
```

### Language & Convention
- **English only** - All names in English
- **camelCase** - For variables, functions, methods, hooks, and GraphQL files
- **PascalCase** - For components, types, interfaces, classes, and context files
- **SCREAMING_SNAKE_CASE** - For constants
- **kebab-case** - For utility files, services, and helpers
- **kebab-case.types.ts** - For type definition files
- **kebab-case.constants.ts** - For constant definition files

#### File Naming Breakdown
| File Type | Convention | Example |
|-----------|------------|---------|
| **React Components** | PascalCase.tsx | `UserProfileCard.tsx` |
| **React Hooks** | camelCase.ts | `useUserData.ts` |
| **React Context** | PascalCase.context.tsx | `UserAuth.context.tsx` |
| **GraphQL Files** | camelCase.gql | `userProfileQueries.gql` |
| **Type Definitions** | kebab-case.types.ts | `user.types.ts` |
| **Constants** | kebab-case.constants.ts | `api-endpoints.constants.ts` |
| **Services** | kebab-case-service.ts | `user-service.ts` |
| **Test Files** | SameName.test.tsx/ts | `UserProfileCard.test.tsx` |

### Golden Rules

#### ✅ **DO's**

**Use descriptive names over short abbreviations**
```typescript
// ✅ Good
const userAuthenticationToken = 'abc123';
const calculateMonthlyRevenue = () => { };
const isUserEmailVerified = true;

// ❌ Bad
const userAuthTkn = 'abc123';
const calcRevenue = () => { };
const isVerified = true;
```

**Follow consistent patterns across the codebase**
```typescript
// ✅ Good - Consistent A/HC/LC pattern
const getUserProfile = (userId: string) => { };
const getUserPreferences = (userId: string) => { };
const getUserNotifications = (userId: string) => { };

// ❌ Bad - Inconsistent patterns
const getUserProfile = (userId: string) => { };
const fetchUserPrefs = (userId: string) => { };
const getNotificationsForUser = (userId: string) => { };
```

**Reflect the expected result in boolean names**
```typescript
// ✅ Good - Clear expected result
const isUserLoggedIn = user.token !== null;
const hasUserCompletedOnboarding = user.onboardingStep === 'complete';
const canUserEditProfile = user.permissions.includes('edit');

// ❌ Bad - Unclear expected result
const userStatus = user.token !== null;
const onboarding = user.onboardingStep === 'complete';
const editProfile = user.permissions.includes('edit');
```

**Use intention-revealing names**
```typescript
// ✅ Good - Clear intention
const elapsedTimeInDays = 86400;
const getUsersWithActiveSubscriptions = () => { };
const MAX_LOGIN_RETRY_ATTEMPTS = 3;

// ❌ Bad - Requires mental mapping
const d = 86400; // elapsed time in days
const getUserList = () => { }; // What kind of users?
const MAX = 3; // Max what?
```

**Make names searchable and greppable**
```typescript
// ✅ Good - Easy to search
const PAYMENT_STATUS_PENDING = 'pending';
const handleUserProfileSubmit = () => { };
const validateUserEmailFormat = () => { };

// ❌ Bad - Hard to search
const STATUS = 'pending';
const handleSubmit = () => { }; // Which submit?
const validate = () => { }; // Validate what?
```

**Use pronounceable names**
```typescript
// ✅ Good - Easy to pronounce and discuss
const generateTimestamp = () => new Date().toISOString();
const userModificationDate = user.updatedAt;
const customerRecord = { id: '123', name: 'John' };

// ❌ Bad - Hard to pronounce
const genymdhms = () => new Date().toISOString();
const modymdhms = user.updatedAt;
const xyzrec = { id: '123', name: 'John' };
```

**Use solution domain names when appropriate**
```typescript
// ✅ Good - Uses established programming terms
const userAccountFactory = () => { };
const orderObserver = new OrderWatcher();
const productDecorator = (product: Product) => { };

// ❌ Bad - Avoid business jargon when tech terms are clearer
const userAccountMaker = () => { };
const orderWatcher = new OrderWatcher(); // This is actually fine
const productEnhancer = (product: Product) => { };
```

#### ❌ **DON'Ts**

**Don't use contractions or abbreviations**
```typescript
// ❌ Bad
const btnClk = () => { }; // → handleButtonClick
const usrPrf = {}; // → userProfile
const calcTax = () => { }; // → calculateTax
const getMsgCnt = () => { }; // → getMessageCount

// ✅ Good
const handleButtonClick = () => { };
const userProfile = {};
const calculateTax = () => { };
const getMessageCount = () => { };
```

**Don't duplicate context unnecessarily**
```typescript
// ❌ Bad - Duplicated context
class MenuItem {
  handleMenuItemClick = () => { }; // Should be handleClick
  getMenuItemLabel = () => { }; // Should be getLabel
}

const UserProfileCard = () => {
  const handleUserProfileCardSubmit = () => { }; // Should be handleSubmit
};

// ✅ Good - Context is clear from surrounding scope
class MenuItem {
  handleClick = () => { };
  getLabel = () => { };
}

const UserProfileCard = () => {
  const handleSubmit = () => { };
};
```

**Don't use misleading names**
```typescript
// ❌ Bad - Misleading names
const getUserList = () => getActiveUsers(); // Returns only active users
const saveUser = (user: User) => validateAndSave(user); // Also validates
const isReady = checkComplexConditions(); // Actually performs complex logic

// ✅ Good - Honest names
const getActiveUsers = () => { };
const validateAndSaveUser = (user: User) => { };
const hasMetAllRequirements = () => checkComplexConditions();
```

**Don't use magic numbers or strings without constants**
```typescript
// ❌ Bad - Magic numbers/strings
if (user.status === 'active' && user.loginAttempts < 5) { }
setTimeout(callback, 300);
const results = items.slice(0, 20);

// ✅ Good - Named constants
const USER_STATUS_ACTIVE = 'active';
const MAX_LOGIN_ATTEMPTS = 5;
const DEBOUNCE_DELAY_MS = 300;
const DEFAULT_PAGE_SIZE = 20;

if (user.status === USER_STATUS_ACTIVE && user.loginAttempts < MAX_LOGIN_ATTEMPTS) { }
setTimeout(callback, DEBOUNCE_DELAY_MS);
const results = items.slice(0, DEFAULT_PAGE_SIZE);
```

**Don't use mental mapping or encoded information**
```typescript
// ❌ Bad - Requires mental mapping
const d = new Date(); // d = date
const u = getCurrentUser(); // u = user
const temp = calculateTotal(items); // temp = ?

// ✅ Good - No mental mapping needed
const currentDate = new Date();
const currentUser = getCurrentUser();
const orderTotal = calculateTotal(items);
```

**Don't use noise words**
```typescript
// ❌ Bad - Noise words add no value
const userData = user; // Just use 'user'
const userInfo = user; // Just use 'user'
const theUser = user; // Just use 'user'
const userObject = user; // Just use 'user'

// ✅ Good - Specific, meaningful names
const user = getCurrentUser();
const userProfile = user.profile;
const userPreferences = user.settings;
const authenticatedUser = getAuthenticatedUser();
```

**Don't use inconsistent vocabulary**
```typescript
// ❌ Bad - Mixed vocabulary for same concept
const fetchUser = (id: string) => { };
const getProduct = (id: string) => { };
const retrieveOrder = (id: string) => { };

const deleteUser = (id: string) => { };
const removeProduct = (id: string) => { };
const destroyOrder = (id: string) => { };

// ✅ Good - Consistent vocabulary
const getUser = (id: string) => { };
const getProduct = (id: string) => { };
const getOrder = (id: string) => { };

const deleteUser = (id: string) => { };
const deleteProduct = (id: string) => { };
const deleteOrder = (id: string) => { };
```

---

## Variables

Variables should use **camelCase** and follow descriptive naming that clearly indicates their purpose and content. Every variable name should be self-documenting and eliminate the need for mental mapping.

### Basic Variable Naming

#### Primitives
```typescript
// ✅ Good - Descriptive and clear
const userEmail = 'john@example.com';
const productPrice = 29.99;
const orderItemCount = 5;
const isUserAuthenticated = true;
const maxRetryAttempts = 3;
const currentTimestamp = Date.now();

// ❌ Bad - Unclear or abbreviated
const email = 'john@example.com'; // Which email?
const price = 29.99; // Price of what?
const count = 5; // Count of what?
const isAuth = true; // Abbreviated
const max = 3; // Max what?
const ts = Date.now(); // Unclear abbreviation
```

#### Objects & Complex Data
```typescript
// ✅ Good - Clear entity and purpose
const userProfile = { name: 'John', email: 'john@example.com' };
const orderSummary = { total: 99.99, itemCount: 3 };
const productDetails = { id: '123', name: 'Laptop', price: 999 };
const shippingAddress = { street: '123 Main St', city: 'NYC' };
const paymentConfiguration = { currency: 'USD', provider: 'stripe' };
const formValidationRules = { email: 'required', age: 'number' };

// ❌ Bad - Generic or unclear
const data = { name: 'John', email: 'john@example.com' };
const info = { total: 99.99, itemCount: 3 };
const obj = { id: '123', name: 'Laptop' };
const config = { currency: 'USD' }; // Too generic
const rules = { email: 'required' }; // Missing context
```

#### Arrays & Collections
```typescript
// ✅ Good - Plural nouns indicating collection content
const userNotifications = [{ id: '1', message: 'Welcome' }];
const productCategories = ['electronics', 'clothing', 'books'];
const orderItems = [{ productId: '1', quantity: 2 }];
const activeUserSessions = [{ userId: '1', startTime: Date.now() }];
const filteredSearchResults = [{ id: '1', title: 'Product A' }];
const selectedProductIds = ['prod-1', 'prod-2', 'prod-3'];

// ❌ Bad - Singular or unclear
const notification = [{ id: '1', message: 'Welcome' }]; // Should be plural
const categories = ['electronics', 'clothing']; // Missing context
const items = [{ productId: '1', quantity: 2 }]; // Too generic
const list = [{ userId: '1' }]; // What kind of list?
const selected = ['prod-1', 'prod-2']; // Selected what?
```

### Boolean Variables

Boolean variables should clearly indicate what condition they represent using established prefixes:

#### Boolean Prefixes & Patterns
| Prefix | Purpose | Example |
|--------|---------|---------|
| **is** | Current state | `isUserActive`, `isFormValid`, `isDataLoading` |
| **has** | Possession/existence | `hasUserPermission`, `hasOrderItems`, `hasFormErrors` |
| **can** | Capability/permission | `canUserEdit`, `canOrderShip`, `canAccessFeature` |
| **should** | Conditional logic | `shouldShowModal`, `shouldValidateInput`, `shouldRefreshData` |
| **will** | Future state | `willOrderExpire`, `willUserReceiveEmail`, `willFormSubmit` |
| **was** | Past state | `wasOrderProcessed`, `wasUserNotified`, `wasPaymentSuccessful` |
| **are** | Multiple items state | `areItemsSelected`, `areFieldsValid`, `areUsersActive` |

#### Boolean Examples
```typescript
// ✅ Good - Clear true/false meaning
const isUserLoggedIn = user.token !== null;
const hasUserCompletedProfile = user.profile.isComplete;
const canUserEditOrder = user.permissions.includes('edit');
const shouldShowNotification = notification.priority === 'high';
const areOrderItemsValid = orderItems.every(item => item.quantity > 0);
const willSubscriptionExpire = subscription.expiresAt < Date.now();

// Form states
const isFormSubmitting = submissionState === 'loading';
const hasFormErrors = Object.keys(errors).length > 0;
const canFormBeSubmitted = isFormValid && !isFormSubmitting;
const shouldShowSuccessMessage = submissionState === 'success';

// ❌ Bad - Unclear boolean meaning
const user = true; // What about the user is true?
const form = false; // What about the form is false?
const data = loading; // Is data loading or is data equal to loading?
const order = status === 'complete'; // Unclear what this represents
const valid = checkInput(); // Valid what?
```

### React State Variables

React state variables should follow the same naming patterns with clear setter names:

```typescript
// ✅ Good - Clear state purpose with descriptive setters
const [isUserModalVisible, setIsUserModalVisible] = useState(false);
const [userFormData, setUserFormData] = useState<UserForm>({});
const [orderSubmissionStatus, setOrderSubmissionStatus] = useState<'idle' | 'loading' | 'success' | 'error'>('idle');
const [selectedProductIds, setSelectedProductIds] = useState<string[]>([]);
const [userSearchQuery, setUserSearchQuery] = useState('');

// Form-specific state
const [userEmailError, setUserEmailError] = useState<string | null>(null);
const [isPasswordVisible, setIsPasswordVisible] = useState(false);
const [hasUserAgreedToTerms, setHasUserAgreedToTerms] = useState(false);
const [currentFormStep, setCurrentFormStep] = useState(1);

// Data loading states
const [userProfileData, setUserProfileData] = useState<UserProfile | null>(null);
const [isUserDataLoading, setIsUserDataLoading] = useState(false);
const [userDataError, setUserDataError] = useState<string | null>(null);

// ❌ Bad - Generic or unclear state
const [visible, setVisible] = useState(false); // What is visible?
const [data, setData] = useState({}); // What kind of data?
const [status, setStatus] = useState('idle'); // Status of what?
const [selected, setSelected] = useState([]); // What is selected?
const [error, setError] = useState(null); // Error for what?
const [loading, setLoading] = useState(false); // Loading what?
```

### Temporary & Computed Variables

Even temporary variables should be descriptive and indicate their purpose:

```typescript
// ✅ Good - Clear temporary purpose
const filteredActiveUsers = users.filter(user => user.isActive);
const sortedProductsByPrice = products.sort((a, b) => a.price - b.price);
const userDisplayName = `${user.firstName} ${user.lastName}`;
const formattedOrderDate = order.createdAt.toLocaleDateString();
const calculatedOrderTotal = orderItems.reduce((sum, item) => sum + item.price, 0);
const transformedApiResponse = mapUserApiResponseToUser(rawApiData);

// Loop variables with context
userNotifications.forEach((userNotification, notificationIndex) => {
  console.log(`Processing notification ${notificationIndex}: ${userNotification.message}`);
});

orderItems.map((orderItem, orderItemIndex) => ({
  ...orderItem,
  position: orderItemIndex + 1
}));

// Intermediate calculations
const baseOrderPrice = orderItems.reduce((sum, item) => sum + item.price, 0);
const calculatedTaxAmount = baseOrderPrice * TAX_RATE;
const finalOrderTotal = baseOrderPrice + calculatedTaxAmount + shippingCost;

// ❌ Bad - Generic temporary variables
const temp = users.filter(user => user.isActive);
const result = products.sort((a, b) => a.price - b.price);
const str = `${user.firstName} ${user.lastName}`;
const formatted = order.createdAt.toLocaleDateString();
const calc = orderItems.reduce((sum, item) => sum + item.price, 0);

// Generic loop variables
items.forEach((item, i) => { }); // What kind of items?
data.map((d, idx) => { }); // What is d?
list.filter((l) => { }); // What is l?
```

### API & Async Variables

Variables handling API responses and async operations should be explicit:

```typescript
// ✅ Good - Clear API response handling
const userApiResponse = await fetchUserProfile(userId);
const transformedUserData = mapUserApiResponseToUser(userApiResponse);
const cachedUserProfile = localStorage.getItem('userProfile');
const parsedUserPreferences = JSON.parse(userPreferencesJson);

// Error handling
const userFetchError = await fetchUserProfile(userId).catch(err => err);
const orderSubmissionError = await submitOrder(orderData).catch(err => err);
const paymentProcessingError = await processPayment(paymentInfo).catch(err => err);

// Loading states
const isUserProfileLoading = userProfileQuery.isLoading;
const isOrderSubmissionInProgress = orderMutation.isLoading;
const hasUserDataLoadingFailed = userQuery.isError;

// ❌ Bad - Generic API handling
const response = await fetchUserProfile(userId);
const data = mapUserApiResponseToUser(response);
const cached = localStorage.getItem('userProfile');
const parsed = JSON.parse(userPreferencesJson);

// Generic error names
const error = await fetchUserProfile(userId).catch(err => err);
const err = await submitOrder(orderData).catch(err => err);
const e = await processPayment(paymentInfo).catch(err => err);
```

### Configuration & Constants

Configuration variables should be descriptive and indicate their scope:

```typescript
// ✅ Good - Descriptive configuration
const userTableConfiguration = {
  defaultPageSize: 20,
  sortableColumns: ['name', 'email', 'createdAt'],
  filterableFields: ['status', 'role']
};

const orderFormValidationRules = {
  requiredFields: ['customerEmail', 'shippingAddress'],
  minimumOrderValue: 10.00,
  maximumOrderItems: 100
};

// Feature flags
const enabledUserFeatures = {
  canExportData: true,
  canDeleteAccount: false,
  canChangeEmail: true
};

// ❌ Bad - Generic configuration
const config = {
  pageSize: 20,
  sortBy: 'createdAt'
};

const settings = {
  debounceMs: 300,
  minSearchLength: 3
};

const rules = {
  required: ['email'],
  min: 10.00
};
```

### Variable Naming Anti-Patterns

```typescript
// ❌ Avoid these patterns:

// 1. Single letter variables (except in very short, obvious loops)
const u = getCurrentUser(); // → currentUser
const p = getProduct(); // → selectedProduct
const e = getEvent(); // → userClickEvent

// 2. Meaningless names
const thing = getUserData(); // → userProfileData
const stuff = getOrderItems(); // → orderLineItems
const foo = calculateTotal(); // → calculatedOrderTotal

// 3. Hungarian notation (type prefixes)
const strUserName = user.name; // → userName
const bIsValid = checkForm(); // → isFormValid
const arrProducts = getProducts(); // → products or productList
const objUser = getUser(); // → user or userProfile

// 4. Misleading names
const userList = getActiveUsers(); // → activeUsers (if it's only active users)
const getData = () => getUserProfile(); // → getUserProfile
const submitForm = formData; // → userFormData (if it's not a function)

// 5. Mental mapping required
const d = new Date(); // → currentDate or orderCreatedDate
const temp = calculate(); // → calculatedOrderTotal
const result = process(); // → processedUserData
const val = getValue(); // → selectedOptionValue

// 6. Inconsistent abbreviations
const usr = getUser(); // → user
const prod = getProduct(); // → product
const addr = getAddress(); // → shippingAddress
const qty = getQuantity(); // → itemQuantity

// 7. Context-free names
const data = apiResponse.data; // → userData or orderData
const items = response.items; // → orderItems or productItems
const list = getList(); // → userNotificationList
const value = input.value; // → userEmailValue or searchQueryValue
```

### Variable Scope Best Practices

```typescript
// ✅ Component-level variables
const UserProfileComponent = () => {
  const currentUser = useCurrentUser();
  const userProfileForm = useForm<UserProfileForm>();
  const isUserProfileEditing = userProfileForm.formState.isDirty;
  
  const handleUserProfileSubmit = (userFormData: UserProfileForm) => {
    // ✅ Function-level variables
    const userValidationResult = validateUserProfileForm(userFormData);
    const updatedUserProfile = { ...currentUser, ...userFormData };
    
    if (userValidationResult.isValid) {
      submitUserProfileUpdate(updatedUserProfile);
    }
  };
  
  return (
    <form onSubmit={handleUserProfileSubmit}>
      {/* Component JSX */}
    </form>
  );
};

// ✅ Module-level variables
const DEFAULT_USER_AVATAR_URL = '/images/default-avatar.png';
const MAX_FILE_UPLOAD_SIZE_MB = 10;
const USER_SESSION_TIMEOUT_MS = 30 * 60 * 1000; // 30 minutes

const userApiEndpoints = {
  profile: '/api/users/profile',
  preferences: '/api/users/preferences',
  notifications: '/api/users/notifications'
} as const;
```

**Remember**: Variable names should be **self-documenting** and eliminate the need for comments or mental mapping. When in doubt, choose the more descriptive name over the shorter one.

## Functions & Event Handlers

All functions and methods should use **camelCase** and strictly follow the **A/HC/LC pattern** to ensure consistency, predictability, and self-documenting code.

### A/HC/LC Pattern Structure

Based on [kettanaito's naming cheatsheet](https://github.com/kettanaito/naming-cheatsheet?tab=readme-ov-file#ahclc-pattern), follow this structure for **every function and method**:

**Format**: `action` + `highContext` + `lowContext`

- **A (Action)** - The function verb (get, set, reset, fetch, etc.)
- **HC (High Context)** - The domain/entity (User, Product, Order, etc.)
- **LC (Low Context)** - The specific detail (Profile, Email, Status, etc.)

```typescript
// Pattern: [action][HighContext][LowContext]
const getUserProfile = () => { };     // get + User + Profile
const setUserPreferences = () => { }; // set + User + Preferences
const resetPasswordForm = () => { };  // reset + Password + Form
```

### Common Actions

| Action | Purpose | Example |
|--------|---------|---------|
| **get** | Retrieve data | `getUserProfile`, `getOrderStatus` |
| **set** | Assign/update data | `setUserEmail`, `setProductPrice` |
| **reset** | Clear/restore defaults | `resetLoginForm`, `resetUserFilters` |
| **fetch** | Async data retrieval | `fetchUserOrders`, `fetchProductDetails` |
| **create** | Generate new entities | `createUserAccount`, `createOrderSummary` |
| **update** | Modify existing data | `updateUserProfile`, `updateOrderStatus` |
| **delete** | Remove entities | `deleteUserSession`, `deleteOrderItem` |
| **validate** | Check data validity | `validateUserEmail`, `validateOrderTotal` |
| **calculate** | Compute values | `calculateOrderTax`, `calculateUserAge` |
| **format** | Transform data display | `formatUserDate`, `formatOrderCurrency` |
| **map** | Transform data structure | `mapUserToProfile`, `mapOrderToSummary` |
| **parse** | Convert data types | `parseUserInput`, `parseOrderResponse` |
| **handle** | Event processing | `handleUserClick`, `handleFormSubmit` |
| **process** | Complex operations | `processOrderPayment`, `processUserAnalytics` |
| **generate** | Create new content | `generateUserToken`, `generateOrderInvoice` |
| **transform** | Convert between formats | `transformUserData`, `transformOrderData` |

### Core Function Categories

#### Data Retrieval Functions
```typescript
// ✅ A/HC/LC Pattern for data access
const getUserProfile = (userId: string): UserProfile | null => {
  return JSON.parse(localStorage.getItem(`user_${userId}`) || 'null');
};

const getOrderHistory = async (userId: string): Promise<Order[]> => {
  const response = await fetch(`/api/users/${userId}/orders`);
  return response.json();
};

const getStoredUserPreferences = (): UserPreferences => {
  const defaultPreferences = { theme: 'light', language: 'en' };
  const stored = localStorage.getItem('userPreferences');
  return stored ? { ...defaultPreferences, ...JSON.parse(stored) } : defaultPreferences;
};

// ❌ Inconsistent patterns
const getProfile = (userId: string): UserProfile => { }; // Missing HC
const fetchUser = (userId: string): UserProfile => { }; // Different action verb
const userProfile = (userId: string): UserProfile => { }; // Missing action
```

#### Data Modification Functions
```typescript
// ✅ A/HC/LC Pattern for data modifications
const updateUserProfile = async (userId: string, profileData: Partial<UserProfile>): Promise<UserProfile> => {
  const response = await fetch(`/api/users/${userId}`, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(profileData)
  });
  return response.json();
};

const deleteOrderItem = (orderId: string, itemId: string): void => {
  const currentCart = getCartFromLocalStorage();
  const updatedCart = currentCart.filter(item => item.id !== itemId);
  localStorage.setItem('cart', JSON.stringify(updatedCart));
};

const createProductReview = async (productId: string, reviewData: CreateReviewRequest): Promise<Review> => {
  const response = await fetch(`/api/products/${productId}/reviews`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(reviewData)
  });
  return response.json();
};

const resetUserPassword = async (email: string): Promise<void> => {
  await fetch('/api/auth/reset-password', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ email })
  });
};

const setUserThemePreference = (theme: 'light' | 'dark'): void => {
  localStorage.setItem('userTheme', theme);
  document.documentElement.setAttribute('data-theme', theme);
};

const clearUserSessionData = (): void => {
  sessionStorage.removeItem('userToken');
  sessionStorage.removeItem('userPermissions');
  localStorage.removeItem('lastLoginTime');
};

// ❌ Inconsistent patterns  
const updateUser = (userId: string, data: any): Promise<any> => { }; // Too generic
const deleteItem = (itemId: string): void => { }; // Missing HC
const create = (data: any): Promise<any> => { }; // Missing action context
```

#### Validation Functions
```typescript
// ✅ A/HC/LC Pattern for  validation
const validateUserEmail = (email: string): ValidationResult => {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return {
    isValid: emailRegex.test(email),
    errorMessage: emailRegex.test(email) ? null : 'Please enter a valid email address'
  };
};

const validateOrderTotal = (cartItems: CartItem[]): ValidationResult => {
  const calculatedTotal = cartItems.reduce((sum, item) => sum + (item.price * item.quantity), 0);
  const isValid = calculatedTotal > 0;
  return {
    isValid,
    errorMessage: isValid ? null : 'Cart cannot be empty'
  };
};

const validateProductStock = (product: Product, requestedQuantity: number): ValidationResult => {
  const isValid = product.stockQuantity >= requestedQuantity && requestedQuantity > 0;
  return {
    isValid,
    errorMessage: isValid ? null : `Only ${product.stockQuantity} items available`
  };
};

const validateFormFields = (formData: Record<string, any>, requiredFields: string[]): ValidationResult => {
  const missingFields = requiredFields.filter(field => !formData[field] || formData[field].trim() === '');
  const isValid = missingFields.length === 0;
  return {
    isValid,
    errorMessage: isValid ? null : `Please fill in: ${missingFields.join(', ')}`
  };
};

const validateFileUpload = (file: File, maxSizeMB: number, allowedTypes: string[]): ValidationResult => {
  const maxSizeBytes = maxSizeMB * 1024 * 1024;
  
  if (file.size > maxSizeBytes) {
    return { isValid: false, errorMessage: `File size must be less than ${maxSizeMB}MB` };
  }
  
  if (!allowedTypes.includes(file.type)) {
    return { isValid: false, errorMessage: `File type must be: ${allowedTypes.join(', ')}` };
  }
  
  return { isValid: true, errorMessage: null };
};

// ❌ Bad - Generic validation
const validate = (data: any): boolean => { }; // What is being validated?
const isValid = (input: string): boolean => { }; // Valid according to what?
const checkEmail = (email: string): boolean => { }; // Inconsistent action verb
```

#### Calculation Functions
```typescript
// ✅ A/HC/LC Pattern for  calculations
const calculateOrderTax = (subtotal: number, taxRate: number): number => {
  return Math.round(subtotal * taxRate * 100) / 100; // Round to 2 decimal places
};

const calculateReadingTime = (textContent: string): number => {
  const wordsPerMinute = 200;
  const wordCount = textContent.trim().split(/\s+/).length;
  return Math.ceil(wordCount / wordsPerMinute);
};

const calculateComponentHeight = (element: HTMLElement): number => {
  const computedStyle = window.getComputedStyle(element);
  const height = element.offsetHeight;
  const padding = parseFloat(computedStyle.paddingTop) + parseFloat(computedStyle.paddingBottom);
  return height - padding;
};

// ❌ Inconsistent patterns
const orderSummary = (cartItems: CartItem[]): number => { }; // Missing action
const getTotal = (items: any[]): number => { }; // Missing HC
const calcShipping = (total: number): number => { }; // Abbreviated action
```

#### Formatting Functions
```typescript
// ✅ A/HC/LC Pattern for formatting
const formatUserDisplayName = (user: User): string => {
  if (user.middleName) {
    return `${user.firstName} ${user.middleName} ${user.lastName}`;
  }
  return `${user.firstName} ${user.lastName}`;
};

const formatOrderCurrency = (amount: number, currency: string = 'USD'): string => {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency: currency
  }).format(amount);
};

const formatProductAvailability = (product: Product): string => {
  if (product.stockQuantity === 0) return 'Out of Stock';
  if (product.stockQuantity < 5) return 'Limited Stock';
  return 'In Stock';
};

const formatUserLastSeen = (lastSeenAt: Date): string => {
  const now = new Date();
  const diffInHours = (now.getTime() - lastSeenAt.getTime()) / (1000 * 60 * 60);
  
  if (diffInHours < 1) return 'Active now';
  if (diffInHours < 24) return `${Math.floor(diffInHours)} hours ago`;
  return `${Math.floor(diffInHours / 24)} days ago`;
};
```

### React-Specific Function Patterns

#### Event Handlers
```typescript
// ✅ A/HC/LC Pattern for React events
const handleUserFormSubmit = (formData: UserForm): void => { };
const handleProductCardClick = (productId: string): void => { };
const handleOrderTableChange = (pagination: any, filters: any): void => { };
const handleUserModalClose = (): void => { };
const handlePasswordInputChange = (e: React.ChangeEvent<HTMLInputElement>): void => { };

// ❌ Generic event handlers (avoid when possible)
const handleSubmit = (formData: any): void => { }; // Too generic
const onClick = (id: string): void => { }; // Not descriptive
const onChange = (e: any): void => { }; // No context
```

#### Custom Hooks
```typescript
// ✅ A/HC/LC Pattern with 'use' prefix
const useUserAuthentication = (): AuthState => { };
const useProductFiltering = (filters: ProductFilters): FilteredProducts => { };
const useOrderValidation = (order: Order): ValidationResult => { };
const useFormSubmission = (endpoint: string): SubmissionState => { };

// ❌ Inconsistent hook patterns
const useAuth = (): AuthState => { }; // Missing context
const userAuthentication = (): AuthState => { }; // Missing 'use' prefix
const useData = (id: string): any => { }; // Too generic
```

#### API Functions
```typescript
// ✅ A/HC/LC Pattern for API calls
const fetchUserProfile = async (userId: string): Promise<UserProfile> => {
  const response = await fetch(`/api/users/${userId}`);
  if (!response.ok) {
    throw new Error(`Failed to fetch user profile: ${response.statusText}`);
  }
  return response.json();
};

const createUserAccount = async (userData: CreateUserRequest): Promise<User> => {
  const response = await fetch('/api/users', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(userData)
  });
  return response.json();
};

const updateProductInventory = async (productId: string, quantity: number): Promise<void> => {
  await fetch(`/api/products/${productId}/inventory`, {
    method: 'PATCH',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ quantity })
  });
};

const deleteOrderItem = async (orderId: string, itemId: string): Promise<void> => {
  await fetch(`/api/orders/${orderId}/items/${itemId}`, {
    method: 'DELETE'
  });
};

// ❌ Inconsistent API patterns
const apiGetUser = async (userId: string): Promise<UserProfile> => { }; // Unnecessary prefix
const userCreate = async (userData: CreateUserRequest): Promise<User> => { }; // Wrong structure
const removeItem = async (orderId: string, itemId: string): Promise<void> => { }; // Missing HC
```

### Advanced Function Patterns

#### Async & Complex Operations
```typescript
// ✅ A/HC/LC Pattern for async operations
const processOrderPayment = async (order: Order): Promise<PaymentResult> => {
  return this.paymentService.processPayment(order.paymentInfo);
};

const uploadProductImage = async (file: File, productId: string): Promise<ImageUploadResult> => {
  const formData = new FormData();
  formData.append('image', file);
  formData.append('productId', productId);
  
  const response = await fetch('/api/products/images', {
    method: 'POST',
    body: formData
  });
  return response.json();
};

const processUserAnalytics = async (userId: string): Promise<void> => {
  await analyticsService.processUserData(userId);
};

const generateOrderInvoice = async (orderId: string): Promise<InvoiceDocument> => {
  const order = await getOrderDetails(orderId);
  return invoiceService.generatePDF(order);
};
```

#### Utility Functions with Context
```typescript
// ✅ A/HC/LC Pattern for utilities
const generateUniqueUserId = () => {
  return `user_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
};

const sanitizeUserInput = (input: string): string => {
  return input.trim().replace(/[<>]/g, '');
};

const debounceUserSearch = (searchFunction: Function, delay: number) => {
  let timeoutId: NodeJS.Timeout;
  return (...args: any[]) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => searchFunction.apply(null, args), delay);
  };
};

const transformUserDataForDisplay = (
  user: User,
  displayOptions: UserDisplayOptions
): UserDisplayData => {
  return {
    displayName: formatUserDisplayName(user),
    avatar: user.profileImage?.url || DEFAULT_AVATAR_URL,
    memberSince: formatUserMembershipDate(user.createdAt),
    isOnline: isUserCurrentlyOnline(user.lastSeenAt)
  };
};
```

#### Compound Actions
```typescript
// ✅ Complex operations following A/HC/LC
const validateAndSaveUserProfile = (profile: UserProfile): Promise<ValidationResult> => { };
const fetchAndCacheProductData = (productId: string): Promise<Product> => { };
const calculateAndFormatOrderTotal = (order: Order): string => { };
const resetAndRefreshUserDashboard = (userId: string): Promise<void> => { };

// ✅ Alternative: Break into separate functions
const validateUserProfile = (profile: UserProfile): ValidationResult => { };
const saveUserProfile = (profile: UserProfile): Promise<void> => { };
const executeUserProfileUpdate = async (profile: UserProfile): Promise<void> => {
  const validation = validateUserProfile(profile);
  if (validation.isValid) {
    await saveUserProfile(profile);
  }
};
```

#### Conditional Actions
```typescript
// ✅ Boolean check functions
const canUserEditProfile = (user: User): boolean => { };
const hasOrderExpired = (order: Order): boolean => { };
const shouldProductBeDiscounted = (product: Product): boolean => { };
const isUserAuthenticated = (user: User): boolean => { };

// ✅ State check functions  
const isProductAvailable = (product: Product): boolean => { };
const hasUserCompletedOnboarding = (user: User): boolean => { };
const areOrderItemsValid = (items: OrderItem[]): boolean => { };
```

#### Error Handling Functions
```typescript
// ✅ A/HC/LC Pattern for error handling
const handleUserAuthenticationError = (error: AuthError): void => {
  if (error.code === 'INVALID_TOKEN') {
    redirectToLogin();
  } else if (error.code === 'TOKEN_EXPIRED') {
    refreshUserToken();
  } else {
    showUserErrorMessage('Authentication failed. Please try again.');
  }
};

const retryOrderSubmission = async (
  orderData: Order,
  maxAttempts: number = 3
): Promise<Order> => {
  let lastError: Error;
  
  for (let attempt = 1; attempt <= maxAttempts; attempt++) {
    try {
      return await submitOrder(orderData);
    } catch (error) {
      lastError = error as Error;
      if (attempt === maxAttempts) break;
      await delay(attempt * 1000);
    }
  }
  
  throw new Error(`Order submission failed after ${maxAttempts} attempts: ${lastError.message}`);
};

const validateAndThrowUserError = (userData: UserData): void => {
  const validationErrors: string[] = [];
  
  if (!userData.email) validationErrors.push('Email is required');
  if (!userData.firstName) validationErrors.push('First name is required');
  
  if (validationErrors.length > 0) {
    throw new ValidationError(`User validation failed: ${validationErrors.join(', ')}`);
  }
};
```

### Function Composition & Higher-Order Functions

```typescript
// ✅ A/HC/LC Pattern for composition
const createUserProfileValidator = (rules: ValidationRule[]) => {
  return (userData: UserProfileData): ValidationResult => {
    return rules.reduce((result, rule) => {
      if (!result.isValid) return result;
      return rule.validate(userData);
    }, { isValid: true, errors: [] });
  };
};

const withUserAuthentication = <T extends any[], R>(
  fn: (...args: T) => R
) => {
  return (...args: T): R => {
    const currentUser = getCurrentUser();
    if (!currentUser.isAuthenticated) {
      throw new Error('User must be authenticated');
    }
    return fn(...args);
  };
};

// ❌ Bad - Generic composition
const createValidator = (rules: any[]) => { }; // Validator for what?
const createFilter = (criteria: any) => { }; // Filter what?
const withAuth = (fn: Function) => { }; // Auth for what?
```

### Function Documentation & Type Safety

Functions should be self-documenting through naming, with TypeScript providing additional safety:

```typescript
// ✅ Good - Self-documenting with types
const transformUserDataForDisplay = (
  user: User,
  displayOptions: UserDisplayOptions
): UserDisplayData => {
  return {
    displayName: formatUserDisplayName(user),
    avatar: user.profileImage?.url || DEFAULT_AVATAR_URL,
    memberSince: formatUserMembershipDate(user.createdAt),
    isOnline: isUserCurrentlyOnline(user.lastSeenAt),
    permissions: filterUserVisiblePermissions(user.permissions, displayOptions.showPermissions)
  };
};

const executeOrderWorkflow = async (
  order: Order,
  workflowSteps: OrderWorkflowStep[]
): Promise<OrderWorkflowResult> => {
  const results: StepResult[] = [];
  
  for (const step of workflowSteps) {
    const stepResult = await executeOrderWorkflowStep(order, step);
    results.push(stepResult);
    
    if (!stepResult.isSuccessful) {
      return {
        isCompleted: false,
        failedStep: step.name,
        results
      };
    }
  }
  
  return {
    isCompleted: true,
    results
  };
};

// Function overloads for different use cases
function searchUsersByEmail(email: string): Promise<User[]>;
function searchUsersByEmail(email: string, exactMatch: true): Promise<User | null>;
function searchUsersByEmail(email: string, exactMatch?: boolean): Promise<User[] | User | null> {
  if (exactMatch) {
    return this.userRepository.findByEmail(email);
  }
  return this.userRepository.searchByEmailPattern(email);
}
```

### A/HC/LC Pattern Benefits

1. **Predictability** - Easy to guess function names following the pattern
2. **Searchability** - Consistent naming makes code easier to search
3. **Grouping** - Functions naturally group by high context (User*, Product*, Order*)
4. **Scalability** - Pattern grows consistently with codebase
5. **Clarity** - Clear action and context in every function name
6. **Self-Documentation** - Function names tell you exactly what they do
7. **Maintainability** - Consistent patterns make refactoring easier
8. **Team Collaboration** - Everyone follows the same naming rules

**Remember**: Every function name should clearly communicate its **action**, **domain context**, and **specific purpose** following the A/HC/LC pattern. The function signature and implementation should fulfill exactly what the name promises.

### Event Handlers

Event handlers should use **camelCase** and follow the **A/HC/LC pattern** with `handle` as the primary action verb. All event handlers must be descriptive and indicate both the source and the type of event being handled.

#### Event Handler Pattern

**Format**: `handle` + `HighContext` + `LowContext` + `Event`

- **Action**: Always `handle`
- **HC (High Context)**: The component/domain (User, Product, Form, Modal, etc.)
- **LC (Low Context)**: The specific element or action (Button, Input, Submit, Close, etc.)
- **Event**: The event type (Click, Change, Submit, etc.)

```typescript
// Pattern: handle[HighContext][LowContext][Event]
const handleUserProfileSubmit = () => { };     // handle + User + Profile + Submit
const handleProductCardClick = () => { };      // handle + Product + Card + Click
const handlePasswordInputChange = () => { };   // handle + Password + Input + Change
```

#### Form Event Handlers

```typescript
// ✅ A/HC/LC Pattern for form events
const handleUserRegistrationSubmit = (formData: UserRegistrationForm): void => {
  validateAndSubmitUserRegistration(formData);
};

const handleUserEmailChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
  const email = e.target.value;
  setUserEmail(email);
  validateUserEmailFormat(email);
};

const handleProductReviewSubmit = async (reviewData: ProductReviewForm): Promise<void> => {
  try {
    await submitProductReview(reviewData);
    showSuccessNotification('Review submitted successfully');
    resetProductReviewForm();
  } catch (error) {
    showErrorNotification('Failed to submit review');
  }
};

const handleUserPreferencesReset = (): void => {
  resetUserPreferencesForm();
  loadDefaultUserPreferences();
};

// ❌ Bad - Generic form handlers
const handleSubmit = (data: any): void => { }; // Submit what?
const handleChange = (e: any): void => { }; // Change on what?
const onSubmit = (data: any): void => { }; // Missing 'handle' prefix
const submitForm = (): void => { }; // Not an event handler pattern
```

#### Click Event Handlers

```typescript
// ✅ A/HC/LC Pattern for click events
const handleUserProfileEditClick = (): void => {
  setIsUserProfileEditing(true);
  focusFirstFormField();
};

// Button-specific click handlers
const handleUserAccountSaveClick = (): void => {
  validateAndSaveUserAccount();
};

const handleOrderRetryPaymentClick = (orderId: string): void => {
  retryOrderPayment(orderId);
};

// ❌ Bad - Generic click handlers
const handleClick = (): void => { }; // Click on what?
const onClick = (): void => { }; // Missing context
const clickHandler = (): void => { }; // Wrong naming pattern
const handleButtonClick = (): void => { }; // Which button?
```

#### Modal & Dialog Event Handlers

```typescript
// ✅ A/HC/LC Pattern for modal events
const handleUserConfirmationModalOpen = (): void => {
  setIsUserConfirmationModalVisible(true);
  focusModalConfirmButton();
};

const handleUserConfirmationModalClose = (): void => {
  setIsUserConfirmationModalVisible(false);
  clearModalState();
};

const handleOrderCancellationModalConfirm = (orderId: string): void => {
  cancelOrder(orderId);
  closeOrderCancellationModal();
  showOrderCancellationSuccess();
};

const handleUserLogoutModalCancel = (): void => {
  setIsUserLogoutModalVisible(false);
  resetLogoutTimer();
};

const handleFileUploadModalDismiss = (): void => {
  clearSelectedFiles();
  resetFileUploadProgress();
  setIsFileUploadModalVisible(false);
};

// ❌ Bad - Generic modal handlers
const handleModalOpen = (): void => { }; // Which modal?
const handleClose = (): void => { }; // Close what?
const handleConfirm = (): void => { }; // Confirm what?
const onModalClose = (): void => { }; // Missing 'handle' prefix
```

#### Input & Selection Event Handlers

```typescript
// ✅ A/HC/LC Pattern for input events
const handleUserSearchQueryChange = (e: React.ChangeEvent<HTMLInputElement>): void => {
  const query = e.target.value;
  setUserSearchQuery(query);
  debouncedUserSearch(query);
};

const handleProductCategorySelect = (categoryId: string): void => {
  setSelectedProductCategory(categoryId);
  filterProductsByCategory(categoryId);
  updateUrlWithCategory(categoryId);
};

const handleOrderStatusFilterChange = (status: OrderStatus): void => {
  setSelectedOrderStatus(status);
  filterOrdersByStatus(status);
};

const handleUserCountrySelect = (countryCode: string): void => {
  setSelectedUserCountry(countryCode);
  loadStatesByCountry(countryCode);
  updateShippingOptions(countryCode);
};

// ❌ Bad - Generic input handlers
const handleInputChange = (e: any): void => { }; // Which input?
const handleSelect = (value: any): void => { }; // Select what?
const onChange = (value: any): void => { }; // Missing 'handle' prefix
const handleChange = (e: any): void => { }; // Too generic
```

#### Table & List Event Handlers

```typescript
// ✅ A/HC/LC Pattern for table/list events
const handleUserTableRowClick = (userId: string): void => {
  navigateToUserDetails(userId);
  highlightSelectedUserRow(userId);
};

const handleOrderTableSortChange = (column: string, direction: 'asc' | 'desc'): void => {
  setOrderTableSortConfig({ column, direction });
  sortOrderTableData(column, direction);
};

const handleOrderListItemSelect = (selectedOrderIds: string[]): void => {
  setSelectedOrderIds(selectedOrderIds);
  updateBulkActionVisibility(selectedOrderIds.length > 0);
};

const handleProductGridViewToggle = (): void => {
  const newView = productViewMode === 'grid' ? 'list' : 'grid';
  setProductViewMode(newView);
  saveUserViewPreference(newView);
};

// ❌ Bad - Generic table handlers
const handleRowClick = (id: string): void => { }; // Row of what?
const handleSort = (column: string): void => { }; // Sort what?
const handlePageChange = (page: number): void => { }; // Page of what?
const onTableChange = (config: any): void => { }; // Missing 'handle' prefix
```

#### File Upload Event Handlers

```typescript
// ✅ A/HC/LC Pattern for file upload events
const handleUserAvatarFileSelect = (files: FileList): void => {
  const selectedFile = files[0];
  if (validateUserAvatarFile(selectedFile)) {
    setSelectedUserAvatarFile(selectedFile);
    previewUserAvatarImage(selectedFile);
  }
};

const handleProductImageFilesDrop = (files: FileList): void => {
  const validFiles = Array.from(files).filter(validateProductImageFile);
  setSelectedProductImageFiles(validFiles);
  generateProductImagePreviews(validFiles);
};

const handleDocumentFileUploadProgress = (progressEvent: ProgressEvent): void => {
  const uploadProgress = Math.round((progressEvent.loaded / progressEvent.total) * 100);
  setDocumentUploadProgress(uploadProgress);
};

const handleUserAvatarUploadComplete = (uploadResult: FileUploadResult): void => {
  setUserAvatarUrl(uploadResult.url);
  clearSelectedUserAvatarFile();
  showAvatarUploadSuccess();
};

const handleProductImageUploadError = (error: FileUploadError): void => {
  showFileUploadError(error.message);
  resetProductImageUpload();
};

const handleBulkFileUploadCancel = (): void => {
  cancelAllFileUploads();
  clearSelectedFiles();
  resetUploadProgress();
};

// ❌ Bad - Generic file handlers
const handleFileSelect = (files: FileList): void => { }; // Files for what?
const handleDrop = (files: FileList): void => { }; // Drop where?
const handleUpload = (): void => { }; // Upload what?
const onFileChange = (e: any): void => { }; // Missing 'handle' prefix
```

#### Event Handler Best Practices

##### Type Safety
```typescript
// ✅ Good - Proper TypeScript typing
const handleUserFormSubmit = (e: React.FormEvent<HTMLFormElement>): void => {
  e.preventDefault();
  const formData = new FormData(e.currentTarget);
  processUserFormSubmission(formData);
};

const handleProductSelectChange = (e: React.ChangeEvent<HTMLSelectElement>): void => {
  const selectedProductId = e.target.value;
  loadProductDetails(selectedProductId);
};

const handleUserButtonClick = (e: React.MouseEvent<HTMLButtonElement>): void => {
  e.stopPropagation();
  executeUserAction();
};

// ❌ Bad - Poor typing
const handleSubmit = (e: any): void => { }; // Any type
const handleChange = (e): void => { }; // No type annotation
const handleClick = (e: Event): void => { }; // Wrong event type
```

##### Component Context
```typescript
// ✅ Good - Clear component context
const UserProfileCard = () => {
  const handleUserProfileEditClick = (): void => {
    setIsEditing(true);
  };
  
  const handleUserProfileSaveClick = (): void => {
    saveUserProfile();
    setIsEditing(false);
  };
  
  const handleUserProfileCancelClick = (): void => {
    resetUserProfileForm();
    setIsEditing(false);
  };
  
  return (
    // JSX with event handlers
  );
};

// ❌ Bad - Generic handlers without context
const UserProfileCard = () => {
  const handleEdit = (): void => { }; // Edit what?
  const handleSave = (): void => { }; // Save what?
  const handleCancel = (): void => { }; // Cancel what?
};
```

##### Async Handler Patterns
```typescript
// ✅ Good - Proper async handling with loading states
const handleUserDataSubmit = async (userData: UserData): Promise<void> => {
  setIsSubmitting(true);
  setSubmissionError(null);
  
  try {
    await submitUserData(userData);
    showSubmissionSuccess();
    resetForm();
  } catch (error) {
    setSubmissionError(error.message);
    showSubmissionError();
  } finally {
    setIsSubmitting(false);
  }
};

// ❌ Bad - No error handling or loading states
const handleSubmit = async (data: any): Promise<void> => {
  await submitData(data); // No error handling
};
```

## React Components

React components should use **PascalCase** and follow descriptive naming that clearly indicates their purpose, scope, and functionality.

### Component Naming Structure

**Format**: `[Domain][Entity][ComponentType]`

```typescript
// ✅ Good - Clear domain and purpose
const UserProfileCard = () => { };        // User domain, Profile entity, Card component
const ProductSearchForm = () => { };      // Product domain, Search entity, Form component
const OrderSummaryModal = () => { };      // Order domain, Summary entity, Modal component
const UserNotificationList = () => { };   // User domain, Notification entity, List component

// ❌ Bad - Generic or unclear
const ProfileCard = () => { };    // Missing domain context
const SearchForm = () => { };     // Search for what?
const Modal = () => { };          // Modal for what?
```

### Core Component Categories

#### Layout & Container Components
```typescript
// ✅ Layout components with clear purpose
const AppLayoutContainer = ({ children }) => {
  return (
    <div className="app-layout">
      <AppNavigationHeader />
      <main className="app-content">{children}</main>
      <AppLayoutFooter />
    </div>
  );
};

const UserDashboardLayout = ({ children }) => {
  // ... existing layout logic ...
  return (
    <div className="user-dashboard-layout">
      <UserNavigationSidebar />
      <div className="dashboard-content">{children}</div>
    </div>
  );
};

// ❌ Bad - Generic layout names
const Layout = ({ children }) => { }; // Layout for what?
const Container = ({ children }) => { }; // Container for what?
```

#### Display Components (Cards, Items, Previews)
```typescript
// ✅ Clear display components
const UserProfileCard: React.FC<UserProfileCardProps> = ({ user, isEditable, onEditClick }) => {
  const handleUserProfileEditClick = () => {
    onEditClick?.();
  };

  return (
    <div className="user-profile-card">
      <UserAvatarImage src={user.profileImage} alt={user.displayName} />
      <div className="user-profile-info">
        <h3>{user.displayName}</h3>
        {/* ... existing profile content ... */}
        {isEditable && (
          <Button onClick={handleUserProfileEditClick}>Edit Profile</Button>
        )}
      </div>
    </div>
  );
};

const ProductCatalogItem: React.FC<{ product: Product }> = ({ product }) => {
  const handleProductCardClick = () => {
    navigateToProductDetails(product.id);
  };

  return (
    <div className="product-catalog-item" onClick={handleProductCardClick}>
      {/* ... existing product display ... */}
    </div>
  );
};

// ❌ Bad - Generic display components
const Card = ({ data }) => { }; // Card for what data?
const Item = ({ item }) => { }; // Item of what type?
```

#### Form Components
```typescript
// ✅ Specific form components
const UserRegistrationForm: React.FC<UserRegistrationFormProps> = ({
  onSubmit,
  isSubmitting,
  validationErrors
}) => {
  const [userFormData, setUserFormData] = useState<UserRegistrationForm>({
    // ... existing form state ...
  });

  const handleUserRegistrationSubmit = (e: React.FormEvent) => {
    e.preventDefault();
    onSubmit(userFormData);
  };

  const handleUserEmailChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    setUserFormData(prev => ({ ...prev, email: e.target.value }));
  };

  return (
    <form onSubmit={handleUserRegistrationSubmit}>
      <UserEmailInput 
        value={userFormData.email}
        onChange={handleUserEmailChange}
        error={validationErrors.email}
      />
      {/* ... existing form fields ... */}
    </form>
  );
};

// ❌ Bad - Generic form components
const Form = ({ fields, onSubmit }) => { }; // Form for what?
const RegistrationForm = ({ onSubmit }) => { }; // Missing domain context
```

#### Modal & Dialog Components
```typescript
// ✅ Specific modal components
const UserAccountDeleteModal: React.FC<UserAccountDeleteModalProps> = ({
  isVisible,
  onConfirm,
  onCancel,
  isDeleting
}) => {
  const handleUserAccountDeleteConfirm = () => {
    onConfirm();
  };

  return (
    <Modal
      title="Delete Account"
      open={isVisible}
      onCancel={onCancel}
      footer={[
        <Button key="cancel" onClick={onCancel}>Cancel</Button>,
        <Button key="delete" danger loading={isDeleting} onClick={handleUserAccountDeleteConfirm}>
          Delete Account
        </Button>
      ]}
    >
      {/* ... existing modal content ... */}
    </Modal>
  );
};

// ❌ Bad - Generic modal components
const ConfirmModal = ({ onConfirm }) => { }; // Confirm what?
const DeleteModal = ({ onDelete }) => { }; // Delete what?
```

#### Input & Control Components
```typescript
// ✅ Specific input components
const UserEmailInput: React.FC<UserEmailInputProps> = ({
  value,
  onChange,
  error,
  disabled
}) => {
  const handleUserEmailChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    onChange(e.target.value);
  };

  const handleUserEmailBlur = () => {
    const validationResult = validateUserEmailFormat(value);
    // ... existing validation logic ...
  };

  return (
    <div className="user-email-input">
      <label htmlFor="user-email">Email Address</label>
      <Input
        id="user-email"
        type="email"
        value={value}
        onChange={handleUserEmailChange}
        onBlur={handleUserEmailBlur}
        status={error ? 'error' : ''}
      />
      {/* ... existing error display ... */}
    </div>
  );
};

// ❌ Bad - Generic input components
const Input = ({ value, onChange }) => { }; // Input for what?
const Select = ({ options, onSelect }) => { }; // Select what?
```

### Component Props Naming

```typescript
// ✅ Props interfaces following component naming
interface UserProfileCardProps {
  user: User;
  isEditable?: boolean;
  onEditClick?: () => void;
  // ... existing prop definitions ...
}

interface ProductCatalogGridProps {
  products: Product[];
  onProductClick: (productId: string) => void;
  onSortChange: (config: ProductSortConfig) => void;
  // ... existing prop definitions ...
}

// ❌ Bad - Generic props naming
interface Props { }           // Props for what component?
interface CardProps { }       // Card for what?
interface FormProps { }       // Form for what?
```

### Component Anti-Patterns

```typescript
// ❌ Avoid these patterns:

// 1. Generic component names
const Component = () => { }; // → UserProfileComponent
const Widget = () => { }; // → UserNotificationWidget

// 2. Missing domain context
const Profile = () => { }; // → UserProfile
const Card = () => { }; // → ProductCard
const Form = () => { }; // → UserRegistrationForm

// 3. Inconsistent naming
const UserProfile = () => { }; // → UserProfileCard
const ProductsGrid = () => { }; // → ProductCatalogGrid
```

### Feature-Based Organization

```typescript
// ✅ Feature-based component structure
// User feature components
const UserProfileCard = () => { };
const UserProfileForm = () => { };
const UserNotificationList = () => { };
// ... existing user components ...

// Product feature components  
const ProductCatalogGrid = () => { };
const ProductDetailView = () => { };
const ProductReviewList = () => { };
// ... existing product components ...

// Order feature components
const OrderSummaryCard = () => { };
const OrderHistoryTable = () => { };
const OrderCancellationModal = () => { };
// ... existing order components ...
```

**Remember**: Component names should be **self-documenting** and clearly indicate their domain, purpose, and functionality. Follow PascalCase consistently and avoid generic names.

## Custom Hooks

Custom hooks should use **camelCase** with the **use** prefix and follow the **A/HC/LC pattern** to clearly indicate their purpose and domain.

### Hook Naming Structure

**Format**: `use` + `HighContext` + `LowContext`

```typescript
// ✅ Good - Clear domain and purpose
const useUserAuthentication = () => { };    // use + User + Authentication
const useProductFiltering = () => { };      // use + Product + Filtering
const useOrderValidation = () => { };       // use + Order + Validation
const useFormSubmission = () => { };        // use + Form + Submission

// ❌ Bad - Generic or unclear
const useAuth = () => { };          // Missing context - auth for what?
const useData = () => { };          // What kind of data?
const useAPI = () => { };           // Which API?
```

### Core Hook Categories

#### Data Fetching Hooks
```typescript
// ✅ Specific data fetching hooks
const useUserProfile = (userId: string) => { };
const useProductCatalog = (filters: ProductFilters) => { };
const useOrderHistory = (userId: string) => { };
const useShoppingCartItems = () => { };
const useUserNotifications = (userId: string) => { };

// ❌ Bad - Generic data hooks
const useData = (id: string) => { }; // Data for what?
const useFetch = (url: string) => { }; // Fetch what?
const useAPI = (endpoint: string) => { }; // Which API?
```

#### Form Management Hooks
```typescript
// ✅ Specific form hooks
const useUserRegistrationForm = () => { };
const useProductSearchForm = () => { };
const useOrderCheckoutForm = () => { };
const useUserProfileEditForm = () => { };
const usePasswordResetForm = () => { };

// ❌ Bad - Generic form hooks
const useForm = () => { }; // Form for what?
const useFormValidation = () => { }; // Validation for what form?
const useFormState = () => { }; // State for which form?
```

#### Authentication & Permission Hooks
```typescript
// ✅ Specific authentication hooks
const useUserAuthentication = () => { };
const useUserPermissions = (userId: string) => { };
const useAdminAccess = () => { };
const useUserSessionManagement = () => { };
const useUserRoleValidation = () => { };

// ❌ Bad - Generic auth hooks
const useAuth = () => { }; // Auth for what?
const usePermissions = () => { }; // Permissions for what?
const useSession = () => { }; // Session for what?
```

#### UI State Management Hooks
```typescript
// ✅ Specific UI state hooks
const useUserModalState = () => { };
const useProductTableState = () => { };
const useOrderFilterState = () => { };
const useNavigationMenuState = () => { };
const useSearchInputState = () => { };

// ❌ Bad - Generic UI hooks
const useModal = () => { }; // Modal for what?
const useTable = () => { }; // Table for what data?
const useToggle = () => { }; // Toggle what?
const useState = () => { }; // State for what?
```

#### Local Storage & Persistence Hooks
```typescript
// ✅ Specific storage hooks
const useUserPreferencesStorage = () => { };
const useShoppingCartStorage = () => { };
const useUserSessionStorage = () => { };
const useProductWishlistStorage = () => { };
const useOrderDraftStorage = () => { };

// ❌ Bad - Generic storage hooks
const useStorage = (key: string) => { }; // Storage for what?
const useLocalStorage = (key: string) => { }; // What data?
const usePersistence = () => { }; // Persistence of what?
```

#### Async Operations Hooks
```typescript
// ✅ Specific async operation hooks
const useOrderSubmission = () => { };
const useFileUpload = () => { };
const useUserEmailVerification = () => { };
const usePasswordReset = () => { };
const useDataExport = () => { };

// ❌ Bad - Generic async hooks
const useAsync = () => { }; // Async what?
const useSubmit = () => { }; // Submit what?
const useUpload = () => { }; // Upload what?
```

### Hook Type Definitions

```typescript
// ✅ Clear hook result types following naming pattern
interface UseUserProfileResult { }
interface UseProductCatalogResult { }
interface UseUserRegistrationFormResult { }
interface UseOrderSubmissionResult { }
interface UseShoppingCartStorageResult { }

// ❌ Bad - Generic result types
interface UseDataResult { } // Data for what?
interface UseFormResult { } // Form for what?
interface UseAPIResult { } // API for what?
```

### Hook Anti-Patterns

```typescript
// ❌ Avoid these hook naming patterns:

// 1. Missing 'use' prefix
const getUserData = () => { }; // → useUserData
const productFiltering = () => { }; // → useProductFiltering
const orderValidation = () => { }; // → useOrderValidation

// 2. Generic hook names
const useData = () => { }; // → useUserProfile or useProductData
const useAPI = () => { }; // → useUserAPI or useProductAPI
const useForm = () => { }; // → useUserRegistrationForm

// 3. Missing high context (domain)
const useAuth = () => { }; // → useUserAuthentication
const usePermissions = () => { }; // → useUserPermissions
const useValidation = () => { }; // → useFormValidation

// 4. Inconsistent naming within domain
const useUser = () => { }; // → useUserProfile
const getUserProducts = () => { }; // → useUserProducts
const userOrders = () => { }; // → useUserOrders

// 5. Abbreviations and unclear names
const useUsrAuth = () => { }; // → useUserAuthentication
const useProdCat = () => { }; // → useProductCatalog
const useOrdVal = () => { }; // → useOrderValidation
```

### Domain-Based Hook Organization

```typescript
// ✅ User domain hooks
const useUserAuthentication = () => { };
const useUserProfile = () => { };
const useUserPreferences = () => { };
const useUserNotifications = () => { };
const useUserPermissions = () => { };

// ✅ Product domain hooks
const useProductCatalog = () => { };
const useProductDetails = () => { };
const useProductSearch = () => { };
const useProductFiltering = () => { };
const useProductComparison = () => { };

// ✅ Order domain hooks
const useOrderHistory = () => { };
const useOrderSubmission = () => { };
const useOrderValidation = () => { };
const useOrderTracking = () => { };
const useOrderCancellation = () => { };

// ✅ Form domain hooks
const useUserRegistrationForm = () => { };
const useProductSearchForm = () => { };
const useOrderCheckoutForm = () => { };
const usePasswordResetForm = () => { };
const useContactForm = () => { };
```

**Remember**: Custom hook names should always start 
with `use`, follow the A/HC/LC pattern (use + 
HighContext + LowContext), and be 
**self-documenting** about their domain and purpose.

### Type & Interface Naming Structure

**Format**: `[Domain][Entity][TypePurpose]`

```typescript
// ✅ Good - Clear domain and purpose
interface UserProfileData { }          // User domain, Profile entity, Data type
interface ProductCatalogFilters { }    // Product domain, Catalog entity, Filters type
interface OrderSubmissionRequest { }   // Order domain, Submission entity, Request type
interface UserNotificationConfig { }   // User domain, Notification entity, Config type

// ❌ Bad - Generic or unclear
interface ProfileData { }      // Missing domain context
interface Filters { }          // Filters for what?
interface Request { }          // Request for what?
interface Config { }           // Config for what?
```

### Core Type Categories

#### Data Types & Models
```typescript
// ✅ Clear entity types
interface User { }              // Base entity
interface UserProfile { }       // Entity + purpose
interface ProductDetails { }    // Entity + purpose
interface OrderSummary { }      // Entity + purpose

// ❌ Bad - Generic entity types
interface Data { }        // Data for what?
interface Item { }        // Item of what type?
interface Entity { }      // Which entity?
```

#### Props & Component Types
```typescript
// ✅ Component props with clear naming
interface UserProfileCardProps { }       // Domain + Entity + Component + Props
interface ProductCatalogGridProps { }    // Domain + Entity + Component + Props
interface OrderHistoryTableProps { }     // Domain + Entity + Component + Props

// ❌ Bad - Generic props types
interface Props { }           // Props for what component?
interface CardProps { }       // Card for what?
interface FormProps { }       // Form for what?
```

#### API & Request/Response Types
```typescript
// ✅ Clear API types
interface CreateUserRequest { }          // Action + Entity + Request
interface GetProductCatalogResponse { }   // Action + Entity + Response
interface UpdateOrderStatusRequest { }   // Action + Entity + Request

// ❌ Bad - Generic API types
interface ApiRequest { }      // Request for what API?
interface ApiResponse { }     // Response from what?
interface RequestData { }     // Data for what request?
```

#### Configuration & Settings Types
```typescript
// ✅ Clear configuration types
interface UserPreferencesConfig { }
interface ProductTableConfig { }
interface OrderFilterConfig { }
interface UserDashboardConfig { }

// ❌ Bad - Generic config types
interface Config { }          // Config for what?
interface Settings { }        // Settings for what?
interface Options { }         // Options for what?
```

#### State & Hook Result Types
```typescript
// ✅ Clear state and result types
interface UserAuthenticationState {
  currentUser: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  authenticationError: string | null;
}

interface ProductSearchState {
  query: string;
  filters: ProductFilters;
  results: Product[];
  isSearching: boolean;
  searchError: string | null;
}

interface UseUserProfileResult {
  userProfile: User | null;
  isUserProfileLoading: boolean;
  userProfileError: string | null;
  refetchUserProfile: () => Promise<void>;
  updateUserProfile: (data: Partial<User>) => Promise<void>;
}

interface UseOrderSubmissionResult {
  isOrderSubmitting: boolean;
  orderSubmissionError: string | null;
  submitOrder: (data: CreateOrderRequest) => Promise<boolean>;
  resetOrderSubmission: () => void;
}

// ❌ Bad - Generic state types
interface State { }           // State for what?
interface HookResult { }      // Result from what hook?
interface LoadingState { }    // Loading what?
```

#### Utility & Helper Types
```typescript
// ✅ Clear utility types
interface UserValidationResult { }
interface ProductSearchResult { }
interface OrderCalculationResult { }

type UserPermission = 'read' | 'write' | 'admin' | 'delete';
type ProductCategory = 'electronics' | 'clothing' | 'books' | 'home';
type OrderStatus = 'pending' | 'processing' | 'shipped' | 'delivered' | 'cancelled';

// Mapped and conditional types
type UserFormData = Pick<User, 'firstName' | 'lastName' | 'email'>;
type ProductUpdateFields = Partial<Pick<Product, 'name' | 'price' | 'description'>>;
type OrderReadOnlyFields = Omit<Order, 'status' | 'updatedAt'>;

// ❌ Bad - Generic utility types
interface Result { }               // Result of what?
interface ValidationResult { }     // Validation of what?
interface SearchResult { }         // Search for what?
```

#### Event & Handler Types
```typescript
// ✅ Clear event and handler types
interface UserClickEvent { }
interface ProductSelectionEvent { }

type UserFormSubmitHandler = (formData: UserRegistrationForm) => void;
type ProductCardClickHandler = (productId: string) => void;
type OrderCancelHandler = (orderId: string, reason: string) => void;

interface UserModalEventHandlers { }

// ❌ Bad - Generic event types
interface ClickEvent { }      // Click on what?
interface SubmitEvent { }     // Submit what?
type Handler = () => void;    // Handler for what?
```

### Type Naming Best Practices

#### Naming Patterns
```typescript
// ✅ Use descriptive suffixes
interface UserProfileProps { }        // Component props
interface ProductCatalogConfig { }    // Configuration object
interface OrderSubmissionRequest { }  // API request
interface UserAuthenticationResponse { } // API response
interface ProductValidationResult { } // Operation result

// Union types for status and permissions
type UserAccountStatus = 'active' | 'inactive' | 'suspended' | 'pending';
type ProductAvailability = 'in-stock' | 'out-of-stock' | 'discontinued';
type OrderPermission = 'view' | 'edit' | 'cancel' | 'refund';
```

#### Generic Types
```typescript
// ✅ Clear generic constraints with domain context
interface UserApiResponse<T> { }
interface ProductTableColumn<T extends Product> { }
interface OrderFormField<T> { }

// ❌ Bad - Generic without context
interface ApiResponse<T> { }  // API for what domain?
interface TableColumn<T> { }  // Table for what?
interface FormField<T> { }    // Form for what?
```

### Type Anti-Patterns

```typescript
// ❌ Avoid these patterns:

// 1. Generic type names
interface Data { }            // → UserData or ProductData
interface Item { }            // → OrderItem or ProductItem

// 2. Missing domain context
interface Profile { }         // → UserProfile
interface Config { }          // → DatabaseConfig or ApiConfig

// 3. Abbreviations
interface UsrPrf { }          // → UserProfile
interface PrdCat { }          // → ProductCategory

// 4. Inconsistent naming within domain
interface User { }
interface UserDetails { }     // → UserProfile (be consistent)
interface UserInformation { } // → UserData (be consistent)
```

### Domain-Based Organization

```typescript
// ✅ User domain types
interface User { }
interface UserProfile { }
interface UserPreferences { }
type UserPermission = 'read' | 'write' | 'admin';
type UserStatus = 'active' | 'inactive' | 'suspended';

// ✅ Product domain types
interface Product { }
interface ProductDetails { }
interface ProductCatalogFilters { }
type ProductCategory = 'electronics' | 'clothing' | 'books';
type ProductAvailability = 'in-stock' | 'out-of-stock' | 'discontinued';

// ✅ Order domain types
interface Order { }
interface OrderItem { }
interface OrderSummary { }
type OrderStatus = 'pending' | 'shipped' | 'delivered' | 'cancelled';
type OrderPermission = 'view' | 'edit' | 'cancel' | 'refund';
```

**Remember**: Type and interface names should be **self-documenting** and clearly indicate their domain, purpose, and usage context. Use PascalCase consistently and include descriptive suffixes when appropriate.

## Types & Interfaces

Types and interfaces should use **PascalCase** and follow descriptive naming that clearly indicates their domain, purpose, and usage context. All type definitions should be self-documenting and eliminate the need for additional comments.

### Type & Interface Naming Structure

**Format**: `[Domain][Entity][TypePurpose]`

```typescript
// ✅ Good - Clear domain and purpose
interface UserProfileData { }          // User domain, Profile entity, Data type
interface ProductCatalogFilters { }    // Product domain, Catalog entity, Filters type
interface OrderSubmissionRequest { }   // Order domain, Submission entity, Request type
interface UserNotificationConfig { }   // User domain, Notification entity, Config type

// ❌ Bad - Generic or unclear
interface ProfileData { }      // Missing domain context
interface Filters { }          // Filters for what?
interface Request { }          // Request for what?
interface Config { }           // Config for what?
```

### Core Type Categories

#### Data Types & Models
```typescript
// ✅ Clear entity types
interface User { }              // Base entity
interface UserProfile { }       // Entity + purpose
interface ProductDetails { }    // Entity + purpose
interface OrderSummary { }      // Entity + purpose

// ❌ Bad - Generic entity types
interface Data { }        // Data for what?
interface Item { }        // Item of what type?
interface Entity { }      // Which entity?
```

#### Props & Component Types
```typescript
// ✅ Component props with clear naming
interface UserProfileCardProps { }       // Domain + Entity + Component + Props
interface ProductCatalogGridProps { }    // Domain + Entity + Component + Props
interface OrderHistoryTableProps { }     // Domain + Entity + Component + Props

// ❌ Bad - Generic props types
interface Props { }           // Props for what component?
interface CardProps { }       // Card for what?
interface FormProps { }       // Form for what?
```

#### API & Request/Response Types
```typescript
// ✅ Clear API types
interface CreateUserRequest { }          // Action + Entity + Request
interface GetProductCatalogResponse { }   // Action + Entity + Response
interface UpdateOrderStatusRequest { }   // Action + Entity + Request

// ❌ Bad - Generic API types
interface ApiRequest { }      // Request for what API?
interface ApiResponse { }     // Response from what?
interface RequestData { }     // Data for what request?
```

#### Configuration & Settings Types
```typescript
// ✅ Clear configuration types
interface UserPreferencesConfig { }
interface ProductTableConfig { }
interface OrderFilterConfig { }
interface UserDashboardConfig { }

// ❌ Bad - Generic config types
interface Config { }          // Config for what?
interface Settings { }        // Settings for what?
interface Options { }         // Options for what?
```

#### State & Hook Result Types
```typescript
// ✅ Clear state and result types
interface UserAuthenticationState {
  currentUser: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  authenticationError: string | null;
}

interface ProductSearchState {
  query: string;
  filters: ProductFilters;
  results: Product[];
  isSearching: boolean;
  searchError: string | null;
}

interface UseUserProfileResult {
  userProfile: User | null;
  isUserProfileLoading: boolean;
  userProfileError: string | null;
  refetchUserProfile: () => Promise<void>;
  updateUserProfile: (data: Partial<User>) => Promise<void>;
}

interface UseOrderSubmissionResult {
  isOrderSubmitting: boolean;
  orderSubmissionError: string | null;
  submitOrder: (data: CreateOrderRequest) => Promise<boolean>;
  resetOrderSubmission: () => void;
}

// ❌ Bad - Generic state types
interface State { }           // State for what?
interface HookResult { }      // Result from what hook?
interface LoadingState { }    // Loading what?
```

#### Utility & Helper Types
```typescript
// ✅ Clear utility types
interface UserValidationResult { }
interface ProductSearchResult { }
interface OrderCalculationResult { }

type UserPermission = 'read' | 'write' | 'admin' | 'delete';
type ProductCategory = 'electronics' | 'clothing' | 'books' | 'home';
type OrderStatus = 'pending' | 'processing' | 'shipped' | 'delivered' | 'cancelled';

// Mapped and conditional types
type UserFormData = Pick<User, 'firstName' | 'lastName' | 'email'>;
type ProductUpdateFields = Partial<Pick<Product, 'name' | 'price' | 'description'>>;
type OrderReadOnlyFields = Omit<Order, 'status' | 'updatedAt'>;

// ❌ Bad - Generic utility types
interface Result { }               // Result of what?
interface ValidationResult { }     // Validation of what?
interface SearchResult { }         // Search for what?
```

#### Event & Handler Types
```typescript
// ✅ Clear event and handler types
interface UserClickEvent { }
interface ProductSelectionEvent { }

type UserFormSubmitHandler = (formData: UserRegistrationForm) => void;
type ProductCardClickHandler = (productId: string) => void;
type OrderCancelHandler = (orderId: string, reason: string) => void;

interface UserModalEventHandlers { }

// ❌ Bad - Generic event types
interface ClickEvent { }      // Click on what?
interface SubmitEvent { }     // Submit what?
type Handler = () => void;    // Handler for what?
```

### Type Naming Best Practices

#### Naming Patterns
```typescript
// ✅ Use descriptive suffixes
interface UserProfileProps { }        // Component props
interface ProductCatalogConfig { }    // Configuration object
interface OrderSubmissionRequest { }  // API request
interface UserAuthenticationResponse { } // API response
interface ProductValidationResult { } // Operation result

// Union types for status and permissions
type UserAccountStatus = 'active' | 'inactive' | 'suspended' | 'pending';
type ProductAvailability = 'in-stock' | 'out-of-stock' | 'discontinued';
type OrderPermission = 'view' | 'edit' | 'cancel' | 'refund';
```

#### Generic Types
```typescript
// ✅ Clear generic constraints with domain context
interface UserApiResponse<T> { }
interface ProductTableColumn<T extends Product> { }
interface OrderFormField<T> { }

// ❌ Bad - Generic without context
interface ApiResponse<T> { }  // API for what domain?
interface TableColumn<T> { }  // Table for what?
interface FormField<T> { }    // Form for what?
```

### Type Anti-Patterns

```typescript
// ❌ Avoid these patterns:

// 1. Generic type names
interface Data { }            // → UserData or ProductData
interface Item { }            // → OrderItem or ProductItem

// 2. Missing domain context
interface Profile { }         // → UserProfile
interface Config { }          // → DatabaseConfig or ApiConfig

// 3. Abbreviations
interface UsrPrf { }          // → UserProfile
interface PrdCat { }          // → ProductCategory

// 4. Inconsistent naming within domain
interface User { }
interface UserDetails { }     // → UserProfile (be consistent)
interface UserInformation { } // → UserData (be consistent)
```

### Domain-Based Organization

```typescript
// ✅ User domain types
interface User { }
interface UserProfile { }
interface UserPreferences { }
type UserPermission = 'read' | 'write' | 'admin';
type UserStatus = 'active' | 'inactive' | 'suspended';

// ✅ Product domain types
interface Product { }
interface ProductDetails { }
interface ProductCatalogFilters { }
type ProductCategory = 'electronics' | 'clothing' | 'books';
type ProductAvailability = 'in-stock' | 'out-of-stock' | 'discontinued';

// ✅ Order domain types
interface Order { }
interface OrderItem { }
interface OrderSummary { }
type OrderStatus = 'pending' | 'shipped' | 'delivered' | 'cancelled';
type OrderPermission = 'view' | 'edit' | 'cancel' | 'refund';
```

**Remember**: Type and interface names should be **self-documenting** and clearly indicate their domain, purpose, and usage context. Use PascalCase consistently and include descriptive suffixes when appropriate.

## Constants & Enums

Constants should use **SCREAMING_SNAKE_CASE** and enums should use **PascalCase** with descriptive naming that clearly indicates their domain and purpose.

### Constants Naming Structure

**Format**: `[DOMAIN]_[ENTITY]_[PURPOSE]`

```typescript
// ✅ Good - Clear domain and purpose
const USER_SESSION_TIMEOUT_MS = 30 * 60 * 1000;     // User domain, Session entity, Timeout purpose
const PRODUCT_SEARCH_DEBOUNCE_MS = 300;             // Product domain, Search entity, Debounce purpose
const ORDER_MAX_ITEMS_COUNT = 100;                  // Order domain, Max entity, Items count purpose
const API_REQUEST_RETRY_ATTEMPTS = 3;               // API domain, Request entity, Retry attempts purpose

// ❌ Bad - Generic or unclear
const TIMEOUT = 30000;           // Timeout for what?
const MAX_ITEMS = 100;          // Max items for what?
const RETRY = 3;                // Retry what?
```

### Core Constant Categories

1. **Constants**: Use `SCREAMING_SNAKE_CASE` with domain context
2. **Enums**: Use `PascalCase` with descriptive names and string values
3. **Organization**: Group by domain (user, product, order, etc.)
4. **File Structure**: Separate constants and enums into domain-specific files
5. **String Literals**: Use for simple, stable values instead of enums
6. **Values**: Use descriptive enum values that match the key meaning
7. **Consistency**: Maintain consistent naming patterns within domains
8. **Documentation**: Make names self-documenting to avoid comments

#### UI & Display Constants
```typescript
// ✅ UI constants with clear purpose
const USER_TABLE_DEFAULT_PAGE_SIZE = 20;
const USER_TABLE_MAX_PAGE_SIZE = 100;
const PRODUCT_GRID_ITEMS_PER_ROW = 4;
const PRODUCT_IMAGE_MAX_SIZE_MB = 5;

const USER_NOTIFICATION_DISPLAY_DURATION_MS = 5000;
const USER_MODAL_ANIMATION_DURATION_MS = 300;
const SEARCH_INPUT_DEBOUNCE_MS = 500;
const TOOLTIP_SHOW_DELAY_MS = 1000;

// Pagination constants
const DEFAULT_PAGE_SIZE = 20;
const MAX_PAGE_SIZE = 100;
const PAGINATION_BOUNDARY_RANGE = 1;
const PAGINATION_SIBLING_COUNT = 1;

// ❌ Bad - Generic UI constants
const PAGE_SIZE = 20;           // Page size for what?
const DURATION = 5000;          // Duration of what?
const DELAY = 1000;             // Delay for what?
```

#### Validation & Limits Constants
```typescript
// ✅ Validation constants with clear domain
const USER_PASSWORD_MIN_LENGTH = 8;
const USER_PASSWORD_MAX_LENGTH = 128;
const USER_EMAIL_MAX_LENGTH = 254;
const USER_NAME_MAX_LENGTH = 100;

const PRODUCT_NAME_MIN_LENGTH = 3;
const PRODUCT_NAME_MAX_LENGTH = 200;
const PRODUCT_DESCRIPTION_MAX_LENGTH = 5000;
const PRODUCT_PRICE_MIN_VALUE = 0.01;
const PRODUCT_PRICE_MAX_VALUE = 999999.99;

const ORDER_ITEMS_MIN_COUNT = 1;
const ORDER_ITEMS_MAX_COUNT = 100;
const ORDER_TOTAL_MIN_VALUE = 1.00;
const ORDER_NOTES_MAX_LENGTH = 500;

const FILE_UPLOAD_MAX_SIZE_MB = 10;
const FILE_UPLOAD_ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/gif'] as const;
const IMAGE_MAX_DIMENSIONS_PX = 2048;

// ❌ Bad - Generic validation constants
const MIN_LENGTH = 8;           // Min length for what?
const MAX_SIZE = 10;            // Max size for what?
const ALLOWED_TYPES = ['jpg'];  // Types for what?
```

#### Business Logic Constants
```typescript
// ✅ Business constants with clear domain
const USER_FREE_TRIAL_DURATION_DAYS = 30;
const USER_SESSION_REFRESH_INTERVAL_MS = 15 * 60 * 1000; // 15 minutes
const USER_PASSWORD_RESET_TOKEN_EXPIRY_HOURS = 24;
const USER_LOGIN_MAX_ATTEMPTS = 5;
const USER_ACCOUNT_LOCKOUT_DURATION_MINUTES = 30;

const PRODUCT_DISCOUNT_MAX_PERCENTAGE = 90;
const PRODUCT_STOCK_LOW_THRESHOLD = 10;
const PRODUCT_REVIEW_MIN_RATING = 1;
const PRODUCT_REVIEW_MAX_RATING = 5;

const ORDER_PROCESSING_TIMEOUT_HOURS = 24;
const ORDER_CANCELLATION_WINDOW_HOURS = 2;
const ORDER_REFUND_PROCESSING_DAYS = 5;
const ORDER_SHIPPING_CUTOFF_HOUR = 15; // 3 PM

// Tax and financial constants
const DEFAULT_TAX_RATE = 0.08;           // 8%
const SHIPPING_FREE_THRESHOLD = 50.00;
const CURRENCY_DECIMAL_PLACES = 2;

// ❌ Bad - Generic business constants
const TRIAL_DAYS = 30;          // Trial for what?
const MAX_ATTEMPTS = 5;         // Attempts for what?
const THRESHOLD = 10;           // Threshold for what?
```

#### Feature Flags & Configuration
```typescript
// ✅ Feature flag constants with clear purpose
const FEATURE_USER_PROFILE_V2_ENABLED = true;
const FEATURE_PRODUCT_RECOMMENDATIONS_ENABLED = false;
const FEATURE_ORDER_TRACKING_ENHANCED_ENABLED = true;
const FEATURE_DARK_MODE_ENABLED = true;

const ANALYTICS_ENABLED = true;
const DEBUG_MODE_ENABLED = process.env.NODE_ENV === 'development';
const CACHE_ENABLED = true;
const LOGGING_LEVEL = 'info' as const;

// Environment-specific constants
const IS_PRODUCTION = process.env.NODE_ENV === 'production';
const IS_DEVELOPMENT = process.env.NODE_ENV === 'development';
const IS_TEST_ENVIRONMENT = process.env.NODE_ENV === 'test';

// ❌ Bad - Generic feature flags
const ENABLED = true;           // What is enabled?
const DEBUG = false;            // Debug what?
const FEATURE_FLAG = true;      // Which feature?
```

### Enums Naming Structure

**Format**: PascalCase with descriptive names

```typescript
// ✅ Good - Clear enum purpose and values
enum UserAccountStatus {
  Active = 'active',
  Inactive = 'inactive',
  Suspended = 'suspended',
  PendingVerification = 'pending_verification',
  Deleted = 'deleted'
}

enum ProductAvailabilityStatus {
  InStock = 'in_stock',
  OutOfStock = 'out_of_stock',
  LimitedStock = 'limited_stock',
  Discontinued = 'discontinued',
  PreOrder = 'pre_order'
}

// ❌ Bad - Generic or unclear enums
enum Status {             // Status of what?
  Active = 'active',
  Inactive = 'inactive'
}

enum Type {               // Type of what?
  A = 'a',
  B = 'b'
}
```

### Domain-Based Enum Categories

#### User Domain Enums
```typescript
// ✅ User-related enums
enum UserRole {
  Guest = 'guest',
  Customer = 'customer',
  Premium = 'premium',
  Admin = 'admin',
  SuperAdmin = 'super_admin'
}

enum UserPreferenceTheme {
  Light = 'light',
  Dark = 'dark',
  Auto = 'auto'
}
```

#### Product Domain Enums
```typescript
// ✅ Product-related enums
enum ProductCategory {
  Electronics = 'electronics',
  Clothing = 'clothing',
  Books = 'books',
  Home = 'home',
  Sports = 'sports',
  Beauty = 'beauty'
}

enum ProductCondition {
  New = 'new',
  Used = 'used',
  Refurbished = 'refurbished',
  OpenBox = 'open_box'
}
```

#### Order Domain Enums
```typescript
// ✅ Order-related enums
enum OrderPaymentMethod {
  CreditCard = 'credit_card',
  DebitCard = 'debit_card',
  PayPal = 'paypal',
  ApplePay = 'apple_pay',
  GooglePay = 'google_pay',
  BankTransfer = 'bank_transfer'
}

enum OrderPriority {
  Low = 'low',
  Normal = 'normal',
  High = 'high',
  Urgent = 'urgent'
}
```

#### System & Technical Enums
```typescript
// ✅ System-related enums
enum ApiErrorCode {
  BadRequest = 'BAD_REQUEST',
  Unauthorized = 'UNAUTHORIZED',
  Forbidden = 'FORBIDDEN',
  NotFound = 'NOT_FOUND',
  InternalServerError = 'INTERNAL_SERVER_ERROR',
  ServiceUnavailable = 'SERVICE_UNAVAILABLE'
}

enum LogLevel {
  Error = 'error',
  Warn = 'warn',
  Info = 'info',
  Debug = 'debug',
  Trace = 'trace'
}
```

### String Literal Types vs Enums

```typescript
// ✅ Use string literal types for simple, stable values
type UserGender = 'male' | 'female' | 'other' | 'prefer_not_to_say';
type ProductSize = 'xs' | 's' | 'm' | 'l' | 'xl' | 'xxl';
type OrderCurrency = 'USD' | 'EUR' | 'GBP' | 'CAD';
type FileFormat = 'pdf' | 'docx' | 'txt' | 'csv' | 'xlsx';

// ✅ Use enums for complex values that might change
enum UserNotificationPreference {
  EmailOnly = 'email_only',
  SMSOnly = 'sms_only',
  EmailAndSMS = 'email_and_sms',
  PushOnly = 'push_only',
  AllChannels = 'all_channels',
  None = 'none'
}
```

### File Organization Patterns

#### Constants Files
```typescript
// ✅ Domain-specific constant files
// user.constants.ts
export const USER_SESSION_TIMEOUT_MS = 30 * 60 * 1000;
export const USER_PASSWORD_MIN_LENGTH = 8;
export const USER_LOGIN_MAX_ATTEMPTS = 5;

// product.constants.ts  
export const PRODUCT_SEARCH_DEBOUNCE_MS = 300;
export const PRODUCT_IMAGE_MAX_SIZE_MB = 5;
export const PRODUCT_NAME_MAX_LENGTH = 200;

// api.constants.ts
export const API_REQUEST_TIMEOUT_MS = 10000;
export const API_MAX_RETRY_ATTEMPTS = 3;

// ui.constants.ts
export const DEFAULT_PAGE_SIZE = 20;
export const MODAL_ANIMATION_DURATION_MS = 300;
```

#### Enum Files
```typescript
// ✅ Domain-specific enum files
// user.types.ts
export enum UserRole { }
export enum UserAccountStatus { }
export enum UserNotificationType { }

// product.types.ts
export enum ProductCategory { }
export enum ProductAvailabilityStatus { }
export enum ProductSortOption { }

// order.types.ts
export enum OrderProcessingStatus { }
export enum OrderPaymentMethod { }
export enum OrderShippingMethod { }
```

### Constant & Enum Anti-Patterns

```typescript
// ❌ Avoid these patterns:

// 1. Generic constant names
const TIMEOUT = 5000;         // → USER_SESSION_TIMEOUT_MS
const MAX = 100;              // → ORDER_ITEMS_MAX_COUNT
const URL = 'api.com';        // → USER_API_BASE_URL

// 2. Inconsistent casing for constants
const userTimeout = 5000;     // → USER_TIMEOUT_MS
const User_Max_Items = 100;   // → USER_MAX_ITEMS

// 3. Generic enum names
enum Status { }               // → UserAccountStatus
enum Type { }                 // → ProductCategory
enum Mode { }                 // → UserPreferenceTheme

// 4. Inconsistent enum value formats
enum OrderStatus {
  pending = 'PENDING',        // Mixed case styles
  Processing = 'processing',
  SHIPPED = 'Shipped'
}

// 5. Magic numbers/strings instead of constants
setTimeout(callback, 5000);   // → USER_SESSION_TIMEOUT_MS
if (items.length > 100) { }   // → ORDER_ITEMS_MAX_COUNT
if (status === 'active') { }  // → UserAccountStatus.Active

// 6. Missing domain context
const API_ENDPOINTS = { };    // → USER_API_ENDPOINTS
const VALIDATION_RULES = { }; // → USER_VALIDATION_RULES
const DEFAULT_CONFIG = { };   // → USER_DEFAULT_CONFIG
```

**Remember**: Constants and enums should be **self-documenting** and clearly indicate their domain and purpose. Use descriptive names that eliminate the need for mental mapping or additional documentation.

## Testing

Test files and functions should follow clear naming conventions that indicate what is being tested, the expected behavior, and the test context using descriptive names that make test failures easy to understand. Use the **AAA pattern** (Arrange, Act, Assert) for test structure and `test` blocks for better readability.

### Core test Categories

1. **Use `test` blocks**: Use `test` instead of `it` for consistency and clarity
2. **AAA Pattern**: Structure all tests with clear Arrange, Act, Assert sections
3. **Test Files**: Match source file names with `.test.[tsx|ts]` suffix
4. **Test Suites**: Use descriptive `describe` blocks indicating component/function being tested
5. **Test Cases**: Start with "should" and describe expected behavior and conditions
6. **Mock Objects**: Prefix with `mock` and include domain context, organize by AAA sections
7. **Helper Functions**: Clearly indicate their AAA purpose (`arrangeX`, `actX`, `assertX`)
8. **Organization**: Group tests by domain and feature area
9. **Integration Tests**: Describe the full flow with clear AAA structure
10. **Consistency**: Maintain consistent naming patterns and AAA structure across all tests

### Test File Naming Structure

**Format**: `[ComponentName].test.[tsx|ts]` or `[functionName].test.[tsx|ts]`

```typescript
// ✅ Good - Clear test file names matching source files
UserProfileCard.test.tsx        // Tests for UserProfileCard component
ProductCatalogGrid.test.tsx     // Tests for ProductCatalogGrid component
useUserAuthentication.test.ts   // Tests for useUserAuthentication hook
calculateOrderTax.test.ts       // Tests for calculateOrderTax function

// ❌ Bad - Generic or unclear test file names
test.tsx                        // Test for what?
component.test.tsx              // Which component?
utils.test.ts                   // Which utilities?
```

### Test Suite Naming (describe blocks)

**Format**: `[ComponentName|FunctionName]`

```typescript
// ✅ Good - Clear test suite organization
describe('UserProfileCard', () => {
  describe('when user is authenticated', () => {
    describe('and user can edit profile', () => {
      // ... test cases using AAA pattern
    });
  });
});

describe('useUserAuthentication', () => {
  describe('when user logs in successfully', () => {
    // ... success scenarios
  });
  
  describe('when user login fails', () => {
    // ... error scenarios
  });
});

// ❌ Bad - Generic or unclear suite names
describe('Component', () => { });         // Which component?
describe('Tests', () => { });             // Tests for what?
```

### Test Case Naming (test blocks with AAA pattern)

**Format**: `should [expected behavior] when [condition/context]`

```typescript
// ✅ Good - Clear test case descriptions using test blocks and AAA pattern
describe('UserProfileCard', () => {
  test('should display user name and email when user data is provided', () => {
    // Arrange
    const mockUserData = {
      id: 'user-123',
      firstName: 'John',
      lastName: 'Doe',
      email: 'john@example.com'
    };
    
    // Act
    render(<UserProfileCard user={mockUserData} onEditClick={mockOnEditClick} />);
    
    // Assert
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });
  
  test('should show edit button when user has edit permissions', () => {
    // Arrange
    const mockUserWithEditPermissions = { ...mockUserData, canEdit: true };
    
    // Act
    render(<UserProfileCard user={mockUserWithEditPermissions} onEditClick={mockOnEditClick} />);
    
    // Assert
    expect(screen.getByRole('button', { name: /edit profile/i })).toBeInTheDocument();
  });
  
  test('should call onEditClick when edit button is clicked', () => {
    // Arrange
    const mockUserData = { ...baseUserData, canEdit: true };
    const mockOnEditClick = jest.fn();
    
    // Act
    render(<UserProfileCard user={mockUserData} onEditClick={mockOnEditClick} />);
    fireEvent.click(screen.getByRole('button', { name: /edit profile/i }));
    
    // Assert
    expect(mockOnEditClick).toHaveBeenCalledWith(mockUserData.id);
  });
});

describe('validateUserEmail', () => {
  test('should return valid result when email format is correct', () => {
    // Arrange
    const validEmail = 'user@example.com';
    
    // Act
    const result = validateUserEmail(validEmail);
    
    // Assert
    expect(result.isValid).toBe(true);
    expect(result.errorMessage).toBeNull();
  });
  
  test('should return invalid result when email is missing @ symbol', () => {
    // Arrange
    const invalidEmail = 'userexample.com';
    
    // Act
    const result = validateUserEmail(invalidEmail);
    
    // Assert
    expect(result.isValid).toBe(false);
    expect(result.errorMessage).toBe('Please enter a valid email address');
  });
});

// ❌ Bad - Unclear or generic test descriptions
test('works', () => { });                           // What works?
test('renders correctly', () => { });               // Renders what correctly?
test('handles click', () => { });                   // Handles click how?
```

### Mock Object Naming with AAA Pattern

**Format**: `mock[HighContext][LowContext]` organized by AAA sections

```typescript
// ✅ Good - Clear mock object names organized by AAA pattern
describe('UserProfileService', () => {
  test('should update user profile when valid data is provided', async () => {
    // Arrange - Setup mocks and test data
    const mockUserProfileData = {
      id: 'user-123',
      firstName: 'John',
      lastName: 'Doe',
      // ... existing user properties
    };
    
    const mockUpdatedProfileData = {
      ...mockUserProfileData,
      firstName: 'Jane'
    };
    
    const mockHttpClient = {
      patch: jest.fn().mockResolvedValue({ success: true, data: mockUpdatedProfileData })
    };
    
    // Act
    const userProfileService = new UserProfileService(mockHttpClient);
    const result = await userProfileService.updateUserProfile(mockUserProfileData.id, { firstName: 'Jane' });
    
    // Assert
    expect(mockHttpClient.patch).toHaveBeenCalledWith(`/users/${mockUserProfileData.id}`, { firstName: 'Jane' });
    expect(result).toEqual(mockUpdatedProfileData);
  });
});

// ❌ Bad - Generic mock names
const mockData = { };                     // Mock data for what?
const mockService = { };                  // Mock service for what?
const mockFunction = jest.fn();           // Mock function for what?
```

### Test Helper Function Naming with AAA Pattern

**Format**: `[arrange|act|assert][TestScenario]Helper`

```typescript
// ✅ Good - AAA-organized test helper functions
const arrangeUserProfileCardTestWithValidData = () => {
  return {
    mockUserData: {
      id: 'user-123',
      firstName: 'John',
      lastName: 'Doe',
      // ... existing user properties
    },
    mockHandleEditClick: jest.fn(),
    // ... existing mock handlers
  };
};

const arrangeUserProfileCardTestWithLoadingState = () => {
  return {
    mockUserData: null,
    isLoading: true,
    mockHandleEditClick: jest.fn()
  };
};

const actUserProfileCardRender = (props: UserProfileCardProps) => {
  return render(<UserProfileCard {...props} />);
};

const assertUserProfileCardDisplaysCorrectly = (expectedUserData: UserProfile) => {
  expect(screen.getByText(`${expectedUserData.firstName} ${expectedUserData.lastName}`)).toBeInTheDocument();
  expect(screen.getByText(expectedUserData.email)).toBeInTheDocument();
  // ... existing assertions
};

// Usage in tests
describe('UserProfileCard', () => {
  test('should display user information when valid data is provided', () => {
    // Arrange
    const { mockUserData, mockHandleEditClick } = arrangeUserProfileCardTestWithValidData();
    
    // Act
    actUserProfileCardRender({ user: mockUserData, onEditClick: mockHandleEditClick });
    
    // Assert
    assertUserProfileCardDisplaysCorrectly(mockUserData);
  });
});

// ❌ Bad - Generic helper function names
const setup = () => { };                  // Setup what?
const render = () => { };                 // Render what?
const check = () => { };                  // Check what?
```

### Integration Test Naming with AAA Pattern

```typescript
// ✅ Good - Clear integration test naming with AAA structure
describe('User Authentication Integration', () => {
  test('should authenticate user and redirect to dashboard when login credentials are valid', async () => {
    // Arrange
    const mockValidCredentials = { email: 'user@example.com', password: 'correctPassword123' };
    const mockAuthResponse = { user: { id: '123', email: 'user@example.com' }, token: 'valid-jwt-token' };
    
    jest.spyOn(authApi, 'login').mockResolvedValue(mockAuthResponse);
    jest.spyOn(router, 'navigate').mockImplementation(mockNavigate);
    
    // Act
    render(<LoginForm />);
    fireEvent.change(screen.getByLabelText(/email/i), { target: { value: mockValidCredentials.email } });
    fireEvent.change(screen.getByLabelText(/password/i), { target: { value: mockValidCredentials.password } });
    fireEvent.click(screen.getByRole('button', { name: /sign in/i }));
    
    // Assert
    await waitFor(() => {
      expect(authApi.login).toHaveBeenCalledWith(mockValidCredentials);
      expect(mockNavigate).toHaveBeenCalledWith('/dashboard');
    });
    expect(localStorage.getItem('authToken')).toBe('valid-jwt-token');
  });
  
  test('should display error message and remain on login page when credentials are invalid', async () => {
    // Arrange
    const mockInvalidCredentials = { email: 'user@example.com', password: 'wrongPassword' };
    jest.spyOn(authApi, 'login').mockRejectedValue(new Error('Invalid credentials'));
    
    // Act
    render(<LoginForm />);
    // ... existing form interaction
    fireEvent.click(screen.getByRole('button', { name: /sign in/i }));
    
    // Assert
    await waitFor(() => {
      expect(screen.getByRole('alert')).toHaveTextContent('Invalid credentials');
    });
    expect(mockNavigate).not.toHaveBeenCalled();
  });
});
```

### Test Anti-Patterns

```typescript
// ❌ Avoid these testing patterns:

// 1. Using 'it' instead of 'test'
it('works', () => { });                    // → Use 'test' for consistency

// 2. Missing AAA pattern structure
test('should update user profile', () => {
  const user = { name: 'John' };           // No clear Arrange section
  updateProfile(user);                     // Act mixed with Arrange
  expect(result).toBeTruthy();             // Assert without clear expectation
});

// 3. Generic test descriptions
test('works', () => { });
test('renders', () => { });
test('handles input', () => { });

// 4. Generic mock names without context
test('should call function', () => {
  const mock = jest.fn();                  // Mock what function?
  const data = { };                        // What kind of data?
  // ... existing test logic
});
```



**Remember**: Test names should be **self-documenting** using the **AAA pattern** structure. Each test should have clear Arrange, Act, and Assert sections with descriptive comments. Use `test` blocks for better readability and consistency across the codebase.

## Types & Interfaces

Types and interfaces should use **PascalCase** and follow descriptive naming that clearly indicates their domain, purpose, and usage context. All type definitions should be self-documenting and eliminate the need for additional comments.

### Type & Interface Naming Structure

**Format**: `[Domain][Entity][TypePurpose]`

```typescript
// ✅ Good - Clear domain and purpose
interface UserProfileData { }          // User domain, Profile entity, Data type
interface ProductCatalogFilters { }    // Product domain, Catalog entity, Filters type
interface OrderSubmissionRequest { }   // Order domain, Submission entity, Request type
interface UserNotificationConfig { }   // User domain, Notification entity, Config type

// ❌ Bad - Generic or unclear
interface ProfileData { }      // Missing domain context
interface Filters { }          // Filters for what?
interface Request { }          // Request for what?
interface Config { }           // Config for what?
```

### Core Type Categories

#### Data Types & Models
```typescript
// ✅ Clear entity types
interface User { }              // Base entity
interface UserProfile { }       // Entity + purpose
interface ProductDetails { }    // Entity + purpose
interface OrderSummary { }      // Entity + purpose

// ❌ Bad - Generic entity types
interface Data { }        // Data for what?
interface Item { }        // Item of what type?
interface Entity { }      // Which entity?
```

#### Props & Component Types
```typescript
// ✅ Component props with clear naming
interface UserProfileCardProps { }       // Domain + Entity + Component + Props
interface ProductCatalogGridProps { }    // Domain + Entity + Component + Props
interface OrderHistoryTableProps { }     // Domain + Entity + Component + Props

// ❌ Bad - Generic props types
interface Props { }           // Props for what component?
interface CardProps { }       // Card for what?
interface FormProps { }       // Form for what?
```

#### API & Request/Response Types
```typescript
// ✅ Clear API types
interface CreateUserRequest { }          // Action + Entity + Request
interface GetProductCatalogResponse { }   // Action + Entity + Response
interface UpdateOrderStatusRequest { }   // Action + Entity + Request

// ❌ Bad - Generic API types
interface ApiRequest { }      // Request for what API?
interface ApiResponse { }     // Response from what?
interface RequestData { }     // Data for what request?
```

#### Configuration & Settings Types
```typescript
// ✅ Clear configuration types
interface UserPreferencesConfig { }
interface ProductTableConfig { }
interface OrderFilterConfig { }
interface UserDashboardConfig { }

// ❌ Bad - Generic config types
interface Config { }          // Config for what?
interface Settings { }        // Settings for what?
interface Options { }         // Options for what?
```

#### State & Hook Result Types
```typescript
// ✅ Clear state and result types
interface UserAuthenticationState {
  currentUser: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  authenticationError: string | null;
}

interface ProductSearchState {
  query: string;
  filters: ProductFilters;
  results: Product[];
  isSearching: boolean;
  searchError: string | null;
}

interface UseUserProfileResult {
  userProfile: User | null;
  isUserProfileLoading: boolean;
  userProfileError: string | null;
  refetchUserProfile: () => Promise<void>;
  updateUserProfile: (data: Partial<User>) => Promise<void>;
}

interface UseOrderSubmissionResult {
  isOrderSubmitting: boolean;
  orderSubmissionError: string | null;
  submitOrder: (data: CreateOrderRequest) => Promise<boolean>;
  resetOrderSubmission: () => void;
}

// ❌ Bad - Generic state types
interface State { }           // State for what?
interface HookResult { }      // Result from what hook?
interface LoadingState { }    // Loading what?
```

#### Utility & Helper Types
```typescript
// ✅ Clear utility types
interface UserValidationResult { }
interface ProductSearchResult { }
interface OrderCalculationResult { }

type UserPermission = 'read' | 'write' | 'admin' | 'delete';
type ProductCategory = 'electronics' | 'clothing' | 'books' | 'home';
type OrderStatus = 'pending' | 'processing' | 'shipped' | 'delivered' | 'cancelled';

// Mapped and conditional types
type UserFormData = Pick<User, 'firstName' | 'lastName' | 'email'>;
type ProductUpdateFields = Partial<Pick<Product, 'name' | 'price' | 'description'>>;
type OrderReadOnlyFields = Omit<Order, 'status' | 'updatedAt'>;

// ❌ Bad - Generic utility types
interface Result { }               // Result of what?
interface ValidationResult { }     // Validation of what?
interface SearchResult { }         // Search for what?
```

#### Event & Handler Types
```typescript
// ✅ Clear event and handler types
interface UserClickEvent { }
interface ProductSelectionEvent { }

type UserFormSubmitHandler = (formData: UserRegistrationForm) => void;
type ProductCardClickHandler = (productId: string) => void;
type OrderCancelHandler = (orderId: string, reason: string) => void;

interface UserModalEventHandlers { }

// ❌ Bad - Generic event types
interface ClickEvent { }      // Click on what?
interface SubmitEvent { }     // Submit what?
type Handler = () => void;    // Handler for what?
```

### Type Naming Best Practices

#### Naming Patterns
```typescript
// ✅ Use descriptive suffixes
interface UserProfileProps { }        // Component props
interface ProductCatalogConfig { }    // Configuration object
interface OrderSubmissionRequest { }  // API request
interface UserAuthenticationResponse { } // API response
interface ProductValidationResult { } // Operation result

// Union types for status and permissions
type UserAccountStatus = 'active' | 'inactive' | 'suspended' | 'pending';
type ProductAvailability = 'in-stock' | 'out-of-stock' | 'discontinued';
type OrderPermission = 'view' | 'edit' | 'cancel' | 'refund';
```

#### Generic Types
```typescript
// ✅ Clear generic constraints with domain context
interface UserApiResponse<T> { }
interface ProductTableColumn<T extends Product> { }
interface OrderFormField<T> { }

// ❌ Bad - Generic without context
interface ApiResponse<T> { }  // API for what domain?
interface TableColumn<T> { }  // Table for what?
interface FormField<T> { }    // Form for what?
```

### Type Anti-Patterns

```typescript
// ❌ Avoid these patterns:

// 1. Generic type names
interface Data { }            // → UserData or ProductData
interface Item { }            // → OrderItem or ProductItem

// 2. Missing domain context
interface Profile { }         // → UserProfile
interface Config { }          // → DatabaseConfig or ApiConfig

// 3. Abbreviations
interface UsrPrf { }          // → UserProfile
interface PrdCat { }          // → ProductCategory

// 4. Inconsistent naming within domain
interface User { }
interface UserDetails { }     // → UserProfile (be consistent)
interface UserInformation { } // → UserData (be consistent)
```

### Domain-Based Organization

```typescript
// ✅ User domain types
interface User { }
interface UserProfile { }
interface UserPreferences { }
type UserPermission = 'read' | 'write' | 'admin';
type UserStatus = 'active' | 'inactive' | 'suspended';

// ✅ Product domain types
interface Product { }
interface ProductDetails { }
interface ProductCatalogFilters { }
type ProductCategory = 'electronics' | 'clothing' | 'books';
type ProductAvailability = 'in-stock' | 'out-of-stock' | 'discontinued';

// ✅ Order domain types
interface Order { }
interface OrderItem { }
interface OrderSummary { }
type OrderStatus = 'pending' | 'shipped' | 'delivered' | 'cancelled';
type OrderPermission = 'view' | 'edit' | 'cancel' | 'refund';
```

**Remember**: Type and interface names should be **self-documenting** and clearly indicate their domain, purpose, and usage context. Use PascalCase consistently and include descriptive suffixes when appropriate.

## Constants & Enums

Constants should use **SCREAMING_SNAKE_CASE** and enums should use **PascalCase** with descriptive naming that clearly indicates their domain and purpose.

### Constants Naming Structure

**Format**: `[DOMAIN]_[ENTITY]_[PURPOSE]`

```typescript
// ✅ Good - Clear domain and purpose
const USER_SESSION_TIMEOUT_MS = 30 * 60 * 1000;     // User domain, Session entity, Timeout purpose
const PRODUCT_SEARCH_DEBOUNCE_MS = 300;             // Product domain, Search entity, Debounce purpose
const ORDER_MAX_ITEMS_COUNT = 100;                  // Order domain, Max entity, Items count purpose
const API_REQUEST_RETRY_ATTEMPTS = 3;               // API domain, Request entity, Retry attempts purpose

// ❌ Bad - Generic or unclear
const TIMEOUT = 30000;           // Timeout for what?
const MAX_ITEMS = 100;          // Max items for what?
const RETRY = 3;                // Retry what?
```

### Core Constant Categories

1. **Constants**: Use `SCREAMING_SNAKE_CASE` with domain context
2. **Enums**: Use `PascalCase` with descriptive names and string values
3. **Organization**: Group by domain (user, product, order, etc.)
4. **File Structure**: Separate constants and enums into domain-specific files
5. **String Literals**: Use for simple, stable values instead of enums
6. **Values**: Use descriptive enum values that match the key meaning
7. **Consistency**: Maintain consistent naming patterns within domains
8. **Documentation**: Make names self-documenting to avoid comments

#### UI & Display Constants
```typescript
// ✅ UI constants with clear purpose
const USER_TABLE_DEFAULT_PAGE_SIZE = 20;
const USER_TABLE_MAX_PAGE_SIZE = 100;
const PRODUCT_GRID_ITEMS_PER_ROW = 4;
const PRODUCT_IMAGE_MAX_SIZE_MB = 5;

const USER_NOTIFICATION_DISPLAY_DURATION_MS = 5000;
const USER_MODAL_ANIMATION_DURATION_MS = 300;
const SEARCH_INPUT_DEBOUNCE_MS = 500;
const TOOLTIP_SHOW_DELAY_MS = 1000;

// Pagination constants
const DEFAULT_PAGE_SIZE = 20;
const MAX_PAGE_SIZE = 100;
const PAGINATION_BOUNDARY_RANGE = 1;
const PAGINATION_SIBLING_COUNT = 1;

// ❌ Bad - Generic UI constants
const PAGE_SIZE = 20;           // Page size for what?
const DURATION = 5000;          // Duration of what?
const DELAY = 1000;             // Delay for what?
```

#### Validation & Limits Constants
```typescript
// ✅ Validation constants with clear domain
const USER_PASSWORD_MIN_LENGTH = 8;
const USER_PASSWORD_MAX_LENGTH = 128;
const USER_EMAIL_MAX_LENGTH = 254;
const USER_NAME_MAX_LENGTH = 100;

const PRODUCT_NAME_MIN_LENGTH = 3;
const PRODUCT_NAME_MAX_LENGTH = 200;
const PRODUCT_DESCRIPTION_MAX_LENGTH = 5000;
const PRODUCT_PRICE_MIN_VALUE = 0.01;
const PRODUCT_PRICE_MAX_VALUE = 999999.99;

const ORDER_ITEMS_MIN_COUNT = 1;
const ORDER_ITEMS_MAX_COUNT = 100;
const ORDER_TOTAL_MIN_VALUE = 1.00;
const ORDER_NOTES_MAX_LENGTH = 500;

const FILE_UPLOAD_MAX_SIZE_MB = 10;
const FILE_UPLOAD_ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/gif'] as const;
const IMAGE_MAX_DIMENSIONS_PX = 2048;

// ❌ Bad - Generic validation constants
const MIN_LENGTH = 8;           // Min length for what?
const MAX_SIZE = 10;            // Max size for what?
const ALLOWED_TYPES = ['jpg'];  // Types for what?
```

#### Business Logic Constants
```typescript
// ✅ Business constants with clear domain
const USER_FREE_TRIAL_DURATION_DAYS = 30;
const USER_SESSION_REFRESH_INTERVAL_MS = 15 * 60 * 1000; // 15 minutes
const USER_PASSWORD_RESET_TOKEN_EXPIRY_HOURS = 24;
const USER_LOGIN_MAX_ATTEMPTS = 5;
const USER_ACCOUNT_LOCKOUT_DURATION_MINUTES = 30;

const PRODUCT_DISCOUNT_MAX_PERCENTAGE = 90;
const PRODUCT_STOCK_LOW_THRESHOLD = 10;
const PRODUCT_REVIEW_MIN_RATING = 1;
const PRODUCT_REVIEW_MAX_RATING = 5;

const ORDER_PROCESSING_TIMEOUT_HOURS = 24;
const ORDER_CANCELLATION_WINDOW_HOURS = 2;
const ORDER_REFUND_PROCESSING_DAYS = 5;
const ORDER_SHIPPING_CUTOFF_HOUR = 15; // 3 PM

// Tax and financial constants
const DEFAULT_TAX_RATE = 0.08;           // 8%
const SHIPPING_FREE_THRESHOLD = 50.00;
const CURRENCY_DECIMAL_PLACES = 2;

// ❌ Bad - Generic business constants
const TRIAL_DAYS = 30;          // Trial for what?
const MAX_ATTEMPTS = 5;         // Attempts for what?
const THRESHOLD = 10;           // Threshold for what?
```

#### Feature Flags & Configuration
```typescript
// ✅ Feature flag constants with clear purpose
const FEATURE_USER_PROFILE_V2_ENABLED = true;
const FEATURE_PRODUCT_RECOMMENDATIONS_ENABLED = false;
const FEATURE_ORDER_TRACKING_ENHANCED_ENABLED = true;
const FEATURE_DARK_MODE_ENABLED = true;

const ANALYTICS_ENABLED = true;
const DEBUG_MODE_ENABLED = process.env.NODE_ENV === 'development';
const CACHE_ENABLED = true;
const LOGGING_LEVEL = 'info' as const;

// Environment-specific constants
const IS_PRODUCTION = process.env.NODE_ENV === 'production';
const IS_DEVELOPMENT = process.env.NODE_ENV === 'development';
const IS_TEST_ENVIRONMENT = process.env.NODE_ENV === 'test';

// ❌ Bad - Generic feature flags
const ENABLED = true;           // What is enabled?
const DEBUG = false;            // Debug what?
const FEATURE_FLAG = true;      // Which feature?
```

### Enums Naming Structure

**Format**: PascalCase with descriptive names

```typescript
// ✅ Good - Clear enum purpose and values
enum UserAccountStatus {
  Active = 'active',
  Inactive = 'inactive',
  Suspended = 'suspended',
  PendingVerification = 'pending_verification',
  Deleted = 'deleted'
}

enum ProductAvailabilityStatus {
  InStock = 'in_stock',
  OutOfStock = 'out_of_stock',
  LimitedStock = 'limited_stock',
  Discontinued = 'discontinued',
  PreOrder = 'pre_order'
}

// ❌ Bad - Generic or unclear enums
enum Status {             // Status of what?
  Active = 'active',
  Inactive = 'inactive'
}

enum Type {               // Type of what?
  A = 'a',
  B = 'b'
}
```

### Domain-Based Enum Categories

#### User Domain Enums
```typescript
// ✅ User-related enums
enum UserRole {
  Guest = 'guest',
  Customer = 'customer',
  Premium = 'premium',
  Admin = 'admin',
  SuperAdmin = 'super_admin'
}

enum UserPreferenceTheme {
  Light = 'light',
  Dark = 'dark',
  Auto = 'auto'
}
```

#### Product Domain Enums
```typescript
// ✅ Product-related enums
enum ProductCategory {
  Electronics = 'electronics',
  Clothing = 'clothing',
  Books = 'books',
  Home = 'home',
  Sports = 'sports',
  Beauty = 'beauty'
}

enum ProductCondition {
  New = 'new',
  Used = 'used',
  Refurbished = 'refurbished',
  OpenBox = 'open_box'
}
```

#### Order Domain Enums
```typescript
// ✅ Order-related enums
enum OrderPaymentMethod {
  CreditCard = 'credit_card',
  DebitCard = 'debit_card',
  PayPal = 'paypal',
  ApplePay = 'apple_pay',
  GooglePay = 'google_pay',
  BankTransfer = 'bank_transfer'
}

enum OrderPriority {
  Low = 'low',
  Normal = 'normal',
  High = 'high',
  Urgent = 'urgent'
}
```

#### System & Technical Enums
```typescript
// ✅ System-related enums
enum ApiErrorCode {
  BadRequest = 'BAD_REQUEST',
  Unauthorized = 'UNAUTHORIZED',
  Forbidden = 'FORBIDDEN',
  NotFound = 'NOT_FOUND',
  InternalServerError = 'INTERNAL_SERVER_ERROR',
  ServiceUnavailable = 'SERVICE_UNAVAILABLE'
}

enum LogLevel {
  Error = 'error',
  Warn = 'warn',
  Info = 'info',
  Debug = 'debug',
  Trace = 'trace'
}
```

### String Literal Types vs Enums

```typescript
// ✅ Use string literal types for simple, stable values
type UserGender = 'male' | 'female' | 'other' | 'prefer_not_to_say';
type ProductSize = 'xs' | 's' | 'm' | 'l' | 'xl' | 'xxl';
type OrderCurrency = 'USD' | 'EUR' | 'GBP' | 'CAD';
type FileFormat = 'pdf' | 'docx' | 'txt' | 'csv' | 'xlsx';

// ✅ Use enums for complex values that might change
enum UserNotificationPreference {
  EmailOnly = 'email_only',
  SMSOnly = 'sms_only',
  EmailAndSMS = 'email_and_sms',
  PushOnly = 'push_only',
  AllChannels = 'all_channels',
  None = 'none'
}
```

### File Organization Patterns

#### Constants Files
```typescript
// ✅ Domain-specific constant files
// user.constants.ts
export const USER_SESSION_TIMEOUT_MS = 30 * 60 * 1000;
export const USER_PASSWORD_MIN_LENGTH = 8;
export const USER_LOGIN_MAX_ATTEMPTS = 5;

// product.constants.ts  
export const PRODUCT_SEARCH_DEBOUNCE_MS = 300;
export const PRODUCT_IMAGE_MAX_SIZE_MB = 5;
export const PRODUCT_NAME_MAX_LENGTH = 200;

// api.constants.ts
export const API_REQUEST_TIMEOUT_MS = 10000;
export const API_MAX_RETRY_ATTEMPTS = 3;

// ui.constants.ts
export const DEFAULT_PAGE_SIZE = 20;
export const MODAL_ANIMATION_DURATION_MS = 300;
```

#### Enum Files
```typescript
// ✅ Domain-specific enum files
// user.types.ts
export enum UserRole { }
export enum UserAccountStatus { }
export enum UserNotificationType { }

// product.types.ts
export enum ProductCategory { }
export enum ProductAvailabilityStatus { }
export enum ProductSortOption { }

// order.types.ts
export enum OrderProcessingStatus { }
export enum OrderPaymentMethod { }
export enum OrderShippingMethod { }
```

### Constant & Enum Anti-Patterns

```typescript
// ❌ Avoid these patterns:

// 1. Generic constant names
const TIMEOUT = 5000;         // → USER_SESSION_TIMEOUT_MS
const MAX = 100;              // → ORDER_ITEMS_MAX_COUNT
const URL = 'api.com';        // → USER_API_BASE_URL

// 2. Inconsistent casing for constants
const userTimeout = 5000;     // → USER_TIMEOUT_MS
const User_Max_Items = 100;   // → USER_MAX_ITEMS

// 3. Generic enum names
enum Status { }               // → UserAccountStatus
enum Type { }                 // → ProductCategory
enum Mode { }                 // → UserPreferenceTheme

// 4. Inconsistent enum value formats
enum OrderStatus {
  pending = 'PENDING',        // Mixed case styles
  Processing = 'processing',
  SHIPPED = 'Shipped'
}

// 5. Magic numbers/strings instead of constants
setTimeout(callback, 5000);   // → USER_SESSION_TIMEOUT_MS
if (items.length > 100) { }   // → ORDER_ITEMS_MAX_COUNT
if (status === 'active') { }  // → UserAccountStatus.Active

// 6. Missing domain context
const API_ENDPOINTS = { };    // → USER_API_ENDPOINTS
const VALIDATION_RULES = { }; // → USER_VALIDATION_RULES
const DEFAULT_CONFIG = { };   // → USER_DEFAULT_CONFIG
```

**Remember**: Constants and enums should be **self-documenting** and clearly indicate their domain and purpose. Use descriptive names that eliminate the need for mental mapping or additional documentation.

## Testing

Test files and functions should follow clear naming conventions that indicate what is being tested, the expected behavior, and the test context using descriptive names that make test failures easy to understand. Use the **AAA pattern** (Arrange, Act, Assert) for test structure and `test` blocks for better readability.

### Core test Categories

1. **Use `test` blocks**: Use `test` instead of `it` for consistency and clarity
2. **AAA Pattern**: Structure all tests with clear Arrange, Act, Assert sections
3. **Test Files**: Match source file names with `.test.[tsx|ts]` suffix
4. **Test Suites**: Use descriptive `describe` blocks indicating component/function being tested
5. **Test Cases**: Start with "should" and describe expected behavior and conditions
6. **Mock Objects**: Prefix with `mock` and include domain context, organize by AAA sections
7. **Helper Functions**: Clearly indicate their AAA purpose (`arrangeX`, `actX`, `assertX`)
8. **Organization**: Group tests by domain and feature area
9. **Integration Tests**: Describe the full flow with clear AAA structure
10. **Consistency**: Maintain consistent naming patterns and AAA structure across all tests

### Test File Naming Structure

**Format**: `[ComponentName].test.[tsx|ts]` or `[functionName].test.[tsx|ts]`

```typescript
// ✅ Good - Clear test file names matching source files
UserProfileCard.test.tsx        // Tests for UserProfileCard component
ProductCatalogGrid.test.tsx     // Tests for ProductCatalogGrid component
useUserAuthentication.test.ts   // Tests for useUserAuthentication hook
calculateOrderTax.test.ts       // Tests for calculateOrderTax function

// ❌ Bad - Generic or unclear test file names
test.tsx                        // Test for what?
component.test.tsx              // Which component?
utils.test.ts                   // Which utilities?
```

### Test Suite Naming (describe blocks)

**Format**: `[ComponentName|FunctionName]`

```typescript
// ✅ Good - Clear test suite organization
describe('UserProfileCard', () => {
  describe('when user is authenticated', () => {
    describe('and user can edit profile', () => {
      // ... test cases using AAA pattern
    });
  });
});

describe('useUserAuthentication', () => {
  describe('when user logs in successfully', () => {
    // ... success scenarios
  });
  
  describe('when user login fails', () => {
    // ... error scenarios
  });
});

// ❌ Bad - Generic or unclear suite names
describe('Component', () => { });         // Which component?
describe('Tests', () => { });             // Tests for what?
```

### Test Case Naming (test blocks with AAA pattern)

**Format**: `should [expected behavior] when [condition/context]`

```typescript
// ✅ Good - Clear test case descriptions using test blocks and AAA pattern
describe('UserProfileCard', () => {
  test('should display user name and email when user data is provided', () => {
    // Arrange
    const mockUserData = {
      id: 'user-123',
      firstName: 'John',
      lastName: 'Doe',
      email: 'john@example.com'
    };
    
    // Act
    render(<UserProfileCard user={mockUserData} onEditClick={mockOnEditClick} />);
    
    // Assert
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });
  
  test('should show edit button when user has edit permissions', () => {
    // Arrange
    const mockUserWithEditPermissions = { ...mockUserData, canEdit: true };
    
    // Act
    render(<UserProfileCard user={mockUserWithEditPermissions} onEditClick={mockOnEditClick} />);
    
    // Assert
    expect(screen.getByRole('button', { name: /edit profile/i })).toBeInTheDocument();
  });
  
  test('should call onEditClick when edit button is clicked', () => {
    // Arrange
    const mockUserData = { ...baseUserData, canEdit: true };
    const mockOnEditClick = jest.fn();
    
    // Act
    render(<UserProfileCard user={mockUserData} onEditClick={mockOnEditClick} />);
    fireEvent.click(screen.getByRole('button', { name: /edit profile/i }));
    
    // Assert
    expect(mockOnEditClick).toHaveBeenCalledWith(mockUserData.id);
  });
});

describe('validateUserEmail', () => {
  test('should return valid result when email format is correct', () => {
    // Arrange
    const validEmail = 'user@example.com';
    
    // Act
    const result = validateUserEmail(validEmail);
    
    // Assert
    expect(result.isValid).toBe(true);
    expect(result.errorMessage).toBeNull();
  });
  
  test('should return invalid result when email is missing @ symbol', () => {
    // Arrange
    const invalidEmail = 'userexample.com';
    
    // Act
    const result = validateUserEmail(invalidEmail);
    
    // Assert
    expect(result.isValid).toBe(false);
    expect(result.errorMessage).toBe('Please enter a valid email address');
  });
});

// ❌ Bad - Unclear or generic test descriptions
test('works', () => { });                           // What works?
test('renders correctly', () => { });               // Renders what correctly?
test('handles click', () => { });                   // Handles click how?
```

### Mock Object Naming with AAA Pattern

**Format**: `mock[HighContext][LowContext]` organized by AAA sections

```typescript
// ✅ Good - Clear mock object names organized by AAA pattern
describe('UserProfileService', () => {
  test('should update user profile when valid data is provided', async () => {
    // Arrange - Setup mocks and test data
    const mockUserProfileData = {
      id: 'user-123',
      firstName: 'John',
      lastName: 'Doe',
      // ... existing user properties
    };
    
    const mockUpdatedProfileData = {
      ...mockUserProfileData,
      firstName: 'Jane'
    };
    
    const mockHttpClient = {
      patch: jest.fn().mockResolvedValue({ success: true, data: mockUpdatedProfileData })
    };
    
    // Act
    const userProfileService = new UserProfileService(mockHttpClient);
    const result = await userProfileService.updateUserProfile(mockUserProfileData.id, { firstName: 'Jane' });
    
    // Assert
    expect(mockHttpClient.patch).toHaveBeenCalledWith(`/users/${mockUserProfileData.id}`, { firstName: 'Jane' });
    expect(result).toEqual(mockUpdatedProfileData);
  });
});

// ❌ Bad - Generic mock names
const mockData = { };                     // Mock data for what?
const mockService = { };                  // Mock service for what?
const mockFunction = jest.fn();           // Mock function for what?
```

### Test Helper Function Naming with AAA Pattern

**Format**: `[arrange|act|assert][TestScenario]Helper`

```typescript
// ✅ Good - AAA-organized test helper functions
const arrangeUserProfileCardTestWithValidData = () => {
  return {
    mockUserData: {
      id: 'user-123',
      firstName: 'John',
      lastName: 'Doe',
      // ... existing user properties
    },
    mockHandleEditClick: jest.fn(),
    // ... existing mock handlers
  };
};

const arrangeUserProfileCardTestWithLoadingState = () => {
  return {
    mockUserData: null,
    isLoading: true,
    mockHandleEditClick: jest.fn()
  };
};

const actUserProfileCardRender = (props: UserProfileCardProps) => {
  return render(<UserProfileCard {...props} />);
};

const assertUserProfileCardDisplaysCorrectly = (expectedUserData: UserProfile) => {
  expect(screen.getByText(`${expectedUserData.firstName} ${expectedUserData.lastName}`)).toBeInTheDocument();
  expect(screen.getByText(expectedUserData.email)).toBeInTheDocument();
  // ... existing assertions
};

// Usage in tests
describe('UserProfileCard', () => {
  test('should display user information when valid data is provided', () => {
    // Arrange
    const { mockUserData, mockHandleEditClick } = arrangeUserProfileCardTestWithValidData();
    
    // Act
    actUserProfileCardRender({ user: mockUserData, onEditClick: mockHandleEditClick });
    
    // Assert
    assertUserProfileCardDisplaysCorrectly(mockUserData);
  });
});

// ❌ Bad - Generic helper function names
const setup = () => { };                  // Setup what?
const render = () => { };                 // Render what?
const check = () => { };                  // Check what?
```

### Integration Test Naming with AAA Pattern

```typescript
// ✅ Good - Clear integration test naming with AAA structure
describe('User Authentication Integration', () => {
  test('should authenticate user and redirect to dashboard when login credentials are valid', async () => {
    // Arrange
    const mockValidCredentials = { email: 'user@example.com', password: 'correctPassword123' };
    const mockAuthResponse = { user: { id: '123', email: 'user@example.com' }, token: 'valid-jwt-token' };
    
    jest.spyOn(authApi, 'login').mockResolvedValue(mockAuthResponse);
    jest.spyOn(router, 'navigate').mockImplementation(mockNavigate);
    
    // Act
    render(<LoginForm />);
    fireEvent.change(screen.getByLabelText(/email/i), { target: { value: mockValidCredentials.email } });
    fireEvent.change(screen.getByLabelText(/password/i), { target: { value: mockValidCredentials.password } });
    fireEvent.click(screen.getByRole('button', { name: /sign in/i }));
    
    // Assert
    await waitFor(() => {
      expect(authApi.login).toHaveBeenCalledWith(mockValidCredentials);
      expect(mockNavigate).toHaveBeenCalledWith('/dashboard');
    });
    expect(localStorage.getItem('authToken')).toBe('valid-jwt-token');
  });
  
  test('should display error message and remain on login page when credentials are invalid', async () => {
    // Arrange
    const mockInvalidCredentials = { email: 'user@example.com', password: 'wrongPassword' };
    jest.spyOn(authApi, 'login').mockRejectedValue(new Error('Invalid credentials'));
    
    // Act
    render(<LoginForm />);
    // ... existing form interaction
    fireEvent.click(screen.getByRole('button', { name: /sign in/i }));
    
    // Assert
    await waitFor(() => {
      expect(screen.getByRole('alert')).toHaveTextContent('Invalid credentials');
    });
    expect(mockNavigate).not.toHaveBeenCalled();
  });
});
```

### Test Anti-Patterns

```typescript
// ❌ Avoid these testing patterns:

// 1. Using 'it' instead of 'test'
it('works', () => { });                    // → Use 'test' for consistency

// 2. Missing AAA pattern structure
test('should update user profile', () => {
  const user = { name: 'John' };           // No clear Arrange section
  updateProfile(user);                     // Act mixed with Arrange
  expect(result).toBeTruthy();             // Assert without clear expectation
});

// 3. Generic test descriptions
test('works', () => { });
test('renders', () => { });
test('handles input', () => { });

// 4. Generic mock names without context
test('should call function', () => {
  const mock = jest.fn();                  // Mock what function?
  const data = { };                        // What kind of data?
  // ... existing test logic
});
```



**Remember**: Test names should be **self-documenting** using the **AAA pattern** structure. Each test should have clear Arrange, Act, and Assert sections with descriptive comments. Use `test` blocks for better readability and consistency across the codebase.
