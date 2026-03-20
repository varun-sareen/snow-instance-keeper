# ❄️ SNOW Instance Keeper

**Automatically keeps your ServiceNow Developer Instance alive so it never gets reclaimed.**

ServiceNow reclaims inactive developer instances after **10 days of no activity**. This tool logs into your instance every day via GitHub Actions (completely free), visits key pages, and keeps it active — all without touching your laptop or phone.

---

## 🎯 What It Does

Every day, this automation will:

1. Open your ServiceNow developer instance
2. Wake it up if it's hibernating (instances sleep after a few hours of inactivity)
3. Log in with your credentials
4. Visit the **Update Sets** page
5. Visit the **Homepage** to register activity

All of this runs on GitHub's servers — **not your computer**. You set it up once and forget about it.

---

## 📋 What You Need Before Starting

- [ ] A **GitHub account** (free) — [Sign up here](https://github.com/signup) if you don't have one
- [ ] A **ServiceNow Developer Instance** — [Get one here](https://developer.servicenow.com/dev.do)
- [ ] Your instance URL (looks like `https://dev12345.service-now.com`)
- [ ] Your instance username (usually `admin`)
- [ ] Your instance password

---

## 🚀 Setup Guide (Step by Step)

### Step 1: Create a New Private Repository on GitHub

1. Go to [github.com/new](https://github.com/new)
2. Fill in:
   - **Repository name**: `snow-instance-keeper`
   - **Description**: `Keeps my ServiceNow dev instance alive`
   - **Visibility**: Select **Private** 🔒
3. **Do NOT** check "Add a README file" (we already have one)
4. Click **Create repository**
5. Keep this page open — you'll need the commands shown on it

### Step 2: Upload This Project to GitHub

Open **Terminal** on your Mac (press `Cmd + Space`, type "Terminal", hit Enter) and run these commands **one at a time**:

```bash
# 1. Go to the folder where you saved this project
cd path/to/snow-instance-keeper

# 2. Initialize git
git init

# 3. Add all files
git add .

# 4. Make your first commit
git commit -m "Initial setup: SNOW Instance Keeper"

# 5. Connect to your GitHub repo (replace YOUR_USERNAME with your GitHub username)
git remote add origin https://github.com/YOUR_USERNAME/snow-instance-keeper.git

# 6. Push the code up
git branch -M main
git push -u origin main
```

After this, refresh your GitHub repo page — you should see all the files there!

### Step 3: Add Your Secrets (Credentials)

This is where you securely store your ServiceNow login info. GitHub encrypts these — **nobody can see them**, not even you after saving.

1. Go to your repo on GitHub: `https://github.com/YOUR_USERNAME/snow-instance-keeper`
2. Click **Settings** (tab at the top of the repo)
3. In the left sidebar, click **Secrets and variables** → **Actions**
4. Click **New repository secret** and add these **one at a time**:

   | Name | Value | Example |
   |------|-------|---------|
   | `SNOW_INSTANCE_URL` | Your full instance URL | `https://dev12345.service-now.com` |
   | `SNOW_USERNAME` | Your instance username | `admin` |
   | `SNOW_PASSWORD` | Your instance password | `your-password-here` |

   For each one:
   - Click **New repository secret**
   - Type the **Name** exactly as shown (all caps, underscores)
   - Paste the **Value**
   - Click **Add secret**

### Step 4: Test It! (Run Manually)

Let's make sure everything works before relying on the daily schedule:

1. Go to your repo on GitHub
2. Click the **Actions** tab (at the top)
3. You'll see **"Keep SNOW Instance Alive"** in the left sidebar — click it
4. Click the **"Run workflow"** button (dropdown on the right)
5. Click the green **"Run workflow"** button

Now watch it run! Click on the running workflow to see live logs. You should see messages like:
```
🚀 Starting SNOW Instance Keeper...
📍 Target instance: https://dev12345.service-now.com
✅ Instance appears to be running!
✅ Login successful!
✅ Update Sets page loaded successfully!
🎉 SUCCESS! Instance is alive and kicking!
```

### Step 5: You're Done! 🎉

The automation will now run **every day at 7:00 AM UTC** automatically. You don't need to do anything else!

To change the schedule time, edit `.github/workflows/keep-alive.yml` and update the cron line. Some handy presets:

| Time (UTC) | Time (IST) | Time (CET/Berlin) | Cron |
|------------|------------|-------------------|------|
| 3:00 AM | 8:30 AM | 4:00 AM | `0 3 * * *` |
| 7:00 AM | 12:30 PM | 8:00 AM | `0 7 * * *` |
| 14:00 PM | 7:30 PM | 3:00 PM | `0 14 * * *` |

---

## 🔧 Optional: Test Locally on Your Mac

If you want to see the automation run on your own machine (with a visible browser!):

```bash
# 1. Install dependencies
npm install

# 2. Install the browser
npx playwright install chromium

# 3. Set your credentials (temporary, just for this terminal session)
export SNOW_INSTANCE_URL="https://dev12345.service-now.com"
export SNOW_USERNAME="admin"
export SNOW_PASSWORD="your-password"

# 4. Run with visible browser (so you can watch!)
npm run test-run
```

---

## 🤝 Sharing With a Friend

Want to share this with a colleague? Here's what to tell them:

1. **If your repo is private**: Go to Settings → Collaborators → Add people → Type their GitHub username
2. They should then **fork** the repo to their own account
3. They add their **own secrets** (Step 3 above) with their own instance URL and credentials
4. That's it! Their fork will run independently with their own schedule

Alternatively, you can make the repo **public** temporarily for them to fork it, then make it private again.

---

## ❓ Troubleshooting

| Problem | Solution |
|---------|----------|
| Workflow doesn't run | Go to Actions tab → make sure workflows are enabled |
| Login fails | Double-check your secrets — the Name must match exactly |
| Instance won't wake up | Try running manually a second time; sometimes first wake-up takes long |
| "No credentials" error | Make sure all 3 secrets are added (check for typos in names) |

---

## 📁 Project Structure

```
snow-instance-keeper/
├── .github/
│   └── workflows/
│       └── keep-alive.yml    ← GitHub Actions schedule & workflow
├── scripts/
│   └── keep-alive.js         ← The actual automation script
├── .gitignore                 ← Keeps sensitive files out of git
├── package.json               ← Project dependencies
└── README.md                  ← You are here!
```

---

## 💡 Want to Add More Pages?

Open `scripts/keep-alive.js` and look for "Step 4". Copy that pattern to add more pages:

```javascript
// Example: Also visit Incident list
log("📋 Opening Incidents...");
await page.goto(`${baseUrl}/incident_list.do`, {
  waitUntil: "networkidle",
  timeout: 30000,
});
await sleep(3000);
```

---

**Built with ❤️ to save ServiceNow developers from losing their instances.**
