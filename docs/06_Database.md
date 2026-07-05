# 06 · Database Design, API Specification & Business Logic

_Source: Master PRD Chapter 6. This is the full chapter narrative. A focused, Prisma-oriented schema
reference is in [DATABASE.md](./DATABASE.md); the full API contract is in [API.md](./API.md)._

## Objective

The database should support: multiple cafés · thousands of products · millions of orders · future
delivery partners · future ONDC integration · loyalty · franchises · multiple branches · mobile
applications. It should remain **normalized while still allowing fast reads**. Use **PostgreSQL +
Prisma**.

## Database principles

- Use **UUIDs** for primary keys.
- Include `createdAt` and `updatedAt` timestamps on **every** table.
- Support **soft deletes** where appropriate using `deletedAt`.
- Use **foreign key constraints**.
- Create **indexes** on frequently queried fields.
- Avoid duplicate data where possible.
- Maintain **referential integrity**.

## Core tables

> Field-level lists below are reproduced from the PRD. See [DATABASE.md](./DATABASE.md) for the
> consolidated reference including indexes, cascade guidance, and enum definitions.

### Restaurant — represents a café
`id (UUID)` · `name` · `slug (unique)` · `description` · `phone` · `email` · `logoUrl` · `bannerUrl` ·
`address` · `latitude` · `longitude` · `timezone` · `currency` · `taxPercentage` · `deliveryRadiusKm` ·
`minimumOrderAmount` · `isOpen` · `createdAt` · `updatedAt` · `deletedAt`.
**Indexes:** `slug`, `isOpen`.

### User — authentication identity
`id` · `email` · `phone` · `passwordHash (if applicable)` · `emailVerified` · `phoneVerified` ·
`lastLogin` · `status` · `createdAt` · `updatedAt`.
**Unique:** `email`, `phone` (nullable depending on auth flow).

### Customer — customer profile
`id` · `userId` · `fullName` · `dateOfBirth (optional)` · `profileImage` · `marketingConsent` ·
`defaultAddressId` · `createdAt` · `updatedAt`.

### Staff
`id` · `userId` · `restaurantId` · `role` · `active` · `createdAt`.
**Roles:** Owner · Manager · Kitchen · Cashier · Support.

### Address
`id` · `customerId` · `label (Home, Work, Other)` · `line1` · `line2` · `city` · `state` · `postalCode`
· `latitude` · `longitude` · `landmark` · `instructions` · `isDefault`.

### Category
`id` · `restaurantId` · `name` · `slug` · `displayOrder` · `image` · `visible`.

### Product
`id` · `restaurantId` · `categoryId` · `name` · `slug` · `shortDescription` · `description` · `price` ·
`discountedPrice` · `preparationTime` · `calories (future)` · `image` · `featured` · `bestseller` ·
`available` · `active`.
**Indexes:** `restaurantId`, `categoryId`, `featured`, `bestseller`, `available`.

### Product Images (multiple per product)
`id` · `productId` · `imageUrl` · `displayOrder`.

### Product Variant Groups (e.g. Size, Milk, Sugar, Ice, Bread)
`id` · `productId` · `title` · `required` · `multiSelect`.

### Variant Options
`id` · `variantGroupId` · `title` · `additionalPrice`. Example: "Oat Milk +₹40".

### Cart (one active cart per customer per restaurant)
`id` · `customerId` · `restaurantId` · `expiresAt`.

### Cart Items
`id` · `cartId` · `productId` · `quantity` · `priceSnapshot` · `notes`.
**Store price snapshots** to preserve historical accuracy even if prices change later.

### Orders
`id` · `restaurantId` · `customerId` · `addressId` · `paymentId` · `couponId` · `subtotal` · `tax` ·
`deliveryFee` · `discount` · `grandTotal` · `status` · `paymentStatus` · `notes` · `placedAt`.

