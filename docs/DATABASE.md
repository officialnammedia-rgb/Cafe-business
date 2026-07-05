# Database Schema Reference

_Prisma-oriented schema reference consolidated from Master PRD Chapter 6 (and the Chapter 4 core-entity
list). The full chapter narrative, business rules, caching, and relationships are in
[06_Database.md](./06_Database.md). This file is the field-by-field reference._

## Conventions (apply to every table)

- **Primary keys:** UUID.
- **Timestamps:** `createdAt`, `updatedAt` on every table.
- **Soft deletes:** `deletedAt` where appropriate.
- **Foreign keys:** enforced constraints; maintain referential integrity.
- **Indexes:** on frequently queried fields (noted per table).
- Normalized schema, but denormalize price via **snapshots** on cart/order items to preserve historical
  accuracy.

## Enums

```
OrderStatus    = Pending | Accepted | Preparing | Ready | OutForDelivery | Delivered | Cancelled | Refunded
PaymentStatus  = Pending | Authorized | Paid | Failed | Refunded
StaffRole      = Owner | Manager | Kitchen | Cashier | Support
CouponType     = Percentage | FixedAmount | FreeDelivery(future)
AddressLabel   = Home | Work | Other
Availability   = Available | OutOfStock | Hidden | Seasonal   (product/menu availability)
StoreStatus    = Open | Busy | Closed | Maintenance
```

## Core tables

### Restaurant (tenant root)
| Field | Notes |
|---|---|
| `id` | UUID PK |
| `name` | |
| `slug` | unique · **index** |
| `description` | |
| `phone`, `email` | |
| `logoUrl`, `bannerUrl` | Cloudinary |
| `address`, `latitude`, `longitude` | |
| `timezone`, `currency` | |
| `taxPercentage` | |
| `deliveryRadiusKm` | |
| `minimumOrderAmount` | |
| `isOpen` | **index** |
| `createdAt`, `updatedAt`, `deletedAt` | |

### User (auth identity)
| Field | Notes |
|---|---|
| `id` | UUID PK |
| `email` | **unique** |
| `phone` | unique, nullable (depends on auth flow) |
| `passwordHash` | if applicable; never plaintext |
| `emailVerified`, `phoneVerified` | |
| `lastLogin`, `status` | |
| `createdAt`, `updatedAt` | |

### Customer (profile)
`id` · `userId` (FK→User) · `fullName` · `dateOfBirth?` · `profileImage` · `marketingConsent` ·
`defaultAddressId` (FK→Address) · `createdAt` · `updatedAt`.

### Staff
`id` · `userId` (FK→User) · `restaurantId` (FK→Restaurant) · `role` (StaffRole) · `active` ·
`createdAt`.

### Address
`id` · `customerId` (FK→Customer) · `label` (AddressLabel) · `line1` · `line2` · `city` · `state` ·
`postalCode` · `latitude` · `longitude` · `landmark` · `instructions` · `isDefault`.

### Category
`id` · `restaurantId` (FK) · `name` · `slug` · `displayOrder` · `image` · `visible`.
**Index:** `restaurantId`.

### Product
| Field | Notes |
|---|---|
| `id` | UUID PK |
| `restaurantId` | FK · **index** |
| `categoryId` | FK · **index** |
| `name`, `slug` | |
| `shortDescription`, `description` | |
| `price`, `discountedPrice` | |
| `preparationTime` | |
| `calories` | future |
| `image` | primary image |
| `featured` | **index** |
| `bestseller` | **index** |
| `available` | **index** |
| `active` | |

### ProductImage
`id` · `productId` (FK) · `imageUrl` · `displayOrder`. (Multiple per product.)

### ProductVariantGroup
`id` · `productId` (FK) · `title` · `required` · `multiSelect`. Examples: Size, Milk, Sugar, Ice, Bread.

### VariantOption
`id` · `variantGroupId` (FK) · `title` · `additionalPrice`. Example: "Oat Milk", `+₹40`.

### Cart (one active cart per customer per restaurant)
`id` · `customerId` (FK) · `restaurantId` (FK) · `expiresAt`. **Unique:** (`customerId`,`restaurantId`)
active cart.

### CartItem
`id` · `cartId` (FK) · `productId` (FK) · `quantity` · `priceSnapshot` · `notes`.

### Order
| Field | Notes |
|---|---|
| `id` | UUID PK |
| `restaurantId` | FK · **index** |
| `customerId` | FK · **index** |
| `addressId` | FK |
| `paymentId` | FK→Payment |
| `couponId` | FK→Coupon, nullable |
| `subtotal`, `tax`, `deliveryFee`, `discount`, `grandTotal` | server-calculated |
| `status` | OrderStatus · **index** |
| `paymentStatus` | PaymentStatus |
| `notes` | customer notes |
| `placedAt` | |

### OrderItem
`id` · `orderId` (FK) · `productId` (FK) · `quantity` · `unitPrice` · `totalPrice` · `notes`.

### OrderItemVariant
Selected options per order item, e.g. "Large", "Oat Milk", "Extra Shot". `id` · `orderItemId` (FK) ·
`title` · `additionalPrice`.

### Coupon
`id` · `restaurantId` (FK) · `code` · `discountType` (CouponType) · `value` · `expiry` · `usageLimit` ·
`minimumOrder`. (Dashboard also captures maxDiscount, applicable categories/products — see
[03_Admin_Dashboard.md](./03_Admin_Dashboard.md#coupons).) **Index:** (`restaurantId`,`code`) unique.

### Payment
`id` · `provider` · `providerOrderId` · `providerPaymentId` · `amount` · `currency` · `verified` ·
`status` (PaymentStatus). Verified server-side before order confirmation.

### Notification
`id` · `userId` (FK) · `title` · `message` · `type` · `read`.

### AuditLog
`id` · `userId` (who) · `action` (what) · `resource` (which) · `timestamp` (when) · `ip` · `metadata`.

## Entity relationships

```
Restaurant → Categories → Products → VariantGroups → VariantOptions
Restaurant → Cart → CartItems
Restaurant → Orders → OrderItems → OrderItemVariants
Orders → Payments
Restaurant → Coupons
User → Customer → Addresses
User → Staff (→ Restaurant)
User → Notifications
* → AuditLogs
```

## Kitchen notes

The dashboard supports **internal kitchen notes** on orders (staff-only, hidden from customers). Model
as a field or related table on Order — see
[03_Admin_Dashboard.md](./03_Admin_Dashboard.md#kitchen-notes).

## Authentication tables

If **Better Auth** is adopted, it manages its own account/session tables which may overlap with the
custom `User` table above. Reconcile during implementation — see
[DECISIONS.md](./DECISIONS.md#i-010--better-auth-managed-tables-vs-custom-user).

## Not modeled in V1 (flagged)

- **Reviews/ratings** — referenced by UI but no table in the PRD (see
  [DECISIONS.md](./DECISIONS.md#i-009--reviews--ratings-source)).
- **Recently viewed** — client-side in V1 (see
  [DECISIONS.md](./DECISIONS.md#i-008--recently-viewed-last-10-products-persistence)).
- **Store settings / hours / holiday hours** — captured under Restaurant + a settings blob; multiple
  time slots per day required (see [03_Admin_Dashboard.md](./03_Admin_Dashboard.md#store-hours)).
