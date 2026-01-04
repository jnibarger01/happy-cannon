# Quick Start Guide

Get your Jace Pay Dashboard running in under 5 minutes!

## Fastest Way to Deploy (30 seconds)

### Netlify Drop (No Account Required Initially)

1. Open https://app.netlify.com/drop
2. Drag the **entire folder** into the drop zone
3. Get your instant URL: `https://random-name.netlify.app`
4. Bookmark this URL on all your devices!

**That's it!** Your dashboard is now live and accessible from any device.

## Alternative: Local Testing First

### Option 1: Double-Click (Simplest)

1. Double-click `index.html`
2. Opens in your default browser
3. Start using immediately!

**Note**: Data is saved locally to your browser only.

### Option 2: Local Server (For Network Access)

**Using Python** (Mac/Linux/Windows with Python):
```bash
python -m http.server 8000
```

**Using Node.js**:
```bash
npx serve . -p 3000
```

Then open:
- On same device: http://localhost:3000
- On other devices: http://YOUR_IP:3000 (find your IP with `ipconfig` on Windows or `ifconfig` on Mac/Linux)

## First-Time Setup

1. Open the deployed URL
2. Go to **Commissions** tab
3. Add your first RO:
   - Enter RO number
   - Fill in labor and parts amounts
   - Click "Add RO"
4. See your commission calculated instantly!

## Cross-Device Usage

### Sync Data Between Devices

**From Device A** (e.g., your desktop):
1. Click "Export Data"
2. Download the JSON file
3. Email it to yourself OR save to cloud drive

**To Device B** (e.g., your phone):
1. Open the same deployed URL
2. Click "Import Data"
3. Select the JSON file
4. Confirm import
5. All your data is now synced!

## Mobile Installation

Make it feel like a native app:

**iPhone**:
1. Open in Safari
2. Tap Share icon (box with arrow)
3. Scroll and tap "Add to Home Screen"
4. Tap "Add"

**Android**:
1. Open in Chrome
2. Tap menu (⋮)
3. Tap "Add to Home Screen"
4. Tap "Add"

## Deployment URLs Summary

After deploying, your URL will look like:

| Platform | URL Format | Custom Domain |
|----------|------------|---------------|
| Netlify | `https://your-name.netlify.app` | Free |
| Vercel | `https://your-name.vercel.app` | Free |
| GitHub Pages | `https://username.github.io/repo` | Free |

## Recommended Workflow

1. **Daily**: Enter ROs as they come in
2. **Weekly**: Export data for backup
3. **Bi-Weekly**: Review commission summary before paycheck
4. **Monthly**: Enter bonus data and review totals

## Tips

- **Bookmark** your deployment URL on all devices
- **Export** data weekly to prevent loss
- **Import** to sync between work computer and phone
- **Pin** to home screen on mobile for quick access

## Need Help?

- Full deployment guide: See `DEPLOYMENT.md`
- Feature guide: See `README.md`
- Can't deploy? Just use `index.html` directly in browser!

---

**You're Ready!** Start tracking your commissions and bonuses across all your devices.
