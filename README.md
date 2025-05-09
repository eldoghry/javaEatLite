# JavaEatLite

## Index

- [JavaEatLite](#javaeatlite)
  - [Index](#index)
  - [Vision](#vision)
  - [Domain](#domain)
  - [Functional Requirements](#functional-requirements)
  - [Non-Functional Requirements](#non-functional-requirements)
  - [Constraints](#constraints)
  - [Assumptions](#assumptions)
  - [Actors](#actors)
  - [Use Cases](#use-cases)
  - [Data Model](#data-model)
  - [API Design](#api-design)

## Vision

The Food Delivery System aims to provide a seamless and efficient platform for customers to browse restaurants, order food, make payments, and track deliveries. It also enables restaurant owners to manage their menus and orders, while delivery agents can track and complete deliveries efficiently.

## Domain

The system operates within the food delivery industry, connecting customers with restaurants and couriers to facilitate online food ordering and delivery.

## Functional Requirements

- User Registration & Authentication

  - Sign up

  - Login

  - Forget Password

  - Email verification or OTP verification

  - User Profile

  - Logout

  - Social Media Authentication

  - Enable or Disable account

  - Role-based Access Control & Permissions

- Restaurant Management

  - Add, update, enable/disable restaurants

  - View all restaurants

  - Manage menus (create, update, delete, view history, list all menus)

  - Search restaurants and menu items

  - Top-rated restaurants and recommendations

- Menu Management

- Cart Management

  - Add, modify, view, clear, remove items

  - Checkout process

  - Update quantities

- Order Management

  - Place order

  - Cancel orders (by customer or restaurant)

  - Order tracking & status updates

  - Order history & summary

  - Order confirmation notifications (email/SMS)

  - Notify customer on order status update

- Customer Management

  - View order history

  - Track current order status

  - Manage preferred payment settings

  - Address management (multiple addresses)

  - Account deactivation

  - Rating & comments

- Payment Integration

- Dashboard (System & Restaurant)

  - Various statistics and reports on orders, transactions, and restaurants

- Coupon Management

## Non-Functional Requirements

- **Scalability:** The system should support a growing number of users, restaurants, and transactions.

- **Security:** Secure authentication, authorization, and data encryption.

- **Performance:** Ensure quick response times for API calls.

- **Availability:** High uptime and failover mechanisms.

- **Logging & Monitoring:** Track system activity for debugging and analytics.

- **User Experience:** Provide an intuitive and user-friendly interface.

## Constraints

- Payment transactions must comply with financial regulations.

- Restaurant onboarding requires verification.

## Assumptions

- Users have internet access.

- Restaurants manage their own menus.

- Payment providers handle transactions securely.

## Actors

- Client (Customer): Browses, orders, and pays for food.

- Restaurant Staff: Manages restaurant, menus, and orders.

- Courier (Delivery Agent): Picks up and delivers food.

- Payment Provider: Handles payment transactions securely.

## Use Cases

1. User Management

   This use case covers user registration, authentication (login/logout), role-based access control (RBAC) + group roles, profile management, and session handling. It ensures secure user access to the system, including customers, restaurant owners, delivery personnel, and administrators.

2. Restaurant Management

   allows restaurant owners or administrators to add, update, and manage restaurant details, including name, location, working hours, and status (active/inactive). It also includes managing multiple restaurant branches and assigning roles to restaurant staff.

3. Menu & Food Management

   enables restaurants to create and manage their menus, including adding, updating, and deleting food items, setting prices, categorizing items, and managing availability (in stock/out of stock). It also supports food variations (e.g., different sizes, add-ons).

4. Cart Management

   allows users to add, update, and remove food items in their cart. It includes handling guest and authenticated user carts, validating item availability, and updating cart quantities dynamically before proceeding to checkout.

   [place order use case](/use_cases/place_order_usecase.md)

5. Order Management

   handles order placement, tracking, and updates. It includes validating the cart, assigning orders to restaurants, processing different order statuses (pending, confirmed, preparing, ready, delivered, canceled), and sending order notifications to users and restaurants.

6. Payment & Transaction Management

   facilitates payment processing via multiple methods (credit/debit cards, digital wallets, cash on delivery). It includes transaction validation, generating payment URLs, handling payment success/failure, refund processing, and maintaining transaction records.

7. Delivery Management

   manages food delivery by assigning orders to couriers, tracking real-time delivery status, updating estimated delivery times, and notifying users about order dispatch and completion. It also includes handling failed deliveries and reassigning couriers.

8. Reviews & Ratings

   allows users to rate and review restaurants and food items. It includes submitting reviews, moderating content, updating/deleting reviews, and displaying average ratings to help other users make informed choices.

9. Coupons & Discounts

   enables administrators and restaurant owners to create, manage, and apply discount coupons. It includes defining coupon codes, setting discount conditions (percentage/fixed amount, min order value), validating coupon usage, and tracking coupon performance.

10. Dashboard (System / Restaurant)

    provides dashboards for system administrators and restaurant owners. The system dashboard includes user activity, sales reports, and overall platform performance, while the restaurant dashboard shows order statistics, revenue insights, customer feedback, and stock management.

## Data Model

![dataModel](https://drive.google.com/uc?export=view&id=1uWHXNZ9VyzpFwuqhkzwn1xy_XlGzRaqo)

## API Design

please check the following page [API Design](./api_design.md)
