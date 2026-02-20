# tailscale-bruno

A [Bruno](https://usebruno.com) collection covering the full Tailscale REST API v2 — read and write endpoints across every resource type.

---

## Requirements

- [Bruno](https://usebruno.com) desktop app (free, open source)
- A Tailscale account with admin access
- A Tailscale API access token (see Step 1 below)

---

## Setup

### Step 1 — Get a Tailscale API token

1. Go to [https://login.tailscale.com/admin/settings/keys](https://login.tailscale.com/admin/settings/keys)
2. Click **Generate access token**
3. Give it a name and copy the token — you won't see it again

> Your token starts with `tskey-api-...`

---

### Step 2 — Get Bruno

Download and install Bruno from [https://usebruno.com](https://usebruno.com). It's free and runs entirely on your machine — no account required.

---

### Step 3 — Get this collection

**Option A: Clone with git**
```bash
git clone https://github.com/ccatprd/tailscale-bruno.git
```

**Option B: Download ZIP**

Click the green **Code** button on this page → **Download ZIP** → extract it somewhere on your machine.

---

### Step 4 — Open the collection in Bruno

1. Open Bruno
2. Click **Open Collection**
3. Navigate to and select the `tailscale-bruno` folder
4. The collection will appear in the left sidebar

---

### Step 5 — Set up your environment

The collection uses environment variables so your credentials and IDs are stored in one place and reused across all requests.

1. In Bruno, click the **shield icon** in the top-right corner
2. Click **Import**
3. Select the file `environments/Tailscale.example.json` from the collection folder
4. The **Tailscale** environment will appear — click it to open it
5. Fill in your values:
   - `tailnet` → your tailnet name, e.g. `example.ts.net`
   - `access_token` → the token you generated in Step 1
6. Click **Save**
7. Select **Tailscale** from the environment dropdown in the top-right corner

> `base_url` is pre-filled and doesn't need changing. Leave other variables blank for now — they get filled in as you work.

---

### Step 6 — Make your first request

1. In the left sidebar, expand **Devices**
2. Click **List Devices**
3. Click the **Send** button (arrow icon, top right of the request panel)
4. You should see a JSON response listing all devices in your tailnet

---

## Example flow: List devices → Set a posture attribute

This walks through a real end-to-end example using two requests in sequence.

### What we're doing

We'll list all devices to find a device ID, then use that ID to write a custom posture attribute to that device.

---

### 1. Run "List Devices"

- Expand **Devices** in the sidebar → click **List Devices** → click **Send**
- You'll get a response like:

```json
{
  "devices": [
    {
      "id": "123456789",
      "name": "my-macbook.example.ts.net",
      ...
    }
  ]
}
```

**The collection does this automatically:** a post-response script runs after List Devices and sets `device_id` in your environment to the first device in the list. You'll see a confirmation in the **Console** tab, e.g.:

```
device_id set to: 123456789 ( my-macbook.example.ts.net )
```

> If you want to target a **different** device, click the shield icon → edit `device_id` manually with the ID you want from the response.

---

### 2. Set a posture attribute on that device

Posture attributes are custom key-value pairs you can attach to devices — useful for tagging compliance state, ownership, environment, etc.

1. In the sidebar, expand **Devices → Posture Attributes**
2. Click **Set Device Posture Attribute**
3. Before sending, set `attribute_key` in your environment (shield icon) to your attribute name. Custom attributes must be prefixed with `custom:`, followed by letters, numbers, or underscores — **no hyphens**. e.g.:
   - `custom:compliance_status`
   - `custom:owner`
   - `custom:patch_level`
4. Edit the request body to set your value:

```json
{
  "value": "compliant",
  "expiry": "2026-12-31T00:00:00Z"
}
```

> `expiry` is optional — remove it if you want the attribute to persist indefinitely.

5. Click **Send**
6. **The response will be `null` — this means success.** Tailscale returns an empty body on 200 OK for this endpoint. It is not an error.

---

### 3. Verify it was set

1. Click **Get Device Attributes** (under **Devices**)
2. Click **Send**
3. Your attribute will appear in the response under `customAttributes`

---

## All endpoints

| Folder | Requests |
|---|---|
| ACL | Get Policy File, Update Policy File |
| Contacts | List Contacts |
| Device Invites | List Device Invites, Get Device Invite |
| Devices | List Devices, Get Device, Get Device Routes, Get Device Attributes, Authorize Device, Expire Device Key, Set Device Routes, Set Device Tags, Delete Device |
| Devices / Posture Attributes | Set Device Posture Attribute, Delete Device Posture Attribute |
| DNS | List Nameservers, Get Preferences, List Search Paths, Get Split DNS, Get DNS Configuration |
| Keys | List Auth Keys, Get Key, Create Auth Key, Delete Key |
| Logging | List Configuration Audit Logs, List Network Flow Logs, Get Stream Status, Get Stream Configuration |
| Posture Integrations | List Posture Integrations, Get Posture Integration |
| Services | List Services, Get Service, List Service Devices, Get Service Device Approval |
| Settings | Get Tailnet Settings, Update Tailnet Settings |
| User Invites | List User Invites, Get User Invite |
| Users | List Users, Get User |
| Users / Actions | Suspend User, Delete User, Restore User |
| Webhooks | List Webhooks, Get Webhook, Create Webhook, Delete Webhook |

---

## Environment variables reference

| Variable | Description | Example |
|---|---|---|
| `base_url` | Tailscale API base URL (pre-filled) | `https://api.tailscale.com/api/v2` |
| `tailnet` | Your tailnet name | `example.ts.net` |
| `access_token` | Your API access token | `tskey-api-...` |
| `device_id` | Device ID — auto-set by List Devices, or set manually | `123456789` |
| `attribute_key` | Posture attribute key (must start with `custom:`) | `custom:compliance-status` |
| `user_id` | User ID for user-specific calls | |
| `key_id` | Auth key ID | |
| `endpoint_id` | Webhook endpoint ID | |
| `log_type` | Log type for streaming endpoints | `configuration` or `network` |
| `service_name` | Service name | |
| `integration_id` | Posture integration ID | |
| `device_invite_id` | Device invite ID | |
| `user_invite_id` | User invite ID | |

---

## API reference

Full Tailscale API documentation: [https://tailscale.com/api](https://tailscale.com/api)
