## Index

- [Index](#index)
  - [Use Case: Place Order](#use-case-place-order)
    - [1. Use Case Name:](#1-use-case-name)
    - [2. Actors](#2-actors)
    - [3. Preconditions](#3-preconditions)
    - [4. Postconditions](#4-postconditions)
    - [5. Main Flow (Happy Path)](#5-main-flow-happy-path)
    - [6. Alternative Flows](#6-alternative-flows)
    - [7. Business Rules](#7-business-rules)
  - [Flow Chart](#flow-chart)
  - [Sequence Diagram](#sequence-diagram)
  - [Pseudo code](#pseudo-code)

### Use Case: Place Order

#### 1. Use Case Name:

Place Order

#### 2. Actors

- **_Client (User):_** A customer using the mobile or web app.
- **_Backend System:_** Manages order processing and interactions with the database.
- **_Database (DB):_** Stores restaurant, cart, order, and user details.
- **_Payment Gateway_**: Processes payments.
- **_Notification Service:_** Sends order-related notifications.
- **_Courier Service:_** Manages order delivery.

#### 3. Preconditions

- The user has accessed the web version.
- The database contains restaurant and product details.
- The system supports guest checkout (for unauthenticated users).

#### 4. Postconditions

- If the order is successful, it is assigned to a courier.
- The user receives notifications about the order status.
- The restaurant is notified about the new order.
- If payment fails, the user is notified, and the order is not processed.

#### 5. Main Flow (Happy Path)

1.  Select a Restaurant

    - The client filters restaurants based on location.
    - The backend fetches restaurant details from the database.
    - The client selects a restaurant.

2.  Add Items to Cart

    - The client adds items to the cart.
    - The backend validates product availability and stock.
    - If the user is authenticated, the system creates or updates the user’s cart.
    - If the user is a guest, the system creates a temporary cart.
    - The backend returns a success response with the cart ID.

3.  Modify Cart (Optional Steps)

    - The client may update item quantity.
    - The backend validates stock and updates the cart.
    - The client may remove items from the cart.
    - The backend processes item removal.

4.  Checkout Process

    - The client proceeds to checkout.
    - If the user is not authenticated, they are prompted to log in or sign up.
    - The backend assigns the cart to the user.
    - The client retrieves saved addresses.
    - The backend calculates subtotal, tax, and shipping costs.
    - The system applies discount coupons (if any).

5.  Payment and Order Placement

    - The client places the order.
    - The backend creates the order and transaction.
    - The backend interacts with the payment gateway to generate a payment URL.
    - The client completes payment via the payment gateway.

6.  Payment and Order Confirmation

    - The payment gateway notifies the backend about payment success.
    - The backend updates the order and transaction status.
    - The system notifies the customer about order confirmation.
    - The system notifies the restaurant about the new order.
    - The order is assigned to a courier for delivery.

#### 6. Alternative Flows

- **_6.1 Payment Failure:_** If the payment fails, the backend updates the order as failed and notifies the customer.

- **_6.2 Guest Checkout Conversion:_** If an unauthenticated user proceeds to checkout, they are redirected to log in or sign up.

#### 7. Business Rules

- A guest user must log in or sign up before completing an order.
- The system ensures product stock validation before adding to the cart.
- Orders cannot be placed without a valid address.
- Payments must be successfully processed before order confirmation.

### Flow Chart

![flowchart](https://drive.google.com/uc?export=view&id=1GsvMB1vqZOaBsnRQHJ1iceHVSVFzsKMV)

### Sequence Diagram

![Sequence](https://drive.google.com/uc?export=view&id=10Jv1OFPMRwTtaDJbSpbLsPuSlUjT9n4Q)

### Pseudo code

```typescript
FUNCTION filterRestaurantsByLocation(location)
    restaurants = getRestaurants(location)  // Fetch restaurants by location
    RETURN restaurants
END FUNCTION

FUNCTION getRestaurantDetails(restaurantId)
    restaurantDetails = fetchRestaurantDetails(restaurantId)  // Fetch restaurant details
    RETURN restaurantDetails
END FUNCTION

FUNCTION addItemToCart(user, cartId, item)
    IF NOT user.isAuthenticated THEN
        cartId = createCartForGuest()  // Create a cart for guest users
    ELSE IF cartId IS NULL THEN
        cartId = createCartForUser(user)  // Create a cart for authenticated users
    END IF

    IF validateProductAvailability(item) THEN
        addItem(cartId, item)
        RETURN { success: true, cartId: cartId }
    ELSE
        RETURN { success: false, message: "Item not available" }
    END IF
END FUNCTION

FUNCTION updateItemQuantity(cartId, itemId, quantity)
    IF validateProductAvailability(itemId, quantity) THEN
        updateCartItem(cartId, itemId, quantity)
        RETURN { success: true }
    ELSE
        RETURN { success: false, message: "Item out of stock" }
    END IF
END FUNCTION

FUNCTION deleteItemFromCart(cartId, itemId)
    removeCartItem(cartId, itemId)
    RETURN { success: true }
END FUNCTION

FUNCTION getUserAddresses(user)
    addressList = getUserAddresses(user)
    RETURN { addresses: addressList }
END FUNCTION

FUNCTION calculateTotal(cartId, address)
    subTotal = calculateSubTotal(cartId)
    tax = calculateTax(subTotal)
    shippingCost = calculateShippingCost(address)
    total = subTotal + tax + shippingCost
    RETURN { total, subTotal, tax, shippingCost }
END FUNCTION

FUNCTION applyDiscount(cartId, couponCode)
    IF validateCoupon(couponCode) THEN
        applyCoupon(cartId, couponCode)
        RETURN { success: true, discountApplied: true }
    ELSE
        RETURN { success: false, message: "Invalid coupon" }
    END IF
END FUNCTION

FUNCTION placeOrder(user, cartId, paymentMethod)
    orderId = createOrder(user, cartId)  // Create order and transaction

    paymentUrl = processPayment(orderId, paymentMethod)  // Generate payment link
    RETURN { paymentUrl }
END FUNCTION

FUNCTION handlePaymentCallback(orderId, status)
    IF status == "failed" THEN
        updateOrderStatus(orderId, "failed")
        updateTransactionStatus(orderId, "failed")
        notifyCustomer(orderId, "Payment failed")
    ELSE IF status == "success" THEN
        updateOrderStatus(orderId, "paid")
        updateTransactionStatus(orderId, "processing")
        notifyCustomer(orderId, "Order confirmed")
        notifyRestaurant(orderId, "New order placed")
        assignOrderToCourier(orderId)
    END IF
END FUNCTION
```
