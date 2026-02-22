# AASR District Central East, South & North — Public Calendar

A simple, mobile-friendly website that displays the District's Google Calendar of chapter meeting dates. Designed for accessibility — large text, high contrast, and easy navigation so any member can view upcoming meetings from their phone or computer without needing a Google account.

**Live site:** [kevinsing999.github.io/public-calendar](https://kevinsing999.github.io/public-calendar/)

---

## How It Works

- Events are pulled directly from the District's Google Calendar using the free Google Calendar API
- The site uses [FullCalendar](https://fullcalendar.io/) (an open-source calendar library) to display events in three views: Agenda, Month, and Week
- Clicking an event shows its details in a popup — no redirect to Google Calendar
- Hosted for free on GitHub Pages with HTTPS

---

## Setup Instructions

### Step 1: Make the Google Calendar Public

The calendar must be shared publicly with full event details visible (not just free/busy).

1. Open [Google Calendar](https://calendar.google.com) and sign in with the account that owns the District calendar
2. In the left sidebar, find the District calendar, click the **three dots** next to it, then click **Settings and sharing**
3. Scroll to **Access permissions for events**
4. Check **"Make available to public"**
5. Change the dropdown from **"See only free/busy (hide details)"** to **"See all event details"**
6. Click **Save** (or the settings auto-save)

### Step 2: Create a Google Calendar API Key

The website needs an API key to read events from Google Calendar. This is free — Google allows up to 1 million requests per day at no cost.

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. If you don't have a Google Cloud project, click **Select a project** at the top → **New Project**
   - Name it something like `DCE Calendar Website`
   - Click **Create**
3. Make sure the new project is selected at the top of the page
4. In the left menu, go to **APIs & Services → Library**
5. Search for **Google Calendar API** and click on it
6. Click the **Enable** button
7. Go to **APIs & Services → Credentials** (in the left menu)
8. Click **Create Credentials** at the top → select **API Key**
9. Your API key will be displayed — **copy it**
10. (Recommended) Secure the key:
    - Click **Restrict Key** (or click the key name to edit it)
    - Under **Application restrictions**, select **HTTP referrers (websites)**
    - Add your GitHub Pages URL: `kevinsing999.github.io/*`
    - Under **API restrictions**, select **Restrict key** → check **Google Calendar API** only
    - Click **Save**

### Step 3: Add the API Key to the Website

1. Open `index.html` in a text editor
2. Find this line (around line 80):
   ```javascript
   googleCalendarApiKey: 'YOUR_API_KEY_HERE',
   ```
3. Replace `YOUR_API_KEY_HERE` with your actual API key:
   ```javascript
   googleCalendarApiKey: 'AIzaSyB1234567890abcdefghijklmnop',
   ```
4. Save the file

### Step 4: Deploy to GitHub Pages

1. Commit and push your changes:
   ```bash
   git add -A
   git commit -m "Add calendar website with API key"
   git push origin main
   ```
2. Go to the repository on GitHub: [github.com/kevinsing999/public-calendar](https://github.com/kevinsing999/public-calendar)
3. Click **Settings** (tab at the top)
4. In the left sidebar, click **Pages**
5. Under **Source**, select **Deploy from a branch**
6. Select the **main** branch and **/ (root)** folder
7. Click **Save**
8. Wait 1–2 minutes, then visit: [kevinsing999.github.io/public-calendar](https://kevinsing999.github.io/public-calendar/)

---

## Testing Locally

You can open `index.html` directly in your browser to check the layout and styling. However, the calendar events won't load locally unless you've configured the API key and the calendar is set to public.

For full local testing:
1. Complete Steps 1–3 above
2. Open `index.html` in Chrome/Firefox/Edge
3. Events should appear if the API key has no domain restrictions (or if you temporarily remove the HTTP referrer restriction)

---

## Customisation

### Changing the Calendar

To point at a different Google Calendar, edit `index.html` and change the `googleCalendarId` value:

```javascript
googleCalendarId: 'your-calendar-id@group.calendar.google.com'
```

You can find a calendar's ID in Google Calendar → Settings → calendar name → **Integrate calendar** section.

### Changing Colours

The main colour scheme is defined in `style.css`. Search for `#1a2a5e` (the dark blue used in the header and buttons) and replace it with your preferred colour.

### Changing the Default View

The default view is `listMonth` (Agenda). To change it, edit the `initialView` line in `index.html`. Options:
- `listMonth` — Agenda list for the current month
- `dayGridMonth` — Traditional month grid
- `listWeek` — Agenda list for the current week

---

## File Structure

```
public-calendar/
├── index.html      # Complete calendar application
├── style.css       # Responsive, accessible styles
└── README.md       # This file
```

## Technology

| Component | Technology | Cost |
|-----------|-----------|------|
| Calendar display | [FullCalendar v6](https://fullcalendar.io/) (MIT license) | Free |
| Event data | Google Calendar API | Free (1M requests/day) |
| Hosting | GitHub Pages | Free |
| Build tools | None — plain HTML/CSS/JS | N/A |
