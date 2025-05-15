Shopify Theme Customizations - README
Overview
This document outlines the implementation details for various Shopify theme customizations including dynamic product badges, collection filtering, personalized greetings, cart upsells, and free shipping messages.

Task 1: Dynamic Product Badge
File
snippets/product-card.liquid

Logic
Price Check:

Displays "Budget Pick" badge if product price < ₹500 (50000 cents)

liquid
Copy
Download
{% if card_product.price < 50000 %}
Stock Check:

Displays "Limited Stock" badge if inventory < 5 and > 0

liquid
Copy
Download
{% if first_variant.inventory_quantity < 5 and first_variant.inventory_quantity > 0 %}
Positioning:

Uses absolute positioning to overlay badges on product image

Badges stack vertically when both conditions apply

Visual States
Condition	Badge Displayed
Price < ₹500	Budget Pick (green)
Stock < 5	Limited Stock (yellow)
Both conditions	Both badges
Task 2: Custom Collection Filter
File
sections/main-collection-product-grid.liquid

Logic
Dropdown Filter:

Uses URL parameters to filter products by tag

liquid
Copy
Download
onchange="if(this.value) window.location.href=this.value;"
Tag Options:

best-seller

new-arrival

discounted

Current Selection:

Maintains selected state using current_tags

liquid
Copy
Download
{% if current_tags contains 'best-seller' %}selected{% endif %}
Flow
User selects option from dropdown

Page reloads with filtered results

Selected option remains highlighted

Task 3: Personalized Greeting
File
templates/index.liquid

Logic
Time Detection:

Uses client-side JavaScript to get local hour

javascript
Copy
Download
const hour = new Date().getHours();
Greeting Rules:

Time Range	Greeting
00:00-11:59	Good Morning
12:00-12:00	Good Noon
12:01-17:59	Good Afternoon
18:00-23:59	Good Evening
Dynamic Update:

Updates on page load

Uses DOM manipulation to insert greeting

Task 4: Cart Upsell Section
File
sections/main-cart-items.liquid

Logic
Cart Value Check:

liquid
Copy
Download
{% if cart_total < 100000 %}  <!-- ₹1000 threshold -->
Product Selection:

< ₹1000: First product from Accessories collection

≥ ₹1000: First product from Premium collection

AJAX Add to Cart:

javascript
Copy
Download
fetch('/cart/add.js', { method: 'POST'... })
Display Logic
Shows product image, title, price

Includes "Add to Cart" button

Only displays if upsell product exists

Bonus: Free Shipping Message
Logic
Threshold Check:

liquid
Copy
Download
{% if cart_total < 500 %}
Dynamic Calculation:

Calculates remaining amount needed

liquid
Copy
Download
{% assign remaining = 500 | minus: cart_total %}
Visual Feedback:

Progress bar shows completion percentage

Celebration emoji when threshold met

Messaging
Cart Value	Message
< ₹500	"Spend ₹X more..."
≥ ₹500	"You've unlocked..."
Implementation Notes
Currency Handling:

All prices stored in cents (₹1 = 100 cents)

Divided by 100 for display purposes

Responsive Design:

All components work on mobile/desktop

Uses flexible units (em, %) for scaling

Performance:

Client-side calculations where possible

Minimal additional database queries

Error Handling:

Fallbacks for missing collections/products

Graceful degradation if JavaScript disabled

Testing Checklist
Product Badges:

Verify badge appears at correct price points

Test with various inventory levels

Check mobile display

Collection Filter:

Test each filter option

Verify pagination works

Check selected state persists

Greeting:

Test at different times of day

Verify timezone handling

Check loading state

Cart Upsells:

Test below/above threshold

Verify add-to-cart functionality

Check empty collection handling
