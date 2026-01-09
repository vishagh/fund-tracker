# Financial Fortress v 0.1

A lightweight, privacy-first financial management application for tracking investment allocation and achieving financial milestones. All data is stored locally on your device using the Origin Private File System (OPFS) API—no server, no cloud, no data sharing.

## Features

### 💰 Monthly Surplus Splitter
- Input monthly surplus amount
- Define custom fund allocation percentages (add/edit/remove funds)
- Automatically calculate distribution across configured funds
- Log investments with automatic date tracking
- View complete investment history with breakdown summaries

### 📊 History & Progress Dashboard
- Track all logged investments chronologically
- View cumulative total saved across all investments
- Monitor allocation distribution over time
- Export complete investment history as JSON backup
- Clear history with confirmation (destructive action protection)

### ✅ To-Do List & Financial Milestones
- Create milestone reminders with specific target dates
- Example milestones: FD maturity dates, relocation targets, savings goals
- Sort tasks automatically by date
- Mark milestones as completed
- Browser notifications for milestone dates (with permission-based alerts)
- Manage task list with add/remove functionality

### 🔐 Local Storage with OPFS
- **No cloud sync required** – all data stays on your device
- **Instant notifications** – milestone reminders trigger on target dates
- **Export backups** – download complete financial data as JSON
- **Graceful fallback** – browser cache as secondary storage option

---

## Technical Stack

### Frontend Framework
- **Alpine.js v3.x** – Lightweight reactive framework for UI interactions
- **Tailwind CSS** – Utility-first CSS framework for responsive design
- **HTML5** – Semantic markup with Web APIs integration

### Key Technologies
| Technology | Purpose |
|-----------|---------|
| **Origin Private File System (OPFS)** | Local persistent storage |
| **File System Access API** | Reading/writing JSON data files |
| **Notification API** | Browser alerts for milestones |
| **Web Storage API** | Fallback to localStorage if OPFS unavailable |
| **Fetch & Blob APIs** | Data export functionality |

### Architecture
```
Single-Page Application (SPA)
├── HTML5 + Alpine.js (Reactive State Management)
├── Tailwind CSS (Styling & Responsive Layout)
└── Origin Private File System (Persistent Data Layer)
```

---

## Understanding OPFS (Origin Private File System)

### What is OPFS?

The **Origin Private File System** is a modern browser API that provides:
- **High-capacity storage** (typically tens of gigabytes, depending on device)
- **Direct file access** via the File System Access API
- **Origin-sandboxed** privacy (isolated per domain)
- **Synchronous operations** (via Worker Threads)
- **No user permission prompts** (unlike the older file picker)

### OPFS Architecture in This Project

```javascript
// Step 1: Request access to root directory
const root = await navigator.storage.getDirectory();

// Step 2: Get or create JSON file handle
const fileHandle = await root.getFileHandle('fortress_master_v7.json', {
  create: true
});

// Step 3: Read existing data (persistent across sessions)
const file = await fileHandle.getFile();
const text = await file.text();
const data = JSON.parse(text);

// Step 4: Write updated data when changes occur
const writable = await fileHandle.createWritable();
await writable.write(JSON.stringify(data));
await writable.close();
```

### Advantages of OPFS

#### 1. **Privacy & Security**
- ✅ Data never leaves your device
- ✅ No account/authentication required
- ✅ No server logs of financial information
- ✅ Encrypted at rest on your hard drive
- ❌ No data sharing with third parties

