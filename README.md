# anyCRM for Claude Code and Codex

anyCRM's MCP server is at `https://mcp.anycrm.io`. Sign-in is OAuth: the first
time, the browser opens anyCRM, you sign in and press Allow. Connected
assistants are listed, and can be disconnected, in anyCRM > Profile > AI
assistants.

The assistant works with your rights: a staff member reaches only the sections
the owner gave them, and every check and side effect is the app's own (stock,
income from done appointments and completed orders, marketplaces,
notifications).

## Claude Code

```bash
claude plugin marketplace add kvlkstudio/anycrm-ai-plugins
claude plugin install anycrm@anycrm
```

Or the server alone, without the plugin's skill:

```bash
claude mcp add --transport http anycrm https://mcp.anycrm.io
```

Then `/mcp` > anycrm > Authenticate.

## Codex

```bash
codex plugin marketplace add kvlkstudio/anycrm-ai-plugins
codex plugin add anycrm@anycrm
codex mcp login anycrm
```

Or the server alone:

```bash
codex mcp add anycrm --url https://mcp.anycrm.io
codex mcp login anycrm
```

## Claude (web and desktop)

Settings > Connectors > Add custom connector, URL `https://mcp.anycrm.io`.

## Tools

- Account and companies: get_profile, update_profile, list_companies,
  get_company, create_company, update_company, list_modules, set_module,
  request_module_access, get_summary.
- Clients: list_clients, get_client, save_client, delete_client.
- Services and materials: list_services, save_service, delete_service,
  list_materials, save_material, delete_material, get_material_history,
  add_material_supply.
- Calendar: list_appointments, save_appointment, delete_appointment.
- Products and stock: list_products, get_product, save_product,
  delete_product, set_product_stock, upload_product_image,
  import_products_yml, list_categories, save_category, delete_category.
- Orders: list_orders, get_order, save_order, delete_order,
  create_payment_link, check_payment.
- Restaurant: list_tables, save_table, delete_table, list_reservations,
  save_reservation, delete_reservation.
- Finances and taxes: list_transactions, save_transaction,
  delete_transaction, export_accounting, get_tax_settings,
  save_tax_settings, get_tax_report, get_income_book, get_tax_declaration.
- Web app: get_web_app, save_web_app, check_alias.
- Integrations and sales channels: list_integrations, add_integration,
  delete_integration, get_connect_url, get_plugin_key,
  save_integration_settings, sync_integration, export_to_channel,
  get_sync_log, get_channel_matrix, retry_channel_queue, set_stock_source,
  list_unlinked_items, link_unlinked_item, ignore_unlinked_item,
  set_listing_price, list_reviews, lookup_company_register,
  export_to_dilovod.
- Delivery: np_find_cities, np_find_warehouses, create_np_waybill, track_np,
  up_find_cities, up_find_warehouses, create_up_shipment, track_up.
- SMS: get_sms_balance, send_sms.
- Fiscal receipts (ПРРО): get_fiscal_shift, open_fiscal_shift,
  close_fiscal_shift, fiscalize_order, list_fiscal_receipts,
  refresh_fiscal_receipt.
- Banks: list_bank_accounts, sync_bank, import_bank_statement.
- Staff: list_staff, invite_staff, set_staff_sections, remove_staff.
- Notifications: list_notifications, mark_notifications_read,
  get_notification_settings, save_notification_settings.
- Billing: get_billing, list_billing_charges, top_up_balance, check_top_up,
  set_auto_top_up.
