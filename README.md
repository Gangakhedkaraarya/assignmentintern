# assignmentintern
Task 1: Dynamic Product Badge
 Modify the product-card.liquid file to display a custom badge based on the product’s 
price and stock status:
 • If a product is under ₹500, show a "Budget Pick" badge.
 • If the product has fewer than 5 items in stock, show a "Limited Stock" badge.
 • If both conditions apply, show both badges.
 Hint: Use product.price and 
product.variants.first.inventory_quantity
Overview
This implementation adds dynamic badges to product cards in a Shopify store using Liquid templating. The badges appear based on product price and inventory conditions.

Logic Explanation
1. Price Badge ("Budget Pick")
liquid
Copy
Download
{% if card_product.price < 50000 %}
  <span class="badge budget-pick">Budget Pick</span>
{% endif %}
Condition: Product price is less than ₹500 (50,000 cents)

Shopify Note: Prices are stored in cents, so ₹500 = 50000

Behavior: Shows green "Budget Pick" badge when price condition is met

2. Inventory Badge ("Limited Stock")
liquid
Copy
Download
{% assign first_variant = card_product.selected_or_first_available_variant %}
{% if first_variant.inventory_quantity < 5 and first_variant.inventory_quantity > 0 %}
  <span class="badge limited-stock">Limited Stock</span>
{% endif %}
Conditions:

Inventory quantity is less than 5

Inventory quantity is greater than 0 (item is not out of stock)

Key Variables:

selected_or_first_available_variant: Gets the most relevant product variant

inventory_quantity: Current stock level

Behavior: Shows orange "Limited Stock" badge when inventory is low but available

3. Combined Behavior
When both conditions are true, both badges appear stacked vertically

Badges are mutually independent - one can appear without the other

Implementation Notes
Placement
Added inside the card__inner div for proper positioning

Uses absolute positioning to overlay product images

Styling
css
Copy
Download
.custom-badges {
  position: absolute;
  top: 12px;
  left: 12px;
  z-index: 1;
  display: flex;
  flex-direction: column;
  gap: 4px;
}
Positions badges at top-left of product card

Stacks badges vertically with 4px gap

Ensures visibility above other elements (z-index: 1)

Testing Instructions
Test Price Badge:

Set product price to ₹499 (49900 in admin)

Verify green "Budget Pick" badge appears

Test Inventory Badge:

Enable inventory tracking for a variant

Set stock to 4

Verify orange "Limited Stock" badge appears

Test Both Badges:

Set price < ₹500 AND stock < 5

Verify both badges appear stacked

Edge Cases:

Out of stock items (should never show "Limited Stock")

Exactly ₹500 price (should not show "Budget Pick")

Exactly 5 in stock (should not show "Limited Stock")

Requirements Met
✅ Displays "Budget Pick" for products under ₹500
✅ Displays "Limited Stock" when inventory < 5
✅ Shows both badges when both conditions apply
✅ Maintains existing theme functionality
✅ Responsive design
✅ No JavaScript required (pure Liquid/CSS solution)

Task 2: Custom Collection Filter
 Modify collection.liquid to add a text-based filter that allows users to filter products by 
a custom tag (best-seller, new-arrival, discounted).
 • Create a dropdown using Liquid that filters products based on tags.
 • Ensure the filter does not affect pagination.
 Hint: Use {% if product.tags contains 'best-seller' %} inside a for 
loop

Overview
This code adds a dropdown filter to a Shopify collection page, allowing users to filter products by tags (best-seller, new-arrival, discounted). It modifies the URL to apply the filter without reloading the entire page (if using AJAX) or with a standard page reload.

How It Works
1. HTML Structure
The dropdown is structured as a <select> element inside main-collection-product-grid.liquid:

html
Copy
Download
Run
<div class="custom-tag-filter">
  <label for="tag-filter"><strong>Filter by:</strong></label>
  <select id="tag-filter" onchange="if(this.value) window.location.href=this.value;">
    <option value="{{ collection.url }}">All Products</option>
    <option value="{{ collection.url }}?constraint=best-seller" {% if current_tags contains 'best-seller' %}selected{% endif %}>Best Sellers</option>
    <option value="{{ collection.url }}?constraint=new-arrival" {% if current_tags contains 'new-arrival' %}selected{% endif %}>New Arrivals</option>
    <option value="{{ collection.url }}?constraint=discounted" {% if current_tags contains 'discounted' %}selected{% endif %}>Discounted</option>
  </select>
