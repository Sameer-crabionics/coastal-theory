# Coastal Theory MVP Architecture

## 1. Product Vision

Coastal Theory is a premium, trust-led seafood discovery and pre-order platform for Odisha.

The MVP should behave like a curated weekly seafood drop, not a full seafood marketplace.

### Mission

Help customers discover and pre-order high-quality seafood directly from trusted coastal sources.

### Initial Cities

- Bhubaneswar
- Berhampur

### Validation Goal

Acquire the first 20 paying customers without inventory risk.

### MVP Promise

Customers can discover this week's catch, reserve seafood on WhatsApp, pay an advance or full amount, and receive delivery on a fixed day.

---

## 2. User Journey

### Discovery

Customers discover Coastal Theory through Instagram Reels, Stories, posts, referrals, WhatsApp shares, and local community groups.

### Interest

Customers visit the Instagram profile, WhatsApp catalogue, landing page, or Weekly Catch page to understand availability, price, source, delivery day, and freshness.

### Trust Building

Trust is created through source location, product photos, founder-led communication, limited weekly slots, delivery clarity, and early testimonials.

### Pre-Order

The customer clicks an `Order on WhatsApp` call to action and sends a pre-filled enquiry.

### Confirmation

The admin confirms product, quantity, city, locality, cleaning option, price, delivery date, and payment requirement.

### Fulfilment

After order cutoff, Coastal Theory procures confirmed quantities, packs customer orders, and delivers on the fixed delivery day.

### Retention

After delivery, the customer receives a feedback message and early access to the next weekly drop.

---

## 3. Sitemap

### Public Pages

1. Home / Landing Page
2. Weekly Catch
3. How It Works
4. About Coastal Theory
5. FAQ
6. Contact / WhatsApp

### Optional Later Pages

1. Thank You page
2. Customer Reviews page

---

## 4. Information Architecture

### Primary Navigation

- Weekly Catch
- How It Works
- About
- FAQ
- Order on WhatsApp

### Content Priority

1. Available seafood
2. Price and quantity
3. Delivery day
4. City availability
5. WhatsApp order button
6. Source and freshness information
7. Payment, cancellation, and delivery rules

---

## 5. Database Schema

For the first 20 customers, Google Sheets or Airtable is sufficient.

### `customers`

| Field | Type | Notes |
|---|---|---|
| customer_id | Unique ID | Manual or generated ID |
| name | Text | Customer name |
| phone | Text | WhatsApp number |
| city | Enum | Bhubaneswar, Berhampur |
| locality | Text | Area or neighborhood |
| full_address | Text | Required after order confirmation |
| customer_type | Enum | New, Repeat, VIP |
| acquisition_source | Enum | Instagram, WhatsApp, Referral, Other |
| referral_by | Text | Referrer details |
| notes | Text | Preferences or service notes |
| created_at | DateTime | First contact date |

### `products`

| Field | Type | Notes |
|---|---|---|
| product_id | Unique ID | Product identifier |
| name | Text | Product name |
| local_name | Text | Local/Odia market name |
| category | Enum | Fish, Prawn, Crab, Shellfish, Dry Fish |
| description | Text | Customer-facing description |
| image_url | URL | Product image |
| unit | Enum | kg, piece, box |
| cleaning_available | Boolean | Cleaning availability |
| cutting_available | Boolean | Cutting availability |
| active | Boolean | Whether available this week |

### `weekly_catch`

| Field | Type | Notes |
|---|---|---|
| catch_id | Unique ID | Weekly item identifier |
| week_start_date | Date | Week reference |
| product_id | Foreign Key | Product reference |
| city | Enum | Bhubaneswar, Berhampur, Both |
| source_location | Text | Coastal source |
| price_per_unit | Number | Selling price |
| estimated_procurement_price | Number | Internal cost estimate |
| available_quantity | Number | Estimated supply |
| min_order_quantity | Number | Minimum order |
| order_cutoff | DateTime | Pre-order deadline |
| delivery_date | Date | Delivery date |
| status | Enum | Draft, Live, Sold Out, Closed |
| display_priority | Number | Sort order |
| notes | Text | Internal notes |

### `orders`

