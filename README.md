# 📅 Wiki Club SATI – International Days Content Planner

A powerful, feature-rich content planning tool for Wikimedia editors and content creators to plan, nominate, and schedule social media posts around international observance days.

**Live Demo:** Inspired by [Suyash Dwivedi's International Days Calendar](https://suyashdwivedi.github.io/international_days.html)

---

## ✨ Features

| Feature | Description |
|---|---|
| 📅 Interactive Calendar | Month-by-month calendar with all major international days |
| 📝 Nominate Button | Click any day to nominate yourself for creating a post |
| 📆 Google Calendar | One-click add reminder to Google Calendar |
| 📥 ICS Download | Download `.ics` file for Outlook, Apple Calendar, etc. |
| 🔔 Browser Notifications | Reminders 1 day, 2 days, and 7 days before events |
| ✨ AI Caption Generator | Generate platform-specific captions (Instagram, LinkedIn, Twitter, etc.) |
| 🏷️ Content Type Tags | Events auto-tagged: Health, Environment, Rights, Culture, etc. |
| 📋 Upcoming Nominations | Sidebar showing all your upcoming nominations |
| 🏆 Team Leaderboard | Rankings by team name based on nomination count |
| 🔗 Share Reminder Link | Shareable deep-link URL for any event |
| 📊 Export CSV | Download all nominations as a spreadsheet |
| 📆 Export ICS | Export all nominations to a calendar file |
| 🔥 Trending Soon | Auto-scrolling banner of events in the next 14 days |
| 📖 Wikipedia Status | Live check if a Wikipedia article exists for each event |
| 🔐 Admin Panel | Password-protected view of all team nominations |
| 🌙 Dark/Light Mode | Toggle between themes |
| 🔍 Search | Search any international day across the full year |

---

## 🚀 Step-by-Step: Deploy to GitHub Pages

### Step 1: Create a GitHub Account
If you don't have one, go to [github.com](https://github.com) and sign up for free.

---

### Step 2: Create a New Repository

1. Click the **+** button in the top-right corner of GitHub
2. Click **"New repository"**
3. Fill in the details:
   - **Repository name:** `international-days-planner` (or any name you like)
   - **Description:** Wiki Club SATI Content Planner
   - **Visibility:** ✅ Public (required for free GitHub Pages)
   - **Add README:** ✅ Check this box
4. Click **"Create repository"**

---

### Step 3: Upload the HTML File

**Option A – Using GitHub's Web Interface (easiest):**

1. Open your new repository on GitHub
2. Click **"Add file"** → **"Upload files"**
3. Drag and drop `international_days.html` into the upload area
4. In the "Commit changes" section at the bottom:
   - Message: `Add Wiki Club SATI International Days Content Planner`
5. Click **"Commit changes"**

**Option B – Using GitHub Desktop:**

1. Download [GitHub Desktop](https://desktop.github.com/)
2. Clone your repository to your computer
3. Copy `international_days.html` into the cloned folder
4. In GitHub Desktop, write a commit message and click **Commit to main**
5. Click **Push origin**

**Option C – Using Git command line:**

```bash
git clone https://github.com/YOUR_USERNAME/international-days-planner
cd international-days-planner
cp /path/to/international_days.html .
git add international_days.html
git commit -m "Add Wiki Club SATI Content Planner"
git push origin main
```

---

### Step 4: Enable GitHub Pages

1. In your repository, click **"Settings"** (top menu)
2. Scroll down to the **"Pages"** section in the left sidebar
3. Under **"Source"**, select:
   - Branch: **`main`**
   - Folder: **`/ (root)`**
4. Click **"Save"**
5. Wait 2–3 minutes for GitHub to build your site

---

### Step 5: Access Your Live Site

After the build completes, your site will be live at:

```
https://YOUR_USERNAME.github.io/international-days-planner/international_days.html
```

Or if you rename the file to `index.html`:

```
https://YOUR_USERNAME.github.io/international-days-planner/
```

**Tip:** Rename `international_days.html` to `index.html` to use the shorter URL.

---

## 🔐 Admin Panel

The default admin password is: **`wikiclutsati`**

To change it:
1. Open `international_days.html` in a text editor
2. Find this line: `if(pass!=='wikiclutsati')`
3. Change `wikiclutsati` to your desired password

---

## ✨ AI Caption Generator Setup (Optional)

The tool includes a smart template-based caption generator that works out of the box.

For **real AI-powered captions** using Claude (Anthropic):

1. Get an API key from [console.anthropic.com](https://console.anthropic.com)
2. In the tool, click **AI Caption** on any event
3. Paste your API key in the field provided
4. Your key is stored only in your browser's localStorage

> ⚠️ **Note:** Direct browser-to-Anthropic API calls may be blocked by CORS in some browsers. If this happens, the tool automatically falls back to the smart template system which generates high-quality captions.

**For advanced users – CORS Proxy with Cloudflare Workers:**

If you want real AI captions without CORS issues, you can set up a free Cloudflare Worker as a proxy:

1. Go to [workers.cloudflare.com](https://workers.cloudflare.com)
2. Create a new Worker with this code:

```javascript
export default {
  async fetch(request) {
    if (request.method === 'OPTIONS') {
      return new Response(null, {
        headers: {
          'Access-Control-Allow-Origin': '*',
          'Access-Control-Allow-Methods': 'POST',
          'Access-Control-Allow-Headers': 'Content-Type, x-api-key, anthropic-version',
        }
      });
    }
    const body = await request.json();
    const apiKey = request.headers.get('x-api-key');
    const res = await fetch('https://api.anthropic.com/v1/messages', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json', 'x-api-key': apiKey, 'anthropic-version': '2023-06-01' },
      body: JSON.stringify(body)
    });
    const data = await res.json();
    return new Response(JSON.stringify(data), {
      headers: { 'Access-Control-Allow-Origin': '*', 'Content-Type': 'application/json' }
    });
  }
}
```

3. Copy your Worker URL (e.g., `https://my-proxy.myname.workers.dev`)
4. In `international_days.html`, find the `fetch('https://api.anthropic.com/v1/messages'` line and replace it with your Worker URL

---

## 📧 Email Reminder Setup (Optional – Advanced)

The tool currently supports:
- ✅ Browser push notifications (works automatically)
- ✅ Google Calendar reminders (one-click)
- ✅ ICS file download (import to any email/calendar app)

For **automated email reminders**, you can add EmailJS:

1. Create a free account at [emailjs.com](https://www.emailjs.com)
2. Create an email template with variables: `{{to_email}}`, `{{event_name}}`, `{{event_date}}`, `{{days_until}}`
3. Add the EmailJS SDK to the `<head>` of `international_days.html`:

```html
<script src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
<script>emailjs.init('YOUR_PUBLIC_KEY');</script>
```

4. In the `submitNomination()` function, add after saving to localStorage:

```javascript
emailjs.send('YOUR_SERVICE_ID', 'YOUR_TEMPLATE_ID', {
  to_email: nom.email,
  to_name: nom.name,
  event_name: nom.eventName,
  event_date: nom.eventDate,
});
```

---

## 🛠️ Customization

### Change the Logo
Find this line in the HTML and replace the `src`:
```html
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/bb/WIKI_CLUB_SATI_Logo.svg/..." 
```

### Add More International Days
Find the `internationalDays` object in the JavaScript section and add entries:
```javascript
"MM-DD": [{
  name: "Name of the Day",
  desc: "Description of the observance.",
  un: true,  // true if UN-recognized, false otherwise
  wiki: "Wikipedia_Article_Title",
  tags: ["environment", "health"]  // choose from available tags
}]
```

### Available Content Tags
`environment`, `health`, `culture`, `science`, `peace`, `rights`, `education`, `women`, `children`, `food`, `wildlife`, `media`, `ocean`, `music`, `history`, `language`, `celebration`

### Change Color Scheme
At the top of the `<style>` section, modify the CSS variables:
```css
:root {
  --p: #7c3aed;   /* Primary purple - change to your color */
  --c: #06b6d4;   /* Cyan accent - change to your color */
}
```

---

## 📁 File Structure

```
your-repo/
├── international_days.html   ← Main tool (rename to index.html for shorter URL)
└── README.md                 ← This guide
```

---

## 🤝 Contributing

1. Fork the repository
2. Make your changes
3. Submit a Pull Request

Feel free to add more international days, improve the UI, or add new features!

---

## 📝 Credits

- **Created by:** [Uday Dongre](https://meta.wikimedia.org/wiki/User:Shoot_stufz)
- **Inspired by:** [Suyash Dwivedi's International Days Calendar](https://suyashdwivedi.github.io/international_days.html)
- **Logo:** [Wiki Club SATI](https://commons.wikimedia.org/wiki/File:WIKI_CLUB_SATI_Logo.svg)
- **Updated:** April 14, 2026

---

## 📜 License

This project is open-source and available under the [MIT License](https://opensource.org/licenses/MIT). Free to use, modify, and distribute!
