# tailscale-bruno

A [Bruno](https://usebruno.com) collection covering the full Tailscale REST API v2 — all read/GET endpoints across every resource type.

## Endpoints included

| Folder | Requests |
|---|---|
| ACL | Get Policy File |
| Contacts | List Contacts |
| Device Invites | List Device Invites, Get Device Invite |
| Devices | List Devices, Get Device, Get Device Routes, Get Device Attributes, Authorize Device, Delete Device |
| DNS | List Nameservers, Get Preferences, List Search Paths, Get Split DNS, Get DNS Configuration |
| Keys | List Auth Keys, Get Key, Create Auth Key |
| Logging | List Configuration Audit Logs, List Network Flow Logs, Get Stream Status, Get Stream Configuration |
| Posture Integrations | List Posture Integrations, Get Posture Integration |
| Services | List Services, Get Service, List Service Devices, Get Service Device Approval |
| Settings | Get Tailnet Settings |
| User Invites | List User Invites, Get User Invite |
| Users | List Users, Get User |
| Webhooks | List Webhooks, Get Webhook |

## Setup

### 1. Get a Tailscale API token

Go to [Tailscale Admin → Settings → Keys](https://login.tailscale.com/admin/settings/keys) and generate an **API access token**.

### 2. Open the collection in Bruno

Open Bruno → **Open Collection** → select this folder.

### 3. Import the environment

- Click the shield icon (top right) → **Import**
- Select `environments/Tailscale.example.json`
- Fill in your values (see below)
- Save

### 4. Environment variables

| Variable | Description | Example |
|---|---|---|
| `base_url` | Tailscale API base URL | `https://api.tailscale.com/api/v2` |
| `tailnet` | Your tailnet name | `example.ts.net` |
| `access_token` | Your API access token | `tskey-api-...` |
| `device_id` | Device ID (for device-specific calls) | `12345` |
| `device_invite_id` | Device invite ID | |
| `user_id` | User ID (for user-specific calls) | |
| `user_invite_id` | User invite ID | |
| `key_id` | Auth key ID | |
| `log_type` | Log type for streaming endpoints | `configuration` or `network` |
| `service_name` | Service name | |
| `integration_id` | Posture integration ID | |
| `endpoint_id` | Webhook endpoint ID | |

## API Reference

Full Tailscale API docs: https://tailscale.com/api
