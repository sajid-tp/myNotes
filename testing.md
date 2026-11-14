# Step Select – Pre-Presentation Test Plan

Tick every box. Where a row says **Expect**, compare what you see with it. Anything that differs is a bug – note the steps and fix it before the demo.

## 0. Setup (do this first)

- Use **two browsers** (e.g. Chrome + Edge) or one normal + one incognito window: one for the customer, one for the admin. Admin and customer use different cookies, so both can be signed in at once – test that too.
- Create test data: 2 categories, 2 brands, 4 products (one with a variant that has stock = 1, one with several sizes), 1 customer with 2 addresses, 1 blocked customer.
- Create these coupons (limits come from `couponConstants.js`: percent 1–90, flat 1–25000, per-customer uses 1–5):

| Code | Setup |
|---|---|
| FLAT100 | flat ₹100, min purchase ₹0, uses per customer 1 |
| PCT10 | 10 %, cap ₹200, min purchase ₹1000 |
| PCT90 | 90 %, no cap |
| LIMIT2 | total redemption limit 2 |
| FUTURE | start date tomorrow |
| EXPIRED | expiry date yesterday |
| OFF | active toggled **off** |

- MongoDB transactions are only used on a replica set (Atlas or local replica set). On a standalone MongoDB the code falls back to "no transaction" mode. **Test on the same setup you will present on.**

---

## 1. Authentication & protected routes (back-button bugs)

For every row: do the action, then press the browser **Back** and **Forward** buttons, then **refresh**.

| # | Scenario | Expect |
|---|---|---|
| 1.1 | Signed out → open `/profile`, `/cart`, `/checkout`, `/orders`, `/wishlist`, `/profile/wallet` by typing the URL | Redirect to `/login`; after login you land back where you wanted |
| 1.2 | Log in → press Back | Does **not** return to the login page (redirects home) |
| 1.3 | Signed in → type `/login`, `/signup`, `/verify-otp` | Redirect to `/` |
| 1.4 | Log out from the navbar → press Back on every page you visited (profile, orders, cart, checkout) | Never shows account data; goes to `/login` |
| 1.5 | Log out in tab A while tab B is open on `/profile` → click anything in tab B | Tab B signs out (SessionGuard) / redirects to login |
| 1.6 | Open `/update-password` directly (no OTP flow) | Redirects to `/forgot-password` *(fixed in this round)* |
| 1.7 | Forgot-password flow → OTP → new password → Back | Cannot reuse the form with an old token |
| 1.8 | Wrong OTP, expired OTP, resend OTP before timer ends | Clear message each time, no crash |
| 1.9 | Signed in as customer → open `/admin` | Goes to `/admin/auth/login`, never shows admin pages |
| 1.10 | Admin login → press Back | Doesn't land on the login form again |
| 1.11 | Admin signed in → type `/admin/auth/login` | Redirects to `/admin` *(fixed this round)* |
| 1.12 | Admin logout → Back / Forward | Login page, never the old dashboard *(back/forward-cache fix this round)* |
| 1.13 | Admin logged in, delete the `token` cookie in DevTools → click any admin tab | Redirect to admin login |
| 1.14 | Block a logged-in customer from admin Users → customer clicks anything | "Blocked" modal appears, session ends |
| 1.15 | Unknown URL `/abc`, `/products/not-a-real-id`, `/orders/XYZ`, `/admin/foo` | 404 page or friendly fallback, no white screen |
| 1.16 | Sign-up validation: empty fields, bad email, weak password, mismatched passwords, existing email, Google sign-in | Each shows the right message |
| 1.17 | Referral link `/invite/:token` with valid / invalid token | Valid → signup with referral; invalid → no crash |

## 2. Product pages & fallbacks

| # | Scenario | Expect |
|---|---|---|
| 2.1 | Open a product, then in admin **block** it, then refresh the product page | "This product is no longer available" page with shop/home buttons |
| 2.2 | Same, but block the product's **category** or **brand** | Same fallback; product disappears from Shop, search and related products |
| 2.3 | Stop the server, open any product | "We couldn't load this product" with **Try again** |
| 2.4 | Product with every size out of stock | Add-to-cart disabled and "Out of stock" shown |
| 2.5 | Colour switch, size switch, image zoom/viewer, wishlist heart (signed out → login prompt) | All work |
| 2.6 | Product in a customer's **cart/wishlist** gets blocked | Cart shows it as unavailable; checkout is blocked until removed |

## 3. Coupons (100 % coverage)

Cart subtotal after product discounts = **base**.

