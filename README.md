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
*React + TypeScript

## Table of Contents
1. [Core Principles](#core-principles)
2. [A/HC/LC Function Naming Pattern](#ahclc-function-naming-pattern)
3. [Variables](#variables)
4. [Functions & Methods](#functions--methods)
5. [Event Handlers](#event-handlers)
6. [React Components](#react-components)
7. [Custom Hooks](#custom-hooks)
8. [Types & Interfaces](#types--interfaces)
9. [Constants & Enums](#constants--enums)
10. [GraphQL](#graphql)
11. [File & Directory Structure](#file--directory-structure)
12. [Testing](#testing)
13. [Quick Reference](#quick-reference)

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

## A/HC/LC Function Naming Pattern

Based on [kettanaito's naming cheatsheet](https://github.com/kettanaito/naming-cheatsheet?tab=readme-ov-file#ahclc-pattern), follow this structure:

**Format**: `action` + `highContext` + `lowContext`

- **A (Action)** - The function verb (get, set, reset, fetch, etc.)
- **HC (High Context)** - The domain/entity (User, Product, Order, etc.)
- **LC (Low Context)** - The specific detail (Profile, Email, Status, etc.)

### Pattern Structure

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
| **parse** | Convert data types | `parseUserInput`, `parseOrderResponse` |
| **handle** | Event processing | `handleUserClick`, `handleFormSubmit` |

### Examples by Context

#### User Management
```typescript
// ✅ A/HC/LC Pattern
const getUserProfile = (userId: string): UserProfile => { };
const setUserPreferences = (prefs: UserPreferences): void => { };
const validateUserEmail = (email: string): boolean => { };
const resetUserPassword = (userId: string): Promise<void> => { };
const fetchUserNotifications = (userId: string): Promise<Notification[]> => { };
const updateUserStatus = (userId: string, status: UserStatus): void => { };
const calculateUserAge = (birthDate: Date): number => { };
const formatUserDisplayName = (user: User): string => { };

// ❌ Inconsistent patterns
const getProfile = (userId: string): UserProfile => { }; // Missing HC
const userEmailValidation = (email: string): boolean => { }; // Wrong structure
const fetchNotifications = (userId: string): Promise<Notification[]> => { }; // Missing HC
```

#### Product Management
```typescript
// ✅ A/HC/LC Pattern
const getProductDetails = (productId: string): Product => { };
const setProductPrice = (productId: string, price: number): void => { };
const validateProductStock = (productId: string): boolean => { };
const updateProductCategory = (productId: string, category: Category): void => { };
const calculateProductDiscount = (product: Product, discountRate: number): number => { };
const formatProductCurrency = (price: number): string => { };

// ❌ Inconsistent patterns  
const productDetails = (productId: string): Product => { }; // Missing action
const setPricing = (productId: string, price: number): void => { }; // Missing HC
const checkStock = (productId: string): boolean => { }; // Different action verb
```

#### Order Processing
```typescript
// ✅ A/HC/LC Pattern
const getOrderSummary = (orderId: string): OrderSummary => { };
const setOrderStatus = (orderId: string, status: OrderStatus): void => { };
const validateOrderItems = (items: OrderItem[]): boolean => { };
const calculateOrderTotal = (order: Order): number => { };
const updateOrderShipping = (orderId: string, shipping: ShippingInfo): void => { };
const resetOrderFilters = (): void => { };
const formatOrderDate = (date: Date): string => { };

// ❌ Inconsistent patterns
const orderSummary = (orderId: string): OrderSummary => { }; // Missing action
const updateStatus = (orderId: string, status: OrderStatus): void => { }; // Missing HC
const getTotalCost = (order: Order): number => { }; // Different LC context
```

### React-Specific Patterns

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
const fetchUserProfile = async (userId: string): Promise<UserProfile> => { };
const createUserAccount = async (userData: CreateUserRequest): Promise<User> => { };
const updateProductInventory = async (productId: string, quantity: number): Promise<void> => { };
const deleteOrderItem = async (orderId: string, itemId: string): Promise<void> => { };

// ❌ Inconsistent API patterns
const apiGetUser = async (userId: string): Promise<UserProfile> => { }; // Unnecessary prefix
const userCreate = async (userData: CreateUserRequest): Promise<User> => { }; // Wrong structure
const removeItem = async (orderId: string, itemId: string): Promise<void> => { }; // Missing HC
```

### Advanced Patterns

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

### Pattern Benefits

1. **Predictability** - Easy to guess function names following the pattern
2. **Searchability** - Consistent naming makes code easier to search
3. **Grouping** - Functions naturally group by high context (User*, Product*, Order*)
4. **Scalability** - Pattern grows consistently with codebase
5. **Clarity** - Clear action and context in every function name

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

const productSearchConfiguration = {
  debounceDelayMs: 300,
  minimumSearchLength: 3,
  maximumResults: 50,
  enableAutoComplete: true
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

## Functions & Methods

## Event Handlers

## React Components

## Custom Hooks

## Types & Interfaces

## Constants & Enums

## GraphQL

## File & Directory Structure

## Testing

## Quick Reference
