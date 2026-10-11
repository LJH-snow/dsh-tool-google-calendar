# @libai168/dsh-tool-google-calendar

Read-only Google Calendar tools for DeepSeek Harness. The package uses the Google Calendar API v3 and never creates, edits, or deletes calendar data.

## Tools

- `gcal_auth_test` — verify access or refresh-token credentials and return a bounded primary-calendar summary.
- `gcal_list_calendars` — list calendars with bounded pagination.
- `gcal_list_events` — list events for a calendar and optional RFC 3339 time window.
- `gcal_search_events` — search events with the Calendar API `q` parameter.
- `gcal_get_event` — read a bounded event summary. Attachments, conference data, and file links are omitted.

All tools are read/search operations. Responses cap descriptions and attendee lists, and pagination tokens are treated as opaque values.

## Configuration

The plugin accepts either a short-lived `accessToken`, or OAuth web-server credentials:

```yaml
- name: 'github:LJH-snow/dsh-tool-google-calendar'
  config:
    clientId: 'your-client-id.apps.googleusercontent.com'
    clientSecret: 'your-client-secret'
    refreshToken: 'your-refresh-token'
```

Use the bundled helper to obtain a refresh token:

```bash
npm exec dsh-google-calendar-auth -- --client-id "..." --client-secret "..."
```

The helper requests the read-only Calendar scopes and prints a Cordis snippet. It does not put the client secret in the authorization URL. Keep client secrets and refresh tokens out of source control.

## Safety

- Production defaults are `https://www.googleapis.com/calendar/v3` and `https://oauth2.googleapis.com/token`.
- Configured endpoints reject credentials, query strings, fragments, local/private/reserved IPs, and DNS results that resolve to blocked ranges.
- Tests inject `fetch` and DNS lookup; they do not call Google or require an account.

## Development

```bash
npm ci
npm run typecheck
npm test
npm run build
npm pack --dry-run
```

See [DEVELOPMENT.md](DEVELOPMENT.md) for release and security details.
