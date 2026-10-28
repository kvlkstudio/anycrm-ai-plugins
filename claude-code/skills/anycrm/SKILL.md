---
name: anycrm
description: Work with the user's anyCRM — clients, appointments, services and materials, products, stock and orders, a restaurant's menu, tables and reservations, finances and ФОП taxes, the online booking/ordering web app, staff, and integrations (Prom, Rozetka, WooCommerce and other shops, Nova Poshta, Ukrposhta, monobank and other payments, Checkbox, banks, SMS). Use when the user asks to find or add a client, book or move an appointment, take or update an order, check stock, see revenue or expenses, prepare taxes, or change anything else in anyCRM.
---

# anyCRM

The anyCRM MCP server acts on the user's own anyCRM account, with their rights:
a staff member can only touch the sections the owner gave them.

## Start

1. `list_companies`. Every other tool takes `companyId`; leave it out only while
   the user has one company. If there are several and it is not clear which one,
   ask.
2. `get_company` shows the company type (`services`, `shop`, `restaurant`),
   its modules and how much it holds. A tool for a module that is off is refused;
   `list_modules` + `set_module` switch modules on (paid ones cost money from the
   balance — ask first).

## Reading

- Lists take `query`, filters, `from`/`to` (ISO 8601) and `limit`/`offset`; a
  page has `total` and `nextOffset` while there is more.
- `get_summary` answers "how are we doing": income and expenses, orders by
  status and their total, appointments by status, new clients, low materials.
- `get_client` gives a client's whole history; `get_order` and `get_product` the
  full record with receipts, listings in shops and stock history.

## Changing

- `save_*` without `id` creates; with `id` it changes only the fields given, so
  pass just what changes. Field names are the ones the tools return.
- Times are ISO 8601 with a time zone (the company is in Europe/Kyiv unless the
  profile says otherwise): `2026-11-03T14:30:00+02:00`.
- Clients: a person needs `firstName` or `lastName`, a business `isCompany` +
  `companyName`. Phone and email are unique within the company — on
  "already exists", find that client with `list_clients` instead.
- Appointments: `clientId`, `serviceId`, `start`; the duration comes from the
  service. Setting `status: "done"` writes off the service's materials and books
  its price as income, once.
- Orders: `items` is the whole list; a line with `productId` takes the product's
  title and price. Stock follows the lines; `status: "completed"` books the
  income; a status change goes back to the marketplace the order came from.
  A restaurant order may have no client.
- Stock moves only through `set_product_stock` (supply / adjustment) and orders,
  never through `save_product`.
- Money is in the company currency (`uah`, `usd`, `eur`).

## Ask before

Deleting anything, `send_sms`, `create_np_waybill` / `create_up_shipment`,
`fiscalize_order` and opening or closing a fiscal shift, `create_payment_link`,
`top_up_balance`, `set_module` for a paid module, `invite_staff` /
`remove_staff`, and adding or removing integrations. Show what will happen first.

## Modules in short

- Calendar & services: `list_appointments`, `save_appointment`, `list_services`,
  `save_service`, materials with `add_material_supply` and `get_material_history`.
- Shop: products, categories, `import_products_yml`, `upload_product_image`
  (base64; an image already online goes straight into `images`), orders,
  `create_payment_link`, `check_payment`.
- Restaurant: the menu is products (`portion`, `modifiers`), `save_table`,
  `save_reservation`, orders with `kind: "dineIn"` and `tableId`.
- Finances: `list_transactions` (with totals), `save_transaction`,
  `import_bank_statement`, `sync_bank`. Taxes: `get_tax_report`,
  `get_income_book`, `get_tax_declaration`, `save_tax_settings`.
- Web app: `get_web_app`, `save_web_app`, `check_alias` — the page is
  `https://<alias>.anycrm.io`.
- Sales channels: `list_integrations`, `sync_integration`, `get_sync_log`,
  `get_channel_matrix`, `list_unlinked_items` + `link_unlinked_item`,
  `set_listing_price`, `set_stock_source`, `save_integration_settings`.
  `add_integration` tells which credentials a service needs; OLX, Google
  Calendar and Shopify connect through `get_connect_url`.
- Delivery: `np_find_cities` → `np_find_warehouses` → `create_np_waybill`,
  `track_np`; the same with `up_*` for Ukrposhta.
- People and the rest: `list_staff`, `set_staff_sections`, `list_notifications`,
  `get_billing`, `list_reviews`, `lookup_company_register` (ЄДРПОУ/ІПН).

Errors are the app's own messages; pass them on in plain words.

The first call opens the browser to sign in to anyCRM and allow access. The user
can disconnect the assistant in anyCRM: Profile > AI assistants.
