# Jace Pay Dashboard

**Service Advisor Pay & Performance Dashboard** - A cross-platform calculator for tracking bi-weekly commissions and monthly bonuses.

## Features

- **Bi-Weekly Commission Tracking**
  - Track repair orders (ROs) with detailed breakdowns
  - Customer Pay (CP): 7% commission
  - Warranty: 5% commission
  - Internal: 5% commission
  - Sublet: 0.5% commission
  - Automatic discount calculation
  - Hours sold tracking for ROI analysis

- **Monthly Bonus Calculations**
  - Tier bonuses (up to 2% based on CP net sales)
  - Objective bonuses (up to 1% weighted by goals)
  - Review bonuses ($375 for 15+ reviews)
  - Real-time calculation updates

- **Data Management**
  - Export data as JSON for backup
  - Import data to sync across devices
  - Local storage (privacy-first, no server)
  - Automatic save on every change

- **Cross-Platform**
  - Works on desktop, mobile, and tablet
  - Responsive design
  - Can be deployed to any web hosting platform
  - Offline-capable after first load

## Quick Start

### Local Use

1. Open `index.html` in any modern web browser
2. Start entering RO data
3. Export your data regularly for backup

### Deploy for Cross-Platform Access

See **[DEPLOYMENT.md](DEPLOYMENT.md)** for detailed instructions on deploying to:
- Netlify (recommended - free, instant)
- Vercel (free, developer-friendly)
- GitHub Pages (free, simple)
- Local network access
- Cloud storage (AWS S3, Google Cloud)

**Fastest option**: Drag `index.html` to [Netlify Drop](https://app.netlify.com/drop) - live in 30 seconds!

## How to Use

### Bi-Weekly Commissions Tab

1. Fill in RO details (RO #, customer, labor, parts, etc.)
2. Click "Add RO" to save
3. View your commission summary in real-time
4. Track ROI with hours sold data
5. Export data before end of pay period

### Monthly Bonuses Tab

1. Enter month and CP net sales
2. Set review count
3. Enter objective bonus inputs (weighted or manual mode)
4. View tier bonus, objective bonus, and review bonus calculations
5. Save monthly inputs

### Data Export/Import

**Export** (backup or transfer):
- Click "Export Data" button
- Save the JSON file to a safe location
- Use this to backup or transfer to another device

**Import** (restore or sync):
- Click "Import Data" button
- Select a previously exported JSON file
- Confirm to replace current data

## Technology

- **Framework**: Vanilla JavaScript (no dependencies)
- **Storage**: Browser localStorage (client-side only)
- **Styling**: Custom CSS with modern design system
- **Architecture**: Clean separation of concerns (Models, Calculator, Storage, State, UI, Events)

## Browser Support

- Chrome/Edge (Desktop & Mobile)
- Firefox (Desktop & Mobile)
- Safari (Desktop & Mobile)
- Opera

## Security & Privacy

- ✅ All data stored locally in your browser
- ✅ No server-side storage or databases
- ✅ No tracking or analytics
- ✅ Works completely offline
- ⚠️ Clear browser data = lost data (use Export regularly!)

## File Structure

```
jace-pay-dashboard/
├── index.html          # Complete application (single file)
├── README.md           # This file
├── DEPLOYMENT.md       # Deployment guide
├── netlify.toml        # Netlify configuration
├── vercel.json         # Vercel configuration
└── .nojekyll          # GitHub Pages configuration
```

## Deployment Platforms

| Platform | Cost | Setup Time | Custom Domain | Best For |
|----------|------|------------|---------------|----------|
| Netlify | Free | 30 seconds | Yes | Drag & drop simplicity |
| Vercel | Free | 1 minute | Yes | Git integration |
| GitHub Pages | Free | 2 minutes | Yes | Open source projects |
| Local Server | Free | Instant | N/A | Testing/home network |

## Commission Rates

- Customer Pay (CP): **7%** on labor + parts - discounts
- Warranty: **5%** on labor + parts
- Internal: **5%** on labor + parts
- Sublet: **0.5%** on labor only

## Tier Bonus Structure

- $35K+ CP Net: +0.5%
- $45K+ CP Net: +0.5% (total 1.0%)
- $55K+ CP Net: +0.5% (total 1.5%)
- $65K+ CP Net: +0.5% (total 2.0%)

Requires 7+ reviews per month (or manual override).

## Objective Bonus Weights

- Alignments: 0.2%
- Total Labor Gross: 0.3%
- CP Labor Gross: 0.2%
- HAG Products: 0.1%
- CP Gross per RO: 0.1%
- Hours per CP RO: 0.1%

**Maximum**: 1.0% total

## License

This is a personal dashboard tool. Use and modify as needed.

## Support

- For deployment help, see [DEPLOYMENT.md](DEPLOYMENT.md)
- For platform-specific issues, consult the platform's documentation
- For feature requests or bugs, modify the code directly (it's all in `index.html`)

---

**Quick Deploy**: `npx serve .` then open http://localhost:3000
