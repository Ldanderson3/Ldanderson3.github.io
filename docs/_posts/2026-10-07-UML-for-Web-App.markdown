---
layout: post
title:  "UML for Web App"
date:   2026-10-06 01:18:00 -0600
categories: UML
---

## What is UML?
UML stands for Unified modeling language, and is commonly used by software engineers to layout the program in a visual way, showing all aspects of the software they will be developing.

## UML for web app
The UML diagram is below:
![UML Diagram](https://raw.githubusercontent.com/Ldanderson3/Ldanderson3.github.io/blob/main/docs/_posts/UML-Diagram-final.png)

Below is the UML code for the diagram:
```
 ============================================================
' Inteliwatches - Smartwatch E-Commerce Website
' PlantUML script containing 6 diagrams (render each @startuml block)
' ============================================================

' ------------------------------------------------------------
' 1. USE CASE DIAGRAM
' ------------------------------------------------------------
@startuml UseCase_Inteliwatches
left to right direction
skinparam packageStyle rectangle

actor "Guest" as Guest
actor "Customer" as Customer
actor "Admin" as Admin
actor "Support Agent" as Support
actor "Payment Gateway" as Pay <<external>>
actor "Shipping Carrier" as Ship <<external>>
actor "Email Service" as Email <<external>>

Customer --|> Guest

rectangle "Inteliwatches Website" {
  usecase "Browse Catalog" as UC1
  usecase "Search Products" as UC2
  usecase "Filter / Sort Products" as UC3
  usecase "View Product Details" as UC4
  usecase "Compare Watches" as UC5
  usecase "Register Account" as UC6
  usecase "Log In / Log Out" as UC7
  usecase "Reset Password" as UC8
  usecase "Manage Shopping Cart" as UC9
  usecase "Manage Wishlist" as UC10
  usecase "Checkout" as UC11
  usecase "Apply Promo Code" as UC12
  usecase "Make Payment" as UC13
  usecase "Track Order" as UC14
  usecase "Request Return / Refund" as UC15
  usecase "Write Product Review" as UC16
  usecase "Manage Profile & Addresses" as UC17
  usecase "Contact Support / Live Chat" as UC18
  usecase "Manage Products & Inventory" as UC19
  usecase "Manage Orders" as UC20
  usecase "Manage Promotions" as UC21
  usecase "Moderate Reviews" as UC22
  usecase "View Sales Reports" as UC23
  usecase "Manage Users" as UC24
  usecase "Handle Support Tickets" as UC25
  usecase "Send Notifications" as UC26
}

Guest --> UC1
Guest --> UC2
Guest --> UC4
Guest --> UC5
Guest --> UC6
Guest --> UC7
Guest --> UC9
Guest --> UC18

Customer --> UC8
Customer --> UC10
Customer --> UC11
Customer --> UC14
Customer --> UC15
Customer --> UC16
Customer --> UC17

UC1 ..> UC3 : <<extend>>
UC2 ..> UC3 : <<extend>>
UC11 ..> UC7 : <<include>>
UC11 ..> UC13 : <<include>>
UC11 ..> UC12 : <<extend>>
UC13 --> Pay
UC14 --> Ship
UC20 --> Ship
UC26 --> Email
UC11 ..> UC26 : <<include>>
UC15 ..> UC26 : <<include>>

Admin --> UC19
Admin --> UC20
Admin --> UC21
Admin --> UC22
Admin --> UC23
Admin --> UC24
Support --> UC25
Support --> UC18
@enduml


' ------------------------------------------------------------
' 2. CLASS DIAGRAM (Domain Model)
' ------------------------------------------------------------
@startuml Class_Inteliwatches
skinparam classAttributeIconSize 0
skinparam linetype ortho

enum Role {
  GUEST
  CUSTOMER
  SUPPORT
  ADMIN
}

enum OrderStatus {
  PENDING
  PAID
  PROCESSING
  SHIPPED
  DELIVERED
  CANCELLED
  RETURN_REQUESTED
  REFUNDED
}

enum PaymentMethod {
  CREDIT_CARD
  DEBIT_CARD
  PAYPAL
  APPLE_PAY
  GOOGLE_PAY
}

enum PaymentStatus {
  INITIATED
  AUTHORIZED
  CAPTURED
  FAILED
  REFUNDED
}

enum DiscountType {
  PERCENTAGE
  FIXED_AMOUNT
  FREE_SHIPPING
}

enum TicketStatus {
  OPEN
  IN_PROGRESS
  RESOLVED
  CLOSED
}

class User {
  - userId : UUID
  - firstName : String
  - lastName : String
  - email : String
  - passwordHash : String
  - phone : String
  - role : Role
  - isVerified : boolean
  - createdAt : DateTime
  - lastLogin : DateTime
  + register() : boolean
  + login(email: String, password: String) : Session
  + logout() : void
  + resetPassword(email: String) : void
  + updateProfile(data: Map) : void
}

class Customer {
  - loyaltyPoints : int
  + addAddress(a: Address) : void
  + placeOrder(cart: Cart) : Order
  + viewOrderHistory() : List<Order>
  + writeReview(p: Product, r: Review) : void
  + requestReturn(o: Order, reason: String) : ReturnRequest
}

class Admin {
  + addProduct(p: Product) : void
  + updateProduct(p: Product) : void
  + removeProduct(productId: UUID) : void
  + updateOrderStatus(o: Order, s: OrderStatus) : void
  + createPromotion(p: Promotion) : void
  + moderateReview(r: Review, approve: boolean) : void
  + generateReport(range: DateRange) : Report
}

class SupportAgent {
  + replyToTicket(t: SupportTicket, msg: String) : void
  + closeTicket(t: SupportTicket) : void
}

class Address {
  - addressId : UUID
  - street : String
  - city : String
  - state : String
  - postalCode : String
  - country : String
  - isDefault : boolean
}

class Category {
  - categoryId : UUID
  - name : String
  - description : String
}

class Brand {
  - brandId : UUID
  - name : String
  - logoUrl : String
}

class Product {
  - productId : UUID
  - sku : String
  - name : String
  - description : String
  - price : Decimal
  - stockQuantity : int
  - isActive : boolean
  - averageRating : float
  + isInStock() : boolean
  + applyDiscount(pct: float) : Decimal
  + updateStock(delta: int) : void
}

class SmartWatch {
  - displaySize : float
  - batteryLifeHours : int
  - waterResistanceRating : String
  - hasGPS : boolean
  - hasHeartRateSensor : boolean
  - hasECG : boolean
  - hasLTE : boolean
  - operatingSystem : String
  - strapMaterial : String
  - colorOptions : List<String>
}

class ProductImage {
  - imageId : UUID
  - url : String
  - altText : String
  - sortOrder : int
}

class Review {
  - reviewId : UUID
  - rating : int
  - title : String
  - body : String
  - isApproved : boolean
  - createdAt : DateTime
}

class Cart {
  - cartId : UUID
  - createdAt : DateTime
  - updatedAt : DateTime
  + addItem(p: Product, qty: int) : void
  + removeItem(p: Product) : void
  + updateQuantity(p: Product, qty: int) : void
  + applyPromo(code: String) : boolean
  + calculateSubtotal() : Decimal
  + calculateTotal() : Decimal
  + clear() : void
}

class CartItem {
  - quantity : int
  - unitPrice : Decimal
  + lineTotal() : Decimal
}

class Wishlist {
  - wishlistId : UUID
  + add(p: Product) : void
  + remove(p: Product) : void
}

class Order {
  - orderId : UUID
  - orderNumber : String
  - status : OrderStatus
  - subtotal : Decimal
  - tax : Decimal
  - shippingCost : Decimal
  - discountTotal : Decimal
  - grandTotal : Decimal
  - placedAt : DateTime
  + calculateTotal() : Decimal
  + cancel() : boolean
  + updateStatus(s: OrderStatus) : void
}

class OrderItem {
  - quantity : int
  - unitPrice : Decimal
  + lineTotal() : Decimal
}

class Payment {
  - paymentId : UUID
  - method : PaymentMethod
  - status : PaymentStatus
  - amount : Decimal
  - transactionRef : String
  - paidAt : DateTime
  + authorize() : boolean
  + capture() : boolean
  + refund(amount: Decimal) : boolean
}

class Shipment {
  - shipmentId : UUID
  - carrier : String
  - trackingNumber : String
  - shippedAt : DateTime
  - estimatedDelivery : Date
  - deliveredAt : DateTime
  + getTrackingInfo() : String
}

class Promotion {
  - promoId : UUID
  - code : String
  - type : DiscountType
  - value : Decimal
  - startDate : Date
  - endDate : Date
  - usageLimit : int
  - timesUsed : int
  + isValid() : boolean
  + calculateDiscount(subtotal: Decimal) : Decimal
}

class ReturnRequest {
  - returnId : UUID
  - reason : String
  - status : String
  - requestedAt : DateTime
  + approve() : void
  + reject(reason: String) : void
}

class SupportTicket {
  - ticketId : UUID
  - subject : String
  - status : TicketStatus
  - createdAt : DateTime
}

class TicketMessage {
  - messageId : UUID
  - body : String
  - sentAt : DateTime
}

class Notification {
  - notificationId : UUID
  - type : String
  - message : String
  - isRead : boolean
  - sentAt : DateTime
}

User <|-- Customer
User <|-- Admin
User <|-- SupportAgent
User --> Role
Product <|-- SmartWatch

Customer "1" *-- "0..*" Address
Customer "1" -- "0..1" Cart
Customer "1" -- "0..1" Wishlist
Customer "1" -- "0..*" Order : places >
Customer "1" -- "0..*" Review : writes >
Customer "1" -- "0..*" SupportTicket : opens >
User "1" -- "0..*" Notification : receives >

Cart "1" *-- "0..*" CartItem
CartItem "0..*" --> "1" Product
Wishlist "0..*" o-- "0..*" Product

Order "1" *-- "1..*" OrderItem
OrderItem "0..*" --> "1" Product
Order "1" -- "1" Payment
Order "1" -- "0..1" Shipment
Order "1" --> "1" Address : ships to
Order --> OrderStatus
Order "0..*" -- "0..1" Promotion : uses >
Order "1" -- "0..*" ReturnRequest
Payment --> PaymentMethod
Payment --> PaymentStatus
Promotion --> DiscountType

Product "0..*" --> "1" Category
Product "0..*" --> "1" Brand
Product "1" *-- "1..*" ProductImage
Product "1" -- "0..*" Review
Category "1" o-- "0..*" Category : subcategories

SupportTicket "1" *-- "1..*" TicketMessage
SupportTicket --> TicketStatus
SupportAgent "1" -- "0..*" SupportTicket : handles >
@enduml


' ------------------------------------------------------------
' 3. SEQUENCE DIAGRAM: Checkout & Payment
' ------------------------------------------------------------
@startuml Sequence_Checkout
title Checkout and Payment Flow
autonumber

actor Customer
participant "Web UI\n(Browser)" as UI
participant "API Gateway" as API
participant "AuthService" as Auth
participant "CartService" as Cart
participant "InventoryService" as Inv
participant "OrderService" as Ord
participant "PaymentService" as PaySvc
participant "Payment Gateway\n(External)" as PG
participant "NotificationService" as Notif
database "Database" as DB

Customer -> UI : Click "Checkout"
UI -> API : POST /checkout/start (JWT)
API -> Auth : validateToken(jwt)
Auth --> API : valid(userId)
API -> Cart : getCart(userId)
Cart -> DB : SELECT cart + items
DB --> Cart : cart data
Cart --> API : cart
API -> Inv : checkStock(cart.items)
Inv -> DB : SELECT stock
DB --> Inv : quantities

alt Item out of stock
  Inv --> API : insufficientStock(items)
  API --> UI : 409 Conflict
  UI --> Customer : "Some items are unavailable"
else All items in stock
  Inv --> API : stockOK
  API --> UI : checkout summary
  Customer -> UI : Enter shipping address, shipping method, promo code
  UI -> API : POST /checkout/review
  API -> Cart : applyPromo(code)
  Cart --> API : updated totals
  API --> UI : order review (subtotal, tax, shipping, total)
  Customer -> UI : Enter payment details, click "Place Order"
  UI -> API : POST /orders
  API -> Ord : createOrder(userId, cart, address)
  Ord -> Inv : reserveStock(items)
  Inv -> DB : UPDATE reserved qty
  Ord -> DB : INSERT order (status = PENDING)
  Ord -> PaySvc : charge(orderId, amount, token)
  PaySvc -> PG : authorizeAndCapture(token, amount)

  alt Payment approved
    PG --> PaySvc : approved(txnRef)
    PaySvc -> DB : INSERT payment (CAPTURED)
    PaySvc --> Ord : paymentSuccess
    Ord -> DB : UPDATE order (status = PAID)
    Ord -> Inv : commitStock(items)
    Ord -> Cart : clearCart(userId)
    Ord -> Notif : sendOrderConfirmation(orderId)
    Notif --> Customer : Confirmation email
    Ord --> API : order created
    API --> UI : 201 Created (orderNumber)
    UI --> Customer : Order confirmation page
  else Payment declined
    PG --> PaySvc : declined(reason)
    PaySvc -> DB : INSERT payment (FAILED)
    PaySvc --> Ord : paymentFailed
    Ord -> Inv : releaseStock(items)
    Ord -> DB : UPDATE order (status = CANCELLED)
    Ord --> API : error
    API --> UI : 402 Payment Required
    UI --> Customer : "Payment failed, try another method"
  end
end
@enduml


' ------------------------------------------------------------
' 4. ACTIVITY DIAGRAM: Shopping Journey
' ------------------------------------------------------------
@startuml Activity_Shopping
title Customer Shopping Journey
start
:Visit Inteliwatches homepage;
:Browse catalog or search for a smartwatch;
:Apply filters (brand, price, features, rating);
:Open product detail page;

if (Compare with other watches?) then (yes)
  :Add to comparison table;
  :Review side-by-side specs;
else (no)
endif

if (Ready to buy?) then (yes)
  :Select color / strap / size;
  :Add to cart;
else (not yet)
  if (Logged in?) then (yes)
    :Add to wishlist;
  else (no)
    :Prompt to log in or register;
  endif
  stop
endif

:Open shopping cart;
:Update quantities / remove items;

if (Have promo code?) then (yes)
  :Enter promo code;
  if (Code valid?) then (yes)
    :Discount applied;
  else (no)
    :Show error message;
  endif
endif

:Click Checkout;

if (Authenticated?) then (no)
  fork
    :Log in;
  fork again
    :Register new account;
  end fork
endif

:Enter / select shipping address;
:Choose shipping method;
:Enter payment information;
:Review order;
:Place order;

if (Payment successful?) then (yes)
  :Show confirmation page;
  :Send confirmation email;
  fork
    :Warehouse picks and packs order;
  fork again
    :Update inventory;
  end fork
  :Ship order and send tracking number;
  :Customer receives watch;
  if (Satisfied?) then (yes)
    :Write a review;
  else (no)
    :Request return / refund;
  endif
else (no)
  :Show payment error;
  :Retry with another method;
endif
stop
@enduml


' ------------------------------------------------------------
' 5. STATE MACHINE DIAGRAM: Order Lifecycle
' ------------------------------------------------------------
@startuml State_Order
title Order State Lifecycle
[*] --> PENDING : order created

PENDING --> PAID : payment captured
PENDING --> CANCELLED : payment failed / timeout / user cancels

PAID --> PROCESSING : warehouse accepts
PAID --> CANCELLED : cancelled before processing\n(refund issued)

PROCESSING --> SHIPPED : carrier pickup\n(tracking number assigned)
PROCESSING --> CANCELLED : stock issue\n(refund issued)

SHIPPED --> DELIVERED : carrier confirms delivery
SHIPPED --> PROCESSING : delivery failed / returned to sender

DELIVERED --> RETURN_REQUESTED : customer requests return\n(within 30 days)
DELIVERED --> [*] : return window expires

RETURN_REQUESTED --> REFUNDED : return approved & received
RETURN_REQUESTED --> DELIVERED : return rejected
```
