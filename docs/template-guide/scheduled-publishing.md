# Scheduled Publishing

Scheduled publishing lets you set a future date and time for a template or content block version to go live automatically. Instead of manually publishing at the right moment, you schedule it in advance and TemplateTo handles the rest.

## How It Works

When you schedule a version for publishing:

1. A publish record is created with a future date
2. The current published version remains active until the scheduled time
3. At the scheduled time, any render request automatically picks up the new version — no cron jobs, no delay

This works because TemplateTo's version resolution query checks `PublishDate <= NOW()` at render time. When the clock passes the scheduled time, the new version becomes the resolved published version instantly.

!!! info "No delay"
    Activation is instantaneous at the scheduled time. There is no background job or polling — the version becomes active the moment a render request is made after the scheduled time.

## Scheduling via the UI

### From the History Panel

1. Open a template or content block in the editor
2. Open the **History** panel (drawer)
3. Find the version you want to schedule
4. Click the **clock icon** next to the Publish button
5. Select the date and time in the popover
6. Click **Schedule**

The version will show a blue **Scheduled** badge with the publish date displayed below.

### From the Version Status Dropdown

When viewing a scheduled version, the version status dropdown in the header shows **Scheduled** in blue. From there you can:

- **Publish Now** — immediately publishes the version (cancels the schedule)
- **Cancel Schedule** — removes the schedule without publishing

### Current Versions Section

Scheduled versions appear in the **Current Versions** section of the History panel alongside Published and Draft versions, with a clock icon and the scheduled publish date.

## Scheduling via the API

Use the existing publish endpoint with a `publishDate` query parameter:

```
PATCH /Content/{id}/version/{versionId}/publish?publishDate=2026-03-25T10:00:00Z
```

- The `publishDate` must be a UTC ISO 8601 datetime string
- If `publishDate` is in the future, the version is scheduled (not immediately published)
- If `publishDate` is omitted or in the past, the version is published immediately

### Cancel a Schedule

```
DELETE /Content/{id}/version/{versionId}/schedule
```

This removes the scheduled publish record. The current published version remains unchanged.

## Behavior Rules

### One Schedule at a Time

Each template or content block can have **one pending schedule** at a time. If you schedule a new version while another is already scheduled, the old schedule is replaced.

### Immediate Publish Cancels Schedules

When you immediately publish any version (via UI or API without a future date), any pending scheduled publish for that content is automatically cancelled. This prevents a stale schedule from later overriding your immediate publish.

| Scenario | Behavior |
|---|---|
| Schedule v2, then immediately publish v3 | v3 goes live now; v2's schedule is auto-cancelled |
| Schedule v2, then schedule v3 | v2's schedule is replaced; only v3 is scheduled |
| Schedule then cancel | Schedule is removed; current published version unchanged |
| Schedule a version that is also the draft | Valid — the version shows both Draft and Scheduled badges |

### API Consumers

If you use the REST API to render templates, scheduled publishing works transparently. The version resolution happens at query time, so your render calls will automatically pick up the scheduled version once the time arrives.

## Checking Schedule Status

### Via API

The version list endpoint returns `isScheduled` and `scheduledPublishDate` fields for each version:

```
GET /Content/{id}/version
```

```json
{
    "contentVersionId": "cntver_abc123",
    "isScheduled": true,
    "scheduledPublishDate": "2026-03-25T10:00:00Z",
    "isDraft": true,
    "isPublished": false
}
```

### Via UI

Scheduled versions display:

- A blue **Scheduled** badge in the History panel and version list
- The scheduled date and time below the version details
- A **Scheduled** status in the version status dropdown (header)