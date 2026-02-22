# AASR District Central East, South & North — Public Calendar

A simple, mobile-friendly website that displays the District's Google Calendar of chapter meeting dates. Designed for accessibility — large text, high contrast, and easy navigation so any member can view upcoming meetings from their phone or computer without needing a Google account.

**Live site:** [kevinsing999.github.io/public-calendar](https://kevinsing999.github.io/public-calendar/)

---

## How It Works

- Events are pulled directly from the District's Google Calendar using the free Google Calendar API
- The site uses [FullCalendar](https://fullcalendar.io/) (an open-source calendar library) to display events in three views: Agenda, Month, and Week
- Clicking an event shows its details in a popup — no redirect to Google Calendar
- Each event has a **Save to Calendar** button that downloads an `.ics` file (works with Apple Calendar, Outlook, Google Calendar, etc.)
- The API key is stored as a GitHub repository secret and injected at deploy time via GitHub Actions — it never appears in the source code
- Hosted for free on GitHub Pages with HTTPS

---

## Prerequisites

### 1. Make the Google Calendar Public

The calendar must be shared publicly with full event details visible (not just free/busy).

1. Open [Google Calendar](https://calendar.google.com) and sign in with the account that owns the District calendar
2. In the left sidebar, find the District calendar, click the **three dots** next to it, then click **Settings and sharing**
3. Scroll to **Access permissions for events**
4. Check **"Make available to public"**
5. Change the dropdown from **"See only free/busy (hide details)"** to **"See all event details"**
6. Settings auto-save

### 2. Google Calendar API Key

The API key is already configured and stored as a GitHub repository secret (`GOOGLE_CALENDAR_API_KEY`). It is:
- Restricted to the `kevinsing999.github.io` domain only
- Restricted to the Google Calendar API only
- Injected into the deployed site automatically by the GitHub Actions workflow

If the key ever needs to be rotated:
1. Create a new API key in [Google Cloud Console](https://console.cloud.google.com/) → APIs & Services → Credentials
2. Apply the same restrictions (HTTP referrers: `kevinsing999.github.io/*`, API: Google Calendar API only)
3. Update the repository secret: Settings → Secrets and variables → Actions → `GOOGLE_CALENDAR_API_KEY`
4. Push any commit to trigger a redeployment

---

## Deployment

The site deploys automatically via GitHub Actions on every push to `main`.

The workflow (`.github/workflows/deploy.yml`):
1. Checks out the code
2. Replaces the API key placeholder in `index.html` with the repository secret
3. Deploys to GitHub Pages

No manual deployment steps are needed — just push to `main`.

---

## Custom Domain (Optional)

To use a custom domain instead of `kevinsing999.github.io/public-calendar`:

1. Buy a domain from any registrar
2. Add DNS records pointing to GitHub Pages:
   - 4 `A` records for the root domain: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - 1 `CNAME` record for `www` pointing to `kevinsing999.github.io`
3. Add a `CNAME` file to the repo containing your domain name
4. Configure the domain in GitHub → Settings → Pages
5. Update the API key restriction to include the new domain

---

## Features

| Feature | Description |
|---------|-------------|
| **Agenda view** | Default. Vertical list of all events this month |
| **Month view** | Traditional calendar grid — click any date to drill down |
| **Week view** | Vertical list of this week's events |
| **Event details** | Click any event to see title, date/time, location, and description in a popup |
| **Save to Calendar** | Download an `.ics` file for any event — works with Apple Calendar, Outlook, Google Calendar |
| **Mobile responsive** | Adapts to any screen size, large touch targets |
| **Print friendly** | `Ctrl+P` prints a clean agenda without navigation buttons |

---

## Customisation

### Changing the Calendar

Edit `index.html` and change the `googleCalendarId` value:

```javascript
googleCalendarId: 'your-calendar-id@group.calendar.google.com'
```

Find a calendar's ID in Google Calendar → Settings → calendar name → **Integrate calendar**.

### Changing Colours

The colour palette is defined as CSS variables at the top of `style.css`:

```css
:root {
  --navy: #0d132d;    /* Header, footer, active buttons */
  --red: #b50000;     /* Accent — header rule, event dots, hover states */
  --pale: #d9dee8;    /* Borders */
  --light: #f3f3f3;   /* Light surfaces */
  --white: #ffffff;   /* Background */
  --text: #293340;    /* Body text */
  --muted: #6b7280;   /* Secondary text */
}
```

Change these values to restyle the entire site.

### Changing the Default View

The default view is `listMonth` (Agenda) on desktop and `listWeek` on mobile. To change it, edit the `initialView` logic in `index.html`.

---

## File Structure

```
public-calendar/
├── .github/
│   └── workflows/
│       └── deploy.yml    # GitHub Actions — injects API key and deploys
├── index.html            # Complete calendar application
├── style.css             # Responsive styles (White House-inspired palette)
└── README.md             # This file
```

## Technology

| Component | Technology | Cost |
|-----------|-----------|------|
| Calendar display | [FullCalendar v6](https://fullcalendar.io/) (MIT license) | Free |
| Event data | Google Calendar API | Free (1M requests/day) |
| Typography | [Instrument Serif + Sans](https://fonts.google.com/?query=instrument) (Google Fonts) | Free |
| Hosting | GitHub Pages | Free |
| Deployment | GitHub Actions | Free |
| Build tools | None — plain HTML/CSS/JS | N/A |
