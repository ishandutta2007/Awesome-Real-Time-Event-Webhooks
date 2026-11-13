# Awesome-Real-Time-Event-Webhooks

# Awesome Real-Time Event Webhooks

> A curated list of top Real-Time Event Webhook platforms — both SaaS/Hosted commercial solutions and open-source alternatives for ingesting, routing, retrying, and delivering event-driven webhooks reliably at scale.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [Chargebee Event Webhooks](#chargebee-event-webhooks)
  - [Svix](#svix)
  - [Hookdeck](#hookdeck)
  - [Webhook.site](#webhooksite)
  - [RequestBin](#requestbin)
  - [Zapier Webhooks](#zapier-webhooks)
  - [Make Webhooks](#make-webhooks)
  - [Courier Webhooks](#courier-webhooks)
  - [Pipedream](#pipedream)
- [Open-Source Software](#-open-source-software)
  - [Convoy](#convoy)
  - [Hook0](#hook0)
  - [Outpost (Hookdeck)](#outpost-hookdeck)
  - [WebhookX](#webhookx)
  - [Webhook Tester (tarampampam)](#webhook-tester-tarampampam)
- [Comparison at a Glance](#-comparison-at-a-glance)
- [Contributing](#-contributing)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted Real-Time Event Webhook platforms. These are not open-source but are included as reference points for feature comparison.

### Chargebee Event Webhooks
**Type:** Billing event webhooks with no signature verification
Chargebee webhooks notify your application about subscription and billing activity — subscription creation, cancellation, payment success/failure, and invoice generation . **Critical security note:** Chargebee does **not** support HMAC signing. There is no signature header, no timestamp, and no replay window. Authenticity rests on optional HTTP Basic Auth (off by default), a secret-in-URL pattern, and region-specific IP allowlisting . Payload shape varies by `api_version` (v1/v2), so defensive parsing is essential. Retries follow 7 attempts over roughly two days; the endpoint must return 2xx. Dedupe on the event `id` .  
**Best for:** Existing Chargebee customers needing to react to billing events, with awareness that signature verification is unavailable and Basic Auth must be manually enabled.

### Svix
**Type:** Webhooks-as-a-service with open-core server and consumer portal
Svix is the leading commercial webhooks platform, offering endpoint management, event type catalogs with dot-notation hierarchy, HMAC signing, automatic retries, dead letter queues, delivery logs, and manual replay . The core server is MIT-licensed, making it a rare example of a commercial platform with a genuinely open-source core, though the self-hosted version lacks some features of the hosted service, and supported on-prem deployment is an Enterprise-plan feature . The free tier includes 50,000 messages/month; paid tiers add longer retention and higher API rates .  
**Best for:** SaaS companies wanting a mature webhooks platform with a consumer portal, strong signing, and a larger ecosystem, accepting open-core trade-offs if self-hosting.

### Hookdeck
**Type:** Event gateway with queue, retries, and observability
Hookdeck provides an inbound event gateway (webhook proxy, queueing, retries, replay) and a separate outbound product, Outpost . It guarantees ingestion even during downtime or traffic spikes, with a Developer tier free up to 10,000 events and 3-day retention, scaling to Team ($39/month) and Growth ($499/month with SSO, SCIM, HIPAA) . A 2026 pricing update increased free limits to 100k requests and made seats unlimited on all paid plans . The Outbound product (Outpost) is Apache-2.0 and self-hostable; the inbound Event Gateway is closed-source .  
**Best for:** Teams wanting a managed gateway for inbound webhook reliability, with an open-source option for outbound delivery.

### Webhook.site
**Type:** Instant inspection with programmable custom actions
Webhook.site gives you a unique URL immediately for inspecting incoming webhooks, HTTP requests, form submissions, and emails in real time . The free tier gives 7-day URL expiry and 100 requests, with data publicly viewable by anyone with the URL . Paid accounts unlock **Custom Actions** (visual and code-based workflows that run on every request), Queues for async processing and throttling, Schedules (cron for webhooks), and a managed PostgreSQL-compatible database . It has evolved from a simple inspector into a programmable automation endpoint.  
**Best for:** Developers wanting instant webhook debugging plus a path to automating workflows (transform, forward, notify, store) without writing backend code.

### RequestBin
**Type:** Webhook debugging workspace with forwarding and mock APIs
RequestBin provides a webhook debugging workspace with named bins, request history, replay, and mock APIs . The Free tier includes 3 bins, 500 requests/bin/day, 100 stored per bin, and 5 API keys . Pro ($12/month) unlocks 20 bins, unlimited request history, edit & replay with a full HTTP Requester, 5 forwarding rules, and 20 Mock APIs with 5 rules each . It includes an MCP server for AI IDEs and bulk export.  
**Best for:** Developers and small teams wanting an affordable, organized webhook debugging workspace with request history and forwarding to a live app.

### Zapier Webhooks
**Type:** Webhook triggers and actions within multi-app automation
Zapier's Webhooks by Zapier app triggers Zaps from incoming webhooks (Catch Hook) or sends webhooks as actions (POST, GET, PUT, or Custom Request for PATCH/DELETE) . It handles automatic data parsing for standard methods, supports JSON/XML/Form/Raw payloads, Basic Auth, and custom headers . The Custom Request option provides raw JSON/XML flexibility for nested arrays, empty values, or specialized headers . It is a lightweight glue layer for connecting webhook-emitting services to 7,000+ apps without code.  
**Best for:** Teams wanting no-code automation that consumes or emits webhooks as part of broader multi-app workflows.

### Make Webhooks
**Type:** Custom webhooks within visual automation scenarios
Make's Webhooks app lets you create custom webhooks and mailhooks to trigger scenarios instantly from third-party services . Each scenario uses its own unique webhook URL, with optional API Key authentication via the `x-make-apikey` header . You can define a data structure to validate incoming payloads (JSON, form data, multipart with file uploads up to 5 MB) or let Make accept all data without validation . It also supports request headers and JSON pass-through for accessing original payloads .  
**Best for:** Teams using Make for visual automation who need to receive webhooks as scenario triggers with structured data validation.

### Courier Webhooks
**Type:** Notification delivery via webhook provider
Courier's Webhook integration delivers HTTP requests to a static or dynamic URL as part of its notification orchestration platform . You can set the method, headers, and basic or bearer authentication, and control whether the payload includes limited or expanded profile data . Dynamic destinations let you specify the webhook URL per-recipient via the profile, and provider overrides can replace the URL, method, headers, or body entirely . It is a notification channel rather than a general-purpose webhook gateway.  
**Best for:** Teams already using Courier for multi-channel notifications wanting to add webhook delivery as one of their notification channels.

### Pipedream
**Type:** Developer-focused event source platform
Pipedream provides HTTP webhook triggers and event sources with code-first Node.js/Python steps, built-in auth handling, and event history . It is suited for developers wanting programmatic control over webhook processing, with the ability to write custom logic between trigger and destination. **Note:** Pipedream Workflows is closing, according to a 2026 report .  
**Best for:** Developers wanting code-first webhook processing with the ability to write custom transformation and routing logic.

---

## 🟩 Open-Source Software

Self-hosted, community-driven Real-Time Event Webhook tools. The open-source ecosystem spans **full webhook gateways**, **event delivery infrastructure**, and **focused inspection/forwarding tools**.

### Convoy
**Stack:** Go / PostgreSQL / Redis | **License:** Open Source (per project)  
Convoy is an open-source, high-performance **webhooks gateway** designed to manage millions of webhooks end-to-end . It supports both **incoming** projects (receive webhooks from providers with a unique URL per source) and **outgoing** projects (publish events from your systems to customers via API) [from previous context]. Features include retries, filtering, transformations, portal links, and observability. It is the most mature open-source option for teams needing a unified gateway for both inbound and outbound webhook traffic.  
**Best for:** Teams needing a self-hosted gateway for both receiving provider webhooks and sending outbound webhooks to customers, with retry and observability built in.

### Hook0
**Stack:** (Check repository) | **License:** SSPL-1.0  
Hook0 is an open-source webhooks-as-a-service platform with **full feature parity between cloud and self-hosted** . Supports outbound webhook delivery with dot-notation event type hierarchy, HMAC signatures, automatic retries with escalating backoff, dead letter handling, delivery logs, manual replay, REST API, and **label-based multi-tenant filtering** for SaaS platforms routing different events to different tenants . SDKs cover JS, Rust, Python, Go, Ruby, PHP, C#, Java, Kotlin, Lua, and Zig. Default deployment uses PostgreSQL for both queueing and storage; high-throughput setups can switch to Apache Pulsar and S3-compatible storage . EU data plane available on every plan .  
**Best for:** Teams wanting genuinely open-source (SSPL) webhooks infrastructure with no feature gates, strong multi-tenant routing, and EU data residency.

### Outpost (Hookdeck)
**Stack:** Go | **License:** Apache-2.0  
Outpost is Hookdeck's open-source **outbound event delivery infrastructure** for sending webhooks and events to customer-preferred destinations . Supports **webhooks (HTTP), AWS SQS, RabbitMQ, GCP Pub/Sub, Amazon EventBridge, and Kafka** as destination types. Features topic-based subscriptions, publish via API or queue, at-least-once delivery, automatic and manual retries, multi-tenant support, a user portal for customers, delivery failure alerts, and OpenTelemetry integration . Managed by Hookdeck with usage-based pricing from $10 per million events .  
**Best for:** SaaS platforms wanting to deliver events not just to webhooks but also directly to customer queues, brokers, and event gateways.

### WebhookX
**Stack:** Go | **License:** Apache-2.0 (production license required for commercial use)  
WebhookX is an open-source webhook gateway for receiving, validating, transforming, and delivering events at scale . Features include Admin API, reliable delivery with automatic retries, fanout to multiple endpoints by event type, rate limiting, **declarative GitOps-oriented configuration**, plugins (WASM transforms, signature verification, event validation, function plugins), OpenTelemetry observability, and HMAC signing via built-in plugin . **Multi-tenancy and secret management require a license**; free production licenses are available for eligible non-commercial use . Requires PostgreSQL and Redis.  
**Best for:** Teams wanting a Go-based, plugin-extensible webhook gateway with declarative configuration and GitOps workflows.

### Webhook Tester (tarampampam)
**Stack:** Go / React | **License:** Open Source (per repository)  
Webhook Tester is a free, self-hosted alternative to webhook.site and RequestBin . Generates unique URLs for capturing and inspecting HTTP requests. Features customizable response codes/headers/body, WebSocket real-time UI updates, optional ngrok tunneling, three storage drivers (memory, Redis, filesystem), and a lightweight Docker image. Supports pre-defined static webhook URLs via UUID prefixes.  
**Best for:** Developers and teams wanting a self-hosted, privacy-respecting webhook inspector for debugging and testing.

---

## 📊 Comparison at a Glance

| Software | License | Stack | Inbound | Outbound | Retries/Queue | Self-Hosted | Best For |
|----------|---------|-------|---------|----------|---------------|-------------|----------|
| **Chargebee** | Proprietary | Cloud | ❌ | ✅ Billing events | ✅ 7 attempts | ❌ | Billing event reactions |
| **Svix** | Proprietary (MIT core) | Cloud | ❌ | ✅ Multi-tenant | ✅ | (Enterprise) | Product webhooks + portal |
| **Hookdeck** | Proprietary | Cloud | ✅ Gateway | ✅ (Outpost) | ✅ Queue | (Outpost) | Managed inbound reliability |
| **Convoy** | Open Source | Go/Postgres | ✅ | ✅ | ✅ | ✅ | Unified inbound + outbound |
| **Hook0** | SSPL-1.0 | (Check) | ❌ | ✅ Full parity | ✅ | ✅ | Multi-tenant + EU residency |
| **Outpost** | Apache-2.0 | Go | ❌ | ✅ Multi-dest | ✅ | ✅ | Customer queues/brokers |
| **WebhookX** | Apache-2.0 | Go | ✅ | ✅ | ✅ | ✅ | Plugin + GitOps gateway |
| **Webhook.site** | Proprietary | Cloud | ✅ Inspector | ✅ Actions | ✅ Queues | ❌ | Debug + workflow automation |
| **RequestBin** | Proprietary | Cloud | ✅ Inspector | ✅ Forwarding | (Pro) | ❌ | Affordable debug + forward |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source Real-Time Event Webhook tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source (or clearly note if it is source-available but not OSI-approved, as with Hook0's SSPL). Note that the open-source ecosystem **splits** between **full gateways** (Convoy for unified inbound/outbound, WebhookX for plugin-extensible GitOps), **outbound delivery infrastructure** (Hook0 for multi-tenant parity, Outpost for multi-destination queues/brokers), and **inspection tools** (Webhook Tester). **Hook0** is notable for having no feature gating between cloud and self-hosted under SSPL , while **Outpost** uniquely supports delivery to SQS, RabbitMQ, Pub/Sub, EventBridge, and Kafka beyond plain webhooks . Commercial platforms like Svix and Hookdeck remain far more capable for multi-tenant SaaS webhooks with consumer portals and managed reliability at scale.

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐
