# Deployment Guide - Jace Pay Dashboard

This guide will help you deploy the Jace Pay Dashboard to multiple platforms for cross-platform access.

## Features

- **Cross-Platform Access**: Works on desktop, mobile, and tablet devices
- **Data Export/Import**: Save your data as JSON and transfer between devices
- **Offline Capable**: Works without internet connection (after initial load)
- **Privacy First**: All data stored locally in your browser (no server storage)

## Quick Deploy Options

### Option 1: Netlify (Recommended - Free)

1. **Create a Netlify account** at https://www.netlify.com
2. **Deploy via Git**:
   - Push this code to a GitHub repository
   - Connect Netlify to your GitHub account
   - Select your repository
   - Netlify will auto-detect the configuration from `netlify.toml`
   - Click "Deploy"

3. **Deploy via Drag & Drop**:
   - Go to https://app.netlify.com/drop
   - Drag the entire folder into the drop zone
   - Get your instant URL (e.g., `https://your-app.netlify.app`)

**Custom Domain** (optional):
- Go to Site Settings → Domain Management
- Add your custom domain
- Configure DNS as instructed

### Option 2: Vercel (Free)

1. **Create a Vercel account** at https://vercel.com
2. **Install Vercel CLI** (optional):
   ```bash
   npm install -g vercel
   vercel login
   vercel
   ```

3. **Deploy via Web UI**:
   - Push code to GitHub
   - Go to https://vercel.com/new
   - Import your repository
   - Vercel will auto-detect the configuration from `vercel.json`
   - Click "Deploy"

### Option 3: GitHub Pages (Free)

1. **Push to GitHub**:
   ```bash
   git add .
   git commit -m "Deploy to GitHub Pages"
   git push origin main
   ```

2. **Enable GitHub Pages**:
   - Go to your repository on GitHub
   - Settings → Pages
   - Source: Deploy from branch `main` / root folder
   - Save
   - Your site will be live at `https://yourusername.github.io/repository-name`

### Option 4: Local Network Access

Use this for quick local testing or home network access:

**Using Python** (built-in on Mac/Linux):
```bash
cd /path/to/jace-pay-dashboard
python3 -m http.server 8000
```

**Using Node.js**:
```bash
npx serve .
```

**Using PHP** (if installed):
```bash
php -S localhost:8000
```

Then access at: `http://localhost:8000` or `http://YOUR_LOCAL_IP:8000` from other devices on your network.

### Option 5: Cloud Storage (AWS S3, Google Cloud Storage)

**AWS S3 Static Website**:
1. Create S3 bucket with public access
2. Enable "Static website hosting"
3. Upload `index.html` and all files
4. Set permissions to public read
5. Access via S3 website endpoint

**Google Cloud Storage**:
1. Create bucket with public access
2. Upload files
3. Set bucket permissions
4. Access via GCS public URL

## Data Management

### Export Your Data

1. Click **"Export Data"** button in the Commissions tab
2. File downloads as `jace-pay-dashboard-YYYY-MM-DD.json`
3. Store this file in a safe location (cloud drive, backup)

### Import Your Data (Transfer Between Devices)

1. Export data from Device A
2. Transfer the JSON file to Device B (email, cloud drive, USB)
3. Open the dashboard on Device B
4. Click **"Import Data"** button
5. Select your JSON file
6. Confirm the import

### Backup Best Practices

- **Weekly Exports**: Export data weekly to prevent loss
- **Cloud Backup**: Store exports in Google Drive, Dropbox, or OneDrive
- **Version Control**: Keep multiple dated exports for history
- **Multi-Device Sync**: Manually sync by exporting from one device and importing to another

## Browser Compatibility

✅ **Fully Supported**:
- Chrome/Edge (Desktop & Mobile)
- Firefox (Desktop & Mobile)
- Safari (Desktop & Mobile)
- Opera

⚠️ **Limited**:
- Internet Explorer 11 (basic functionality, no modern features)

## Mobile Installation (PWA)

Make the app feel like a native mobile app:

**iOS (Safari)**:
1. Open the deployed URL
2. Tap Share button
3. Tap "Add to Home Screen"
4. Name it "Jace Pay"
5. Tap "Add"

**Android (Chrome)**:
1. Open the deployed URL
2. Tap the menu (3 dots)
3. Tap "Add to Home Screen"
4. Name it "Jace Pay"
5. Tap "Add"

## Security & Privacy

- ✅ All data stored **locally** in browser localStorage
- ✅ No server-side storage or databases
- ✅ No third-party tracking or analytics
- ✅ Works completely offline after first load
- ⚠️ Data is tied to browser - clearing browser data will erase all entries
- ⚠️ Use Export feature regularly to backup your data

## Troubleshooting

### Data Not Saving
- Check browser privacy settings
- Ensure cookies/localStorage are enabled
- Try a different browser

### Can't Access on Mobile
- Ensure devices are on same network (for local server)
- Check firewall settings
- For public deployment, wait 2-5 minutes for DNS propagation

### Import Failed
- Ensure JSON file is valid (from Export feature)
- Check file wasn't corrupted during transfer
- Try re-exporting and importing again

### Site Not Loading
- Clear browser cache
- Check deployment platform status
- Verify URL is correct
- Try incognito/private mode

## Platform URLs

After deployment, you'll get a URL like:

- **Netlify**: `https://your-app-name.netlify.app`
- **Vercel**: `https://your-app-name.vercel.app`
- **GitHub Pages**: `https://username.github.io/repo-name`

Bookmark this URL on all your devices for easy access!

## Support

For deployment issues:
- Netlify: https://docs.netlify.com
- Vercel: https://vercel.com/docs
- GitHub Pages: https://docs.github.com/pages

---

**Quick Tip**: For the fastest deployment, use Netlify's drag-and-drop feature. You'll have a live URL in under 30 seconds!
