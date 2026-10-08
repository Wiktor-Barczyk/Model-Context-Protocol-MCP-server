# Odoo MCP Connector

A Python-based Model Context Protocol (MCP) server that connects AI assistants to Odoo ERP and related business documents. It provides a structured, read-only interface for retrieving customer information, sales orders, helpdesk conversations, accounting records, CRM data, tasks, production activity, and selected files.

The connector is designed to reduce manual navigation between business systems while keeping access to sensitive data within an authorized environment.

## Features

- **Customer lookup:** resolve customer records using a name or an Odoo reference, and retrieve a consolidated customer overview.
- **Sales and CRM:** browse sales orders, opportunities, communication history, and call activity.
- **Helpdesk:** retrieve ticket conversations, workflow history, response metrics, and workload statistics.
- **Accounting:** inspect invoices, payments, outstanding balances, and invoice delivery status.
- **Document management:** browse approved directories, search file names and paths, and read supported documents, including PDFs and images.
- **Projects and knowledge:** retrieve employee tasks, Odoo activities, and internal knowledge articles.
- **Manufacturing and productivity:** analyze work orders, recorded operation time, expected versus actual duration, and employee work summaries.
- **GnuCash integration:** inspect accounts and transactions from supported GnuCash file formats using temporary read-only snapshots.
- **Time-based filtering:** limit selected queries by date range, employee, customer, status, or work center.
- **AI integration:** expose these operations as MCP tools for compatible clients.

## Technology

- Python 3.11
- Model Context Protocol (MCP)
- Streamable HTTP transport
- Odoo ERP (integration developed for Odoo 12)
- GnuCash file readers (SQLite, XML, and compressed XML)
- Service-based deployment, with support for process supervision such as systemd

## Available MCP Tools

The current connector exposes the following **25 read-oriented tools**:

| Area | Tools | Purpose |
| --- | --- | --- |
| Customers | `resolve_client`, `get_client_overview` | Find records and summarize customer-related objects. |
| Sales | `list_sales_orders` | List sales orders with optional filters. |
| Documents | `list_files`, `search_documents`, `read_document` | Find and read files within configured locations. |
| Communication | `read_messages` | Read messages connected to helpdesk tickets, production orders, and sales orders. |
| Accounting | `read_accounting`, `read_accounting_all`, `read_invoice_messages`, `audit_invoice_emails` | Review invoices, payments, balances, and invoice email status. |
| Identification | `read_id_numbers` | Retrieve restricted identification records for authorized business use. |
| CRM | `list_opportunities`, `read_opportunity`, `list_crm_opportunities` | Inspect opportunities, stages, notes, and contact activity. |
| Tasks | `list_user_tasks`, `read_task`, `list_user_activities` | Inspect assigned tasks, task details, and scheduled activities. |
| Knowledge | `list_knowledge`, `read_knowledge_article` | Access internal knowledge articles. |
| GnuCash | `list_gnucash_accounts`, `read_gnucash_transactions` | Read accounts and transactions from GnuCash files. |
| Operations | `list_mrp_operations`, `list_helpdesk_activity`, `list_employee_work_summary` | Analyze production time, helpdesk performance, and employee workload. |

## How It Works

1. An MCP-compatible AI client requests a specific tool, such as `get_client_overview` or `list_employee_work_summary`.
2. The server receives the request over its configured MCP transport.
3. The appropriate service resolves references and applies the requested filters.
4. The connector reads permitted Odoo records or authorized files; GnuCash readers use a temporary snapshot rather than changing the original file.
5. Structured results are returned to the AI client for summarization or analysis.

### Example Use Cases

- Summarize open customer helpdesk issues and their recent conversations.
- Check which sales orders or production operations are associated with a customer.
- Compare employee operation time with expected durations for a selected date range.
- Audit whether invoices from a given period were sent successfully.
- Search authorized project folders for documents related to a particular order.
- Combine manufacturing, helpdesk, and activity statistics into a workload overview.

## Privacy

This project was developed for **Optymalizator Sp. z o.o.** and **Additional Solution.**

This README describes capabilities only. It contains **no production credentials, customer records, tax identifiers, authentication tokens, or deployment-specific addresses**. Any public release of the source code should undergo a separate security and privacy review.
