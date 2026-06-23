# Connect the dashboard to Google Calendar (one-time setup)

The dashboard reads bookings straight from Google Calendar **in your browser** when you
click **"Connect Calendar"** and sign in. No server, no stored data. To allow this, Google
needs one thing: an **OAuth Client ID** that trusts this site's address.

It is free and takes ~5 minutes. The Client ID is **not a secret** (it's meant to live in the
page), but only Google accounts **you** approve can sign in.

## Steps (in Google Cloud Console)

1. Go to **https://console.cloud.google.com/** and create a project (top bar → New Project),
   e.g. `zip-to-zip-reports`. Select it.
2. **Enable the Calendar API:** APIs & Services → **Library** → search **"Google Calendar API"** → **Enable**.
3. **OAuth consent screen** (APIs & Services → OAuth consent screen):
   - User type: **External** → Create.
   - App name: `Zip To Zip Reports`; user support email + developer email: your email.
   - **Test users** → add the Google account(s) that will sign in (e.g. the one that owns
     `contact@ziptozipmoving.com`). In testing mode only these accounts can connect.
   - Save through the steps (you can leave scopes empty — the page requests read-only at sign-in).
4. **Create the Client ID:** APIs & Services → **Credentials** → **Create credentials** →
   **OAuth client ID**:
   - Application type: **Web application**
   - Name: `Zip To Zip Pages`
   - **Authorized JavaScript origins** → **Add URI** → exactly:

         https://marymd84.github.io

     (origin only — no `/Main-Folder`, no trailing slash)
   - (No redirect URI is needed.)
   - **Create**, then copy the **Client ID** — it looks like
     `1234567890-abc123.apps.googleusercontent.com`.
5. Send that Client ID to Claude (or paste it into `booking_dashboard_live.html` at
   `const GOOGLE_CLIENT_ID = ''`). Then it's live: open the dashboard → **Connect Calendar** →
   pick your account → data loads.

## Notes
- Scope requested: `calendar.readonly` (the page can read events, never edit/delete).
- The account you sign in with must have access to the calendar `contact@ziptozipmoving.com`.
- If you later use a custom domain, add that origin too.