</div>
2. Key Logic Explained
A. Dropdown Options
All Products → Resets the filter ({{ collection.url }}).

Best Sellers → Filters by best-seller tag (?constraint=best-seller).

New Arrivals → Filters by new-arrival tag (?constraint=new-arrival).

Discounted → Filters by discounted tag (?constraint=discounted).

 Task 3: Personalized Greeting (Homepage Feature)
 Modify index.liquid to display a personalized greeting based on the time of day:
 • Show "Good Morning, Shopper!" before 12 PM.
 • Show "Good Afternoon, Shopper!" between 12 PM – 6 PM.
• Show "Good Evening, Shopper!" after 6 PM.
 Hint: Use now | date: "%H" to fetch the current hour and apply conditional statements.

 Overview
This feature dynamically displays a time-based greeting ("Good Morning," "Good Afternoon," or "Good Evening") on the Shopify store's homepage. It uses client-side JavaScript to determine the user's local time and update the greeting accordingly.

How It Works
1. HTML Structure
A placeholder div is added to index.liquid where the greeting will appear:

html
Copy
Download
Run
<div id="personalized-greeting" class="personalized-greeting" style="text-align: center; margin: 2rem 0;">
  <h2>Loading your greeting...</h2>
</div>
id="personalized-greeting": Used to target the element in JavaScript.

style="text-align: center; margin: 2rem 0;": Ensures the greeting is centered with proper spacing.

2. JavaScript Logic
The greeting is determined based on the user's local time:

javascript
Copy
Download
document.addEventListener("DOMContentLoaded", function () {
  const greetingEl = document.getElementById("personalized-greeting");
  const hour = new Date().getHours(); // Gets current hour (0-23)
  let greeting = "";

  if (hour < 12) {
    greeting = "Good Morning, Shopper!";
  } else if (hour === 12) {
    greeting = "Good Noon, Shopper!";
  } else if (hour < 18) {
    greeting = "Good Afternoon, Shopper!";
  } else {
    greeting = "Good Evening, Shopper!";
  }

  greetingEl.innerHTML = `<h2>${greeting}</h2>`;
});
Key Steps
Wait for DOM to Load

The script runs after the page is fully loaded (DOMContentLoaded).

Get Current Hour

new Date().getHours() returns the hour (0-23) based on the user's local time.

Determine Greeting

Morning (Before 12 PM) → Good Morning, Shopper!

Noon (Exactly 12 PM) → Good Noon, Shopper! (optional, not in original requirements)

Afternoon (12 PM - 5:59 PM) → Good Afternoon, Shopper!

Evening (6 PM - Midnight) → Good Evening, Shopper!

Update the DOM

The greeting replaces the "Loading..." text dynamically.

Why JavaScript Instead of Liquid?
Local Time Accuracy:

JavaScript uses the user's local time, while Liquid (now | date: "%H") uses server time, which may differ.

No Caching Issues:

Since the greeting updates client-side, it works even if Shopify caches the page.

Dynamic Updates:

If the page stays open, the greeting can be updated without a refresh (if extended with a timer).

Testing Instructions
Check Morning Greeting (Before 12 PM)

Set your computer's time to before noon → Should display "Good Morning, Shopper!"

Check Afternoon Greeting (12 PM - 5:59 PM)

Set time to between 12 PM and 6 PM → Should display "Good Afternoon, Shopper!"

Check Evening Greeting (After 6 PM)

Set time to after 6 PM → Should display "Good Evening, Shopper!"

Task 4: Custom Upsell Section (Cart Page)
 Modify cart.liquid to display an upsell product recommendation dynamically:
 • If the cart total is below ₹1000, suggest a product from the Accessories collection.
 • If the cart total is ₹1000 or above, suggest a product from the Premium collection.
 • Make sure the suggested product changes dynamically.
 Hint: Use {% assign cart_total = cart.total_price %} and 
collections['Accessories'].products.first.title