#### 2. **Performance**
- ✅ Instant read/write operations (no network latency)
- ✅ No sync delays or conflicts
- ✅ Optimized for large datasets (unlike localStorage limit of 5-10MB)
- ✅ Asynchronous I/O (doesn't block main thread)

#### 3. **Reliability**
- ✅ Persistent across browser sessions
- ✅ Survives browser restart and OS reboot
- ✅ Survives browser cache clear (unlike sessionStorage)
- ✅ Graceful fallback to browser cache if unavailable

#### 4. **User Control**
- ✅ Data export to JSON anytime
- ✅ No forced cloud backup
- ✅ Manual backup creation
- ✅ Transparent storage location (user filesystem)

#### 5. **Standards Compliant**
- ✅ W3C standard (file-system-access Level 1)
- ✅ Supported in Chrome, Edge, Opera (66%+ browser coverage)
- ✅ Graceful degradation for unsupported browsers
- ✅ Future-proof API design

### OPFS vs Alternatives

| Feature | OPFS | localStorage | IndexedDB | Server DB |
|---------|------|-------------|----------|-----------|
| **Storage Capacity** | 50+ GB | 5-10 MB | 50-100 MB | Unlimited |
| **Privacy** | 🟢 Device Only | 🟢 Device Only | 🟢 Device Only | 🔴 Cloud |
| **Speed** | 🟢 Instant | 🟢 Instant | 🟡 Slower | 🔴 Network |
| **Persistence** | 🟢 Permanent | 🟡 Cache Clear | 🟢 Permanent | 🟢 Permanent |
| **Backup Easy** | 🟢 Direct Export | 🔴 Manual JSON | 🔴 Complex | 🟢 Automatic |
| **Sync Devices** | 🔴 Local Only | 🔴 Local Only | 🔴 Local Only | 🟢 Cloud |

---

## Usage

### Getting Started
1. Open `index.html` in a modern browser (Chrome, Edge, Opera 66+)
2. Grant OPFS permission when prompted (one-time)
3. Input monthly surplus and configure fund allocations
4. Click "Log Investment" to record the transaction
5. View history and progress in the History tab

### Managing Funds
- **Add Fund**: Click "Manage Fund List" → Enter fund name → Click "Add Fund"
- **Update Allocation**: Modify percentage ratio for any fund
- **Remove Fund**: Click "×" button next to fund name (requires confirmation)

### Setting Milestones
- Switch to "To-Do List" tab
- Enter milestone title and target date
- Browser will notify you on the target date (if notifications enabled)

### Exporting Data
- Click "EXPORT JSON" in the top bar
- Receives timestamped backup file (e.g., `fortress_backup_2026-01-09.json`)
- Importable to spreadsheets or other tools

### Security Notifications
- **Green Indicator**: "STORAGE: SECURE (OPFS)" = Local storage active
- **Orange Indicator**: "STORAGE: BROWSER CACHE" = OPFS unavailable, fallback mode

---

## Browser Support

### Fully Supported
- ✅ Chrome 90+
- ✅ Edge 90+
- ✅ Opera 76+
- ✅ Brave 1.24+

### Partial Support (Fallback Mode)
- ⚠️ Safari 15+ (OPFS in development)
- ⚠️ Firefox (in development)

### Not Supported
- ❌ Internet Explorer
- ❌ Older mobile browsers

---

## Development Notes

### File Structure
```
fund-tracker/
├── index.html          # Single-file SPA with HTML, CSS, and JavaScript
├── README.md           # This documentation
└── fortress_master_v7.json  # Auto-generated OPFS data file
```

### Data Schema
```json
{
  "surplus": 50000,
  "allocations": [
    {"fundName": "ICICI Savings", "ratio": 50},
    {"fundName": "Axis Short Duration", "ratio": 30},
    {"fundName": "ICICI BAF", "ratio": 20}
  ],
  "history": [
    {
      "date": "01/09/2026",
      "total": 50000,
      "summary": "ICICI Savings (50%) | Axis Short Duration (30%) | ICICI BAF (20%)"
    }
  ],
  "todos": [
    {"title": "SBI FD Maturity (2.02L)", "date": "2026-01-14", "completed": false}
  ]
}
```

### Key Functions
- `initOPFS()` – Initialize storage and load data
- `saveData()` – Persist data to OPFS
- `logInvestment()` – Record new investment entry
- `exportData()` – Generate JSON backup
- `checkReminders()` – Trigger milestone notifications
- `calculateTotalSaved()` – Compute cumulative savings

---

## Privacy & Licensing

- **No telemetry** – Nothing is tracked or sent anywhere
- **No analytics** – Your investment data is never analyzed
- **No ads** – This is purely a utility for personal use
- **Local-first** – Default behavior is device-only storage
- **Open to enhance** – Feel free to fork, modify, or redistribute

---

## Troubleshooting

### "STORAGE: BROWSER CACHE" instead of OPFS
- Your browser doesn't support OPFS yet
- Data still persists locally in browser cache
- Upgrade to Chrome 90+, Edge 90+, or Opera 76+

### Data disappeared after browser update
- OPFS may be cleared in some browser updates
- **Always keep JSON exports** as backups
- Re-import data from export file if needed

### Notifications not showing
- Click "ENABLE ALERTS" button to grant permission
- Some browsers require desktop notifications to be enabled at OS level
- Date must match exactly (YYYY-MM-DD format)

### Fund allocation doesn't add to 100%
- Feature allows flexibility (e.g., savings in safety net)
- Exact percentages are recorded in history for transparency

---

**Built with ❤️ for financial independence**
