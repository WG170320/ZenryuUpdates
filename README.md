# Zenryu Updates

Remote update manifest for the **Zenryu Toolkit** Android app.

The app fetches [`update.json`](update.json) once per launch and decides whether to show
an update dialog. Edit this file to control every user instantly — no app rebuild needed.

## How the app decides

Let `vc` be the version code the user currently has installed.

| Condition | Result |
|---|---|
| `vc < min_version_code` | **Forced update.** Dialog cannot be dismissed. |
| `vc < latest_version_code` | **Optional update.** Dialog has a "Later" button. |
| otherwise | No dialog. |

`maintenance.enabled: true` shows a blocking maintenance notice instead, with no update button.

If the file is unreachable or malformed, the app shows **nothing** and continues normally.
The update checker can never lock users out.

## Fields

| Field | Type | Meaning |
|---|---|---|
| `latest_version_code` | int | Newest version code published. Users below this see an optional update. |
| `latest_version_name` | string | Shown to the user, e.g. `2.9`. |
| `min_version_code` | int | Lowest version code still allowed. Users below this are **forced** to update. Set to `0` to never force. |
| `title` | string | Dialog heading. |
| `message` | string | Short body text. |
| `whats_new` | string[] | Optional bullet list. Omit or leave empty to hide. |
| `update_url` | string | Where the Update button sends the user. |
| `maintenance.enabled` | bool | Show a blocking maintenance notice. |
| `maintenance.title` | string | Maintenance heading. |
| `maintenance.message` | string | Maintenance body. |

## Recipes

**Announce a normal update** — bump `latest_version_code` and `latest_version_name`,
update `message` and `whats_new`. Leave `min_version_code` alone.

**Force everyone off a broken version** — set `min_version_code` to the first good
version code. Everyone below it is blocked until they update.

**Pause the app during an outage** — set `maintenance.enabled` to `true`.
Set it back to `false` when you are done.

> Change `update_url` with care. If the app is distributed through Google Play, this must
> point at the Play listing. Pointing a Play-distributed build at a direct APK download
> violates the Play *Device and Network Abuse* policy and can get the listing suspended.
