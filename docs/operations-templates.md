# Coastal Theory Operations Templates

## Google Sheets Workbook

Create one workbook named `Coastal Theory MVP Ops` with these tabs:

1. Customers
2. Weekly Catch
3. Orders
4. Order Items
5. Payments
6. Suppliers
7. Procurement
8. Delivery Routes
9. Feedback

## Weekly Catch Template

| catch_id | product | city | source | price | min_order | cutoff | delivery_date | status | notes |
|---|---|---|---|---:|---:|---|---|---|---|
| WC-001 | Tiger Prawns | Both | Gopalpur | 0 | 500g | Thursday 8 PM | Sunday | Draft | Confirm size before launch |

## Order Tracker Template

| order_id | customer | phone | city | locality | product | qty | cleaning | total | advance | payment_status | order_status | delivery_date | notes |
|---|---|---|---|---|---|---:|---|---:|---:|---|---|---|---|
| CT-0001 |  |  | Bhubaneswar |  |  |  |  |  |  | Pending | Enquiry |  |  |

## Procurement Summary Template

| delivery_date | city | product | total_ordered_qty | buffer_qty | supplier | expected_cost | actual_cost | status | quality_notes |
|---|---|---|---:|---:|---|---:|---:|---|---|
|  |  |  |  |  |  |  |  | Pending |  |

## Delivery Route Template

| route_id | date | city | locality_cluster | order_ids | driver | status | notes |
|---|---|---|---|---|---|---|---|
| R-001 |  | Bhubaneswar | Patia |  |  | Planned |  |

## Feedback Template

| order_id | customer | freshness_rating | quality_rating | delivery_rating | testimonial | issues | reorder_interest |
|---|---|---:|---:|---:|---|---|---|
|  |  |  |  |  |  |  |  |

## Order ID Convention

Use sequential IDs:

```text
CT-0001
CT-0002
CT-0003
```

## Status Values

### Order Status

- Enquiry
- Confirmed
- Paid
- Procured
- Packed
- Out for Delivery
- Delivered
- Cancelled

### Payment Status

- Pending
- Partial
- Paid
- Refunded

### Weekly Catch Status

- Draft
- Live
- Limited
- Sold Out
- Closed

## Refund / Replacement Policy Draft

If a confirmed catch is unavailable at procurement time, Coastal Theory will offer one of the following:

1. Replacement with a similar product and adjusted price.
2. Delivery in the next weekly drop.
3. Full refund of the advance paid.

## Packing Checklist

- Verify customer order ID.
- Verify product and quantity.
- Verify cleaning/cutting preference.
- Label package with order ID and customer name.
- Separate city and route clusters.
- Keep payment balance note if cash balance is pending.

## Delivery Checklist

- Send packing update.
- Send out-for-delivery update.
- Confirm address and phone.
- Collect balance if needed.
- Mark order as delivered.
- Send feedback message within 6 hours.
