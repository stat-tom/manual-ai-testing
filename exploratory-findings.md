# TechShop — Exploratory Testing Session Findings

**Date:** November 20, 2026  
**Target:** `techshop/broken-app` (served via local HTTP)  
**Tools Used:** Playwright MCP (`browser_navigate`, `browser_snapshot`, `browser_click`, `browser_fill_form`, `browser_evaluate`)

---

## Findings Summary

### General & UI
1. **Browser Tab Title Shows "Untitled"**  
   The page `<title>` tag is hardcoded as `"Untitled"` instead of displaying a descriptive application title such as "TechShop".

2. **Low Contrast on Guest Browse Button**  
   The "Browse as Guest" button styles light gray text (`#d1d5db`) over a white background (`#ffffff`), failing WCAG AA contrast compliance.

### Login
3. **Password Input Exposed in Plaintext**  
   The password input is configured as `type="text"` rather than `type="password"`, leaving passwords completely visible on screen.

4. **Login Bypass with Empty Credentials**  
   Clicking "Sign In" with empty username and password fields immediately signs in and redirects to the product catalog without validation.

5. **No Error Message Displayed on Failed Login**  
   Entering invalid credentials gives no visual feedback or error message, silently bypassing authentication.

### Product Catalog
6. **Incorrect Product Discount Calculation**  
   Discounts are computed with a divisor of `1000` rather than `100` (`price * (1 - discount / 1000)`), reducing discounts by a factor of 10.

7. **Broken Image URL on Wireless Headphones**  
   The primary product card points to an invalid image URL (`BROKEN_LINK_404`), producing a broken image icon and a 404 network error.

8. **Missing Image Alt Attribute**  
   The first product image (`Wireless Headphones`) completely omits the `alt` attribute, violating accessibility standards for screen readers.

9. **Product Description Overflows Card Container**  
   The "Mechanical Keyboard" description text applies `white-space: nowrap`, causing the description to spill out horizontally past the card boundary.

10. **Add to Cart Accepts Zero and Negative Quantities**  
    The quantity spinbox allows values like `0` or `-5`, which can be added directly to the shopping cart without restriction.

### Shopping Cart
11. **Negative Quantities Permitted in Cart**  
    The cart quantity input field permits negative integers without input validation, causing negative subtotals and negative cart item counts.

12. **Cart Total Fails to Recalculate on Item Removal**  
    Removing an item updates the row list but leaves the Subtotal and Total in the Order Summary frozen at their previous amounts.

13. **"Proceed to Checkout" Button Does Nothing**  
    The "Proceed to Checkout" button triggers `proceedToCheckoutBuggy()` which returns `false` without navigating to checkout.

### Checkout & Confirmation
14. **Email Field Lacks Format Validation**  
    The email field is set to `type="text"` and lacks email regex validation, accepting arbitrary text such as "invalid-email-address".

15. **Phone Number Field Accepts Letters and Symbols**  
    The phone field is a generic text input without `type="tel"` or pattern enforcement, allowing alphabetic characters and symbols.

16. **Payment Details Lack Format and Checksum Validation**  
    Card number, expiry date, and CVV fields accept arbitrary non-numeric text without validation.

17. **Checkout Order Summary Is Blank**  
    The order summary sidebar on the Checkout page displays "Nothing to show here." rather than showing the user's cart items and totals.

18. **Confirmation Screen Missing Details and Does Not Clear Cart**  
    After order placement, no order number, itemized receipt, or summary is shown, and the cart badge retains previous items instead of resetting.