| # | Scenario | Expect |
|---|---|---|
| 3.1 | Apply FLAT100 on a ₹2000 cart | −₹100; total recalculated |
| 3.2 | PCT10 on ₹800 cart | Rejected: minimum purchase message with the shortfall |
| 3.3 | PCT10 on ₹5000 | 10 % = ₹500 but **capped at ₹200** |
| 3.4 | PCT90 on ₹1000 | −₹900; total is never negative and the order is never free |
| 3.5 | Try to create a 100 % coupon in admin / by API | Rejected (max 90 %) |
| 3.6 | Lower-case / spaced code (` flat100 `) | Accepted (trimmed + upper-cased) |
| 3.7 | Wrong code, empty code | "Coupon not found" / "code required" |
| 3.8 | FUTURE, EXPIRED, OFF | Each rejected with its own message; none listed as available |
| 3.9 | LIMIT2: use it with 2 different customers, then a 3rd | 3rd is rejected, limit reached |
| 3.10 | FLAT100 used once, same customer applies again (limit 1) | "Already used" |
| 3.11 | **Cancel** the order that used a coupon | Coupon becomes usable again; redemption count goes back down |
| 3.12 | Apply coupon, then remove items so base < minimum, open checkout | Coupon auto-removed with an explanation |
| 3.13 | Apply coupon, admin deactivates it, customer clicks **Place order** | Order refused with message; coupon cleared from cart |
| 3.14 | Two browsers place orders at the same moment using the last redemption of LIMIT2 | Exactly one succeeds |
| 3.15 | Coupon + referral discount on one order | Both applied, total correct, order detail shows both |
| 3.16 | Coupon dropdown in cart/checkout: applicable vs not applicable states, "amount needed" text | Correct for current cart |
| 3.17 | Admin coupon form: duplicate code, code with spaces/symbols, min > max, expiry before start, negative numbers, decimals in percent | Each blocked with a message (client **and** server) |
| 3.18 | Admin coupon list: search, filter, status toggle with modal, edit, delete (soft), pagination | All work |

## 4. Offers, pricing & cart

| # | Scenario | Expect |
|---|---|---|
| 4.1 | Product offer, category offer and variant discount on the same product | The best one wins; price on card = product page = cart = checkout = invoice |
| 4.2 | Offer expires / admin deactivates it | Price returns to normal everywhere |
| 4.3 | Add the same variant+size twice | Quantity increases, no duplicate line |
| 4.4 | Quantity above stock; quantity 0 / negative via +/- | Blocked with a message |
| 4.5 | Stock drops (admin edits stock) while item sits in cart | Cart shows insufficient stock; checkout blocked |
| 4.6 | Free-shipping threshold: ₹ just below and just above | Shipping fee appears/disappears correctly |
| 4.7 | Tax line | Check one order by hand: tax = % of discounted line price (see note in section 11) |

## 5. Checkout & payment

| # | Scenario | Expect |
|---|---|---|
| 5.1 | No address → place order | Blocked, prompt to add address |
| 5.2 | COD order | Stock reduces, cart empties, success page, order in My Orders |
| 5.3 | Press **Back** from the success page | Not an empty-checkout error; goes to a sensible page |
| 5.4 | Double-click **Place order** quickly | Only one order created |
| 5.5 | Last-unit race: two browsers buy the last pair | One succeeds, the other gets an out-of-stock message |
| 5.6 | Wallet payment with enough balance / not enough | Debits exactly the total / clear error |
| 5.7 | Online (Razorpay/demo) success | Order marked paid, success page |
| 5.8 | Online failure, cancel the popup, close the tab mid-payment | Failure page, retry works, stock restored if abandoned |
| 5.9 | Payment retry limits and countdown | Behaves as designed |
| 5.10 | Reload on `/payment-success/:n` and `/payment-failed/:n` | Stays on correct state |
| 5.11 | Another user's order number in the URL | Not found, never shows their data |
| 5.12 | Invoice download | Numbers equal the order page |

## 6. Orders, cancellation, returns & refunds

Worked example to verify by hand: order of 2 items (₹1000 + ₹500), coupon −₹150, shipping ₹50, online paid.