| Field | Type | Notes |
|---|---|---|
| order_id | Unique ID | Example: CT-0001 |
| customer_id | Foreign Key | Customer reference |
| city | Enum | Bhubaneswar, Berhampur |
| order_status | Enum | Enquiry, Confirmed, Paid, Procured, Packed, Out for Delivery, Delivered, Cancelled |
| order_date | DateTime | Order date |
| delivery_date | Date | Fixed delivery date |
| delivery_slot | Text | Delivery window |
| subtotal | Number | Product total |
| delivery_fee | Number | Delivery fee |
| discount | Number | Discount amount |
| total_amount | Number | Final amount |
| advance_paid | Number | Advance amount |
| payment_status | Enum | Pending, Partial, Paid, Refunded |
| payment_method | Enum | UPI, Cash, Bank Transfer |
| whatsapp_thread_link | Text | Optional reference |
| internal_notes | Text | Operational notes |

### `order_items`

| Field | Type | Notes |
|---|---|---|
| order_item_id | Unique ID | Unique row |
| order_id | Foreign Key | Order reference |
| catch_id | Foreign Key | Weekly catch reference |
| product_name_snapshot | Text | Product name at order time |
| quantity | Number | Ordered quantity |
| unit | Text | kg or piece |
| price_per_unit | Number | Snapshot price |
| line_total | Number | Quantity × price |
| cleaning_option | Enum | Whole, Cleaned, Cut, Deveined |
| special_instruction | Text | Customer request |

### `suppliers`

| Field | Type | Notes |
|---|---|---|
| supplier_id | Unique ID | Supplier identifier |
| name | Text | Supplier name |
| phone | Text | Internal contact |
| source_location | Text | Coastal location |
| product_categories | Text | Product categories |
| reliability_score | Number | Internal 1-5 score |
| payment_terms | Text | Payment terms |
| notes | Text | Quality notes |

### `procurement`

| Field | Type | Notes |
|---|---|---|
| procurement_id | Unique ID | Procurement row |
| delivery_date | Date | Fulfilment date |
| city | Enum | Bhubaneswar, Berhampur |
| product_id | Foreign Key | Product reference |
| total_ordered_quantity | Number | Sum of confirmed orders |
| buffer_quantity | Number | Optional buffer |
| supplier_id | Foreign Key | Supplier reference |
| expected_cost | Number | Estimated cost |
| actual_cost | Number | Actual cost |
| procurement_status | Enum | Pending, Confirmed, Purchased, Delivered to Packing |
| quality_notes | Text | Internal notes |

### `payments`

| Field | Type | Notes |
|---|---|---|
| payment_id | Unique ID | Payment identifier |
| order_id | Foreign Key | Order reference |
| customer_id | Foreign Key | Customer reference |
| amount | Number | Payment amount |
| method | Enum | UPI, Cash, Bank Transfer |
| payment_status | Enum | Pending, Verified, Failed, Refunded |
| transaction_reference | Text | UPI or bank reference |
| screenshot_url | URL | Optional proof |
| received_at | DateTime | Payment time |

### `delivery_routes`

| Field | Type | Notes |
|---|---|---|
| route_id | Unique ID | Route identifier |
| delivery_date | Date | Delivery day |
| city | Enum | Bhubaneswar, Berhampur |
| locality_cluster | Text | Area cluster |
| order_ids | Text | Related orders |
| delivery_partner | Text | Assigned person/vendor |
| route_status | Enum | Planned, In Progress, Completed |
| notes | Text | Delivery notes |

---

## 6. Admin Workflow

### Monday: Demand Creation

- Publish Instagram Reel and Stories.
- Update the Weekly Catch page.
- Broadcast the weekly catch to interested customers.
- Collect poll responses and DMs.

### Tuesday-Wednesday: Pre-Order Collection

- Capture customer name, phone, city, locality, product, quantity, and cleaning preference.
- Share price and delivery date.
- Collect advance or full payment.
- Mark paid orders as confirmed.

### Thursday: Order Cutoff

- Close orders.
- Summarize required procurement quantities.
- Confirm supplier availability.
- Refund or replace if minimum sourcing quantity is not viable.

### Friday-Sunday: Procurement, Packing, Delivery

- Procure confirmed quantities only.
- Quality check and pack by order.
- Label packages.
- Deliver by city/locality route.
- Mark delivery status.

### Post-Delivery

- Collect freshness rating.
- Request testimonial or photo.
- Add customer to repeat list.
- Tease next drop.

---

