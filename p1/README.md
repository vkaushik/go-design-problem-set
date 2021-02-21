# RDBMS and API design for Zomato/Swiggy

---

## Problem:

Design a relational database for Zomato/Swiggy Application for all their major features: <br>

- Restaurants listing
- Reviews
- Food Delivery
- Orders
- Payments
- Wallet
- Etc.

Please add as much detail as possible and think it thoroughly. Bonus points for API Design as well.

---

## Solution:

# MVP

For the consumers, following should be the minimal feature set to deliver for MVP:

1. View list of nearby restaurant
1. View list of items from each restaurant
1. View/Update Cart
1. View order summary
1. Place Order
1. Track Order

Considering the minimalist versions of these features we may need following tables:

> Restaurant

- id (PK)
- imageBlob
- name
- longitude
- latitude

> FoodItem

- id (PK)
- name
- description
- imageBlob
- price

> Order

- id (PK)
- amount
- delivery_address
- customer_name
- customer_mobile
- customer_email

> OrderItems

- order_id (FK)
- food_item_id (FK)
- quantity

> OrderStatus

- id (PK)
- trackingStatus
- paymentStatus

> RestaurantFoodItems

- restaurant_id (FK)
- food_item_id (FK)

## REST APIs:

1. View list of nearby restaurant

```js
Get: /api/v1/restaurants
ResponseBody:
{
    restaurants: [
        {
            id: "id1"
            name: "no1 hotel",
            image_url: "",
            longitude: "",
            latitude: ""
        },
        ...
    ]
}

```

2. View list of items from each restaurant

```js
Get: /api/v1/restaurants/a34kl/items
ResponseBody:
{
    food_items: [
        {
            id: "",
            name: "",
            description: "",
            image_url: "",
            price: ""
        },
        ...
    ]
}
```

3. View/Update Cart (

```txt
Can be completely managed at client end as in MVP we're not persisting the cart)
```

4. View order summary

```js
POST: /api/v1/order
RequestBody:
{
    food_items: [
        {
            id: "food_item_id",
            qty: 2
        },
        ...
    ]
}

ResponseBody:
{
    order: {
        id: "order_id",
        amount: 1232.23,
        food_items: [
        {
            id: "food_item_id",
            qty: 2
        },
        ...
    ]
    }
}
```

5. Place Order

```js
POST: /api/v1/order/<order_id>
RequestBody:
{
    deliver_address: "",
    customer_name: "",
    customer_mobile: +91-9999999999,
    customer_email: ""
}
```

6. Track Order

```js
GET: /api/v1/order/<order_id>/track
ResponseBody:
{
    status: "accepted"
}
```

<br>

---

Considering the use cases, broadly there are three categories of **Actors** for our Food App (will refer it as Food App instead off Zomato/Swiggy):

- Users / Consumers, do actions like:
  - Browse restaurants and food items
  - Order food and manage cart
  - Do payment
  - Review and Rate items or restaurants
  - Ask questions about a particular restaurant or item
  - Track Order
  - Manager her profile (delivery addresses, passwords, email, wishlist etc.)
  - Manage Wallets
- Restaurant side users / Producers / Suppliers, do actions like:
  - Add/update menu items e.g. price, description, availability, photos etc.
  - Reply to users about their restaurant or item
  - Provide Offers
  - Accept Payments and update order status
  - Manager their profile e.g. employees, users and access.
- Food App Admins / Vendors / Delivery
  - Verify consumers and providers
  - Access control for consumers and providers
  - Handle delivery and tracking
  - Manage app level offers
  - Payments and wallets
  - Use Analytics and suggestion system
  - Customer support

There would be many more use cases than what's listed above. The MVP scope is quite subjective and relies on discussion and mutual consensus. The basic intent of the app is to provide a platform for users to browse and order all the available food options (restricted by availability and deliverability).
Also, provide a platform for sellers to sell their food item.

A single feature can be quite extensive and resource intensive, for example:

> for Search, Filter and Sort, here are some possibilities:

- Restaurant Search by keyword
- Restaurant Filter
  - by rating
  - by vet/non-veg
  - by cuisine
  - by open and closing time
- Restaurant Sort
  - by distance
  - by name
  - by rating
- Item Search by keyword
- Item Filter
  - by veg/non-veg
  - by cuisine
  - by delivery time
  - by price
  - by rating
- Item Sort
  - by price ascending/descending
  - by distance
  - by name
  - by rating

It should be driven by the real market and user research, and business insights.

---