#### Order status
`Pending · Accepted · Preparing · Ready · OutForDelivery · Delivered · Cancelled · Refunded`.

#### Payment status
`Pending · Authorized · Paid · Failed · Refunded`.

### Order Items (each purchased product)
`id` · `orderId` · `productId` · `quantity` · `unitPrice` · `totalPrice` · `notes`.

### Order Item Variants (selected options)
Stores selected options, e.g. "Large", "Oat Milk", "Extra Shot".

### Coupons
`id` · `restaurantId` · `code` · `discountType` · `value` · `expiry` · `usageLimit` · `minimumOrder`.

### Payments
`id` · `provider` · `providerOrderId` · `providerPaymentId` · `amount` · `currency` · `verified` ·
`status`.

### Notifications
`id` · `userId` · `title` · `message` · `type` · `read`.

### Audit Logs
Track: **Who · What · When · IP · Metadata.**

## Entity relationships

```
Restaurant → Categories → Products → Variants → Cart → Orders → Payments
Customer   → Addresses  → Orders   → Notifications
```

## API standards

Base URL: `/api/v1`. Consistent success envelope:

```json
{ "success": true, "data": {}, "message": "Optional message" }
```

Error envelope:

```json
{
  "success": false,
  "error": { "code": "ORDER_NOT_FOUND", "message": "The requested order could not be found." }
}
```

### Endpoint groups (full list in [API.md](./API.md))
- **Auth:** `POST /auth/register`, `/auth/login`, `/auth/logout`, `/auth/refresh`, `GET /auth/me`.
- **Customer:** `GET/PATCH /customer/profile`, `GET /customer/orders`, `GET /customer/favorites`,
  `POST /customer/address`, `PATCH/DELETE /customer/address/:id`.
- **Menu:** `GET /menu`, `/menu/categories`, `/products`, `/products/:slug`, `/featured`, `/search`.
- **Cart:** `GET /cart`, `POST /cart/add`, `PATCH /cart/update`, `DELETE /cart/item/:id`, `DELETE /cart`.
- **Checkout:** `POST /checkout`, `POST /payments/create`, `POST /payments/verify`, `GET /orders/:id`.
- **Dashboard:** `GET /dashboard`, `GET /orders`, `PATCH /orders/:id/status`, `POST /products`,
  `PATCH /products/:id`, `DELETE /products/:id`, `GET /analytics`, `GET /customers`.

## Validation rules

Every endpoint must: validate input types · reject unknown fields · validate IDs · validate ownership ·
validate restaurant context · return meaningful errors.

## Business rules

**Products marked unavailable:** cannot be added to the cart; existing carts are revalidated at
checkout.

**Orders:** totals are always calculated on the server; coupon eligibility is recalculated at checkout;
payment verification is required before marking prepaid orders as confirmed.

**Restaurant:** cannot accept orders when marked closed (unless scheduled pre-orders are introduced in
the future).

**Customers:** can only access their own orders and profile data.

**Staff:** can only manage data belonging to their assigned restaurant.

## Caching strategy

**Cache:** Menu · Categories · Featured products · Restaurant settings.
**Do NOT cache:** Payments · User profiles · Active orders · Sensitive account information.
**Invalidate caches immediately after relevant updates.**

## Rate limiting

Apply stricter limits to: Login · Registration · Password reset · OTP requests (future) · Checkout ·
Payment verification. General read endpoints may have more relaxed limits. See
[07_Security.md](./07_Security.md#rate-limiting).

## Logging & monitoring

Track: API latency · error rates · payment failures · failed login attempts · order creation failures ·
background job failures. Use **structured logs with correlation IDs** to aid debugging.

## Final instruction (Chapter 6)

Treat the backend as a **long-lived platform**, not a one-off project. Favor clean abstractions,
explicit validation, strong typing, and modular architecture over shortcuts. Every decision should make
it easier to add future capabilities — delivery integrations, ONDC, loyalty, or additional cafés —
without major refactoring.