## 7. Customer Workflow

1. Customer discovers Coastal Theory on Instagram or WhatsApp.
2. Customer opens Weekly Catch page.
3. Customer clicks `Order on WhatsApp`.
4. Customer shares city, product, quantity, and locality.
5. Admin confirms availability and delivery date.
6. Customer pays advance or full amount.
7. Admin confirms order ID.
8. Customer receives procurement and delivery updates.
9. Customer receives order.
10. Customer shares feedback and is invited to the next weekly drop.

---

## 8. Mobile Wireframes

### Home Page

```text
Coastal Theory
Premium seafood pre-orders for Odisha
[Order on WhatsApp]

This Week's Coastal Catch
[Product Card]
Image
Product name
Source
Price
Delivery date
[Pre-order on WhatsApp]

[View Full Weekly Catch]

How It Works
1. Discover this week's catch
2. Pre-order on WhatsApp
3. We source after confirmed orders
4. Fresh seafood delivered on fixed day

Why Pre-Order?
No stale inventory. Less waste. Better quality planning.

Serving
Bhubaneswar | Berhampur

[Order This Week's Catch]
```

### Weekly Catch Page

```text
This Week's Catch
Pre-order closes: Thursday, 8 PM
Delivery: Saturday/Sunday

[City Selector]
Bhubaneswar | Berhampur

[Catch Card]
Image
Name
Source
Price
Minimum order
Cleaning option
Availability status
[Reserve on WhatsApp]

Order Rules
- Orders close Thursday
- Advance payment confirms order
- Replacement/refund if unavailable

[Sticky WhatsApp CTA]
```

---

## 9. Landing Page Structure

1. Hero with positioning and WhatsApp CTA
2. Weekly Catch preview
3. Four-step How It Works section
4. Why Pre-Order section
5. Trust and sourcing section
6. Cities served
7. FAQ
8. Final WhatsApp and Instagram CTA

---

## 10. Weekly Catch Page Structure

1. Page headline
2. Order cutoff and delivery date
3. City selector
4. Product cards
5. Order rules
6. Quality promise
7. Sticky WhatsApp CTA

---

## 11. WhatsApp Ordering Flow

### Entry Message

```text
Hi Coastal Theory, I want to pre-order from this week's catch.

City:
Product:
Quantity:
Delivery locality:
```

### Order Summary

```text
Order Summary

Name:
City:
Locality:
Product:
Quantity:
Cleaning:
Delivery date:
Subtotal:
Delivery fee:
Total:

To confirm your order, please pay ₹___ advance via UPI.
UPI ID: ______
```

### Confirmation

```text
Thank you. Your Coastal Theory pre-order is confirmed.

Order ID: CT-0001
Product:
Quantity:
Delivery date:
Balance due, if any:

We'll update you once the catch is sourced and packed.
```

---

## 12. Deployment Architecture

### Recommended MVP Stack

| Function | Tool |
|---|---|
| Website | Carrd, Framer, or Webflow |
| Ordering | WhatsApp Business |
| Database | Google Sheets or Airtable |
| Payments | UPI |
| Design | Canva |
| Analytics | Instagram Insights and Google Sheets |
| Delivery | Manual route clustering with Google Maps |

### Architecture

```text
Instagram
  -> Landing Page / Weekly Catch Page
  -> WhatsApp Business
  -> Manual Admin Confirmation
  -> Google Sheets / Airtable
  -> Supplier Procurement
  -> Packing + Delivery
  -> Customer Feedback + Repeat Broadcast
```

---

## 13. MVP Scope

### Must-Have

- Instagram page
- WhatsApp Business account
- Landing page
- Weekly Catch page
- Google Sheet order tracker
- UPI payment process
- Manual order confirmation
- Fixed delivery day
- City-specific delivery rules
- Feedback loop

### Success Metrics

| Funnel Step | Target |
|---|---|
| Instagram profile visits | 300+ |
| WhatsApp enquiries | 50 |
| Confirmed orders | 25 |
| Paid customers | 20 |
| Repeat intent | 30%+ |

---

## 14. Features to Avoid

Avoid building these before validation:

- Full e-commerce checkout
- Customer login
- Real-time inventory
- Daily delivery
- Large product catalogue
- Multiple payment gateways
- Mobile app
- Complex delivery software
- Supplier marketplace
- Automated chatbot
