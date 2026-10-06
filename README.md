# GA4 + Google Tag Manager Tracking Setup

End-to-end conversion tracking for two sites through one Google Tag Manager container and one GA4 property:

- **Portfolio site** — engagement events on a live site (project clicks, dashboard clicks, contact clicks, scroll depth).
- **Demo store** (`index.html` in this repo) — a mock t-shirt shop that sends the full GA4 e-commerce funnel through the dataLayer. Nothing is for sale and no payment is taken.

## What is tracked

| Event | Site | Fires when | Key parameters |
| --- | --- | --- | --- |
| `page_view` | Both | Every page load (Google tag) | page location, title |
| `project_click` | Portfolio | Click on a GitHub project link | `link_url`, `link_text` |
| `dashboard_click` | Portfolio | Click on a Looker Studio dashboard link | `link_url`, `link_text` |
| `contact_click` | Portfolio | Click on email, phone or LinkedIn | `contact_method`, `link_url` |
| `scroll_depth` | Both | Page scrolled 25%, 50%, 75% | `percent_scrolled` |
| `view_item_list` | Demo store | Product grid shown | `items[]` |
| `view_item` | Demo store | Product detail opened | `currency`, `value`, `items[]` |
| `add_to_cart` | Demo store | Add to cart clicked | `currency`, `value`, `items[]` |
| `begin_checkout` | Demo store | Checkout clicked | `currency`, `value`, `items[]` |
| `purchase` | Demo store | Test order placed | `transaction_id`, `currency`, `value`, `items[]` |
| `generate_lead` | Demo store | Sign-up form submitted | `form_name` (no personal data) |

GA4 Enhanced Measurement also records 90% scroll, outbound clicks and file downloads.

## Files

- `index.html` — demo store with the Tag Manager snippet and dataLayer pushes.
- `gtm-container.json` — importable Tag Manager container: 7 tags, 6 triggers, 3 variables.

## How it works

1. The page pushes an event object into `dataLayer`.
2. A Tag Manager trigger matches the event name (or a link click / scroll threshold).
3. The matching GA4 event tag forwards it to the GA4 property, with e-commerce data read from the dataLayer.

No personal data is sent: the sign-up form pushes only the event name and form name.