| # | Scenario | Expect |
|---|---|---|
| 6.1 | Cancel one item | Refund to wallet = item price share − its coupon share + its tax; shipping stays |
| 6.2 | Cancel the second item | Remaining amount incl. shipping refunded; order = cancelled; coupon released |
| 6.3 | Cancel **part** of a quantity (qty 3 → cancel 1) | Totals and stock adjust proportionally |
| 6.4 | Cancel a COD order | No wallet refund (nothing was paid) but stock returns |
| 6.5 | Cancel after the item has shipped | Not allowed |
| 6.6 | Request return on delivered item, admin approves / rejects | Refund only on approve; stock restored per your rules |
| 6.7 | Return more than purchased / twice | Blocked |
| 6.8 | Wallet balance and transaction list after each step | Add up exactly, no duplicates |
| 6.9 | Admin moves status pending → shipped → out for delivery → delivered; try skipping a step | Skips are refused |
| 6.10 | Order list: search by number, filter by status, sort, pagination | Work and combine |

## 7. Admin panel, every tab

For **Categories, Brands, Products, Offers, Coupons, Banners** (and **Users** block/unblock):

- Create (valid / every invalid field), edit, delete (with confirmation), activate/deactivate toggle **with modal**, cancel on the modal does nothing.
- Duplicate name, leading/trailing spaces, very long text, special characters, emoji.
- Image upload: wrong type, > size limit, crop, replace, remove.
- Search (type, clear, no results state), filter, sort, pagination (go to page 3, search → resets to page 1).
- Deactivating/deleting a **category or brand** hides its products on the storefront; reactivating brings them back.
- **Inventory**: change stock, set 0 → storefront shows out of stock; low-stock filter.
- Product form: variants, sizes, price 0 / negative / text, discount > 100, min/max images, unsaved-changes navigation.

## 8. Search, filter & sort (storefront)

- Shop search: partial word, upper/lower case, special chars like `( ) [ ] * +`, empty, only spaces, very long text → never errors.
- Category + brand + price range + size + colour + sort together; min > max; clear-all button; URL/refresh keeps or resets consistently.
- Pagination after every filter change starts at page 1; last page; page beyond range.
- Debounce: typing fast fires one request, not one per letter (DevTools → Network).

## 9. Dashboard & sales report (numbers must reconcile)

1. Note dashboard totals and sales report for "today".
2. Place a COD order → both increase by the order total.
3. Cancel it → sales falls back; cancelled count +1.
4. Place and pay an online order, abandon another unpaid one → only the paid one counts.
5. Approve a return → revenue falls by the refund, order count unchanged (by design).
6. Compare sales-report total vs the sum of the table rows vs the exported file (PDF/Excel).
7. Date ranges: today, week, month, year, custom, end < start, > 3 years, empty range (zeros, not NaN).
8. Top products / categories / coupon usage match what you ordered.

## 10. Non-functional

- Responsive: 360 px phone, tablet, laptop; navbar, filters, tables, modals.
- Slow network (DevTools → Network → Slow 3G): loaders show, no double submits.
- Server down / 500 / 401 mid-session: friendly messages.
- DevTools **Console**: no red errors on any page; **Network**: no failing calls.
- Security spot-checks: customer token on an admin API (expect 401/403), editing another user's address/order by ID (expect not found), price or total sent from the browser is ignored (server recomputes), HTML/script in text fields is escaped.
- Production build: `npm run build` in client, then `npm run preview` and repeat sections 1, 5 and 9 on it.

---

## 11. Code-review findings

**Fixed in this round**
1. `/update-password` could be opened directly – now redirects.
2. Admin login page was reachable while signed in – now redirects to the dashboard.
3. Admin dashboard could reappear via the browser back/forward cache after logout – session is re-verified.
4. No global crash fallback – added an error boundary (friendly page instead of white screen).

**Your decision (not changed – these are design choices)**
- **Tax and coupons:** tax is calculated on the line price *before* the coupon, so a coupon does not reduce tax. Decide if that is intended and keep the invoice wording consistent.
- **Returns in reports:** the dashboard's top products subtract returned units; the sales report's category breakdown does not (its own comment says so). Be ready to explain this if the two screens are compared.
- **Secrets:** both `.env` files were inside the zips you shared. If those contain real keys (JWT secret, Cloudinary, Razorpay, DB password), rotate them and keep `.env` out of git.

**Not reviewed** – please test these carefully: payment gateway verification, wallet top-up, referral rewards, product/variant admin services, inventory, banners and image uploads.

## 12. Making it repeatable

- **Server:** add `jest` + `supertest` and write tests for `calculateTotals`, `calculateCouponDiscount`, `allocateCouponShares` (pure functions – easiest wins) and for apply-coupon / place-order / cancel flows against a test database.
- **Client:** add Playwright and automate sections 1, 3 and 5 so you can re-run them before every demo.
- Keep a short **demo script** with seeded data so the presentation never depends on live typing.
