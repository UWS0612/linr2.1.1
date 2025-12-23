# LINR – CQA Logging App

A Construction Quality Assurance (CQA) logging application for geomembrane projects. This app allows field teams to track daily reports, materials, welds, and various quality tests with offline-capable localStorage persistence.

## Features

- **8 Data Logging Modules:**
  - Daily Summary Reports
  - Material Tracker (geomembrane rolls)
  - Weld Log
  - Air Channel Tests
  - Vacuum Box Tests
  - Spark/Holiday Tests
  - Destructive Tests
  - NCR (Non-Conformance Report) Register

- **Settings Management:**
  - Company information
  - Contact details
  - Project region
  - Logo upload (stored as base64)

- **Data Export:**
  - Export all logs as CSV files
  - Generate final summary report (Markdown)
  - Package everything into a ZIP file

- **Offline-First:**
  - All data stored in browser localStorage
  - No backend required
  - Works offline after initial load

## Tech Stack

- **Next.js 14** (App Router)
- **TypeScript**
- **Tailwind CSS**
- **shadcn/ui** components
- **JSZip** for export functionality
- **Lucide React** for icons

## Getting Started

### Prerequisites

- Node.js 20 or higher
- npm or pnpm

### Installation

1. Clone the repository or navigate to the project directory:

```bash
cd linr-cqa
```

2. Install dependencies:

```bash
npm install
```

### Development

Run the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser.

### Build

Create a production build:

```bash
npm run build
```

### Production

Run the production server:

```bash
npm run build
npm start
```

## Deployment

### Vercel (Recommended)

1. Install Vercel CLI:

```bash
npm i -g vercel
```

2. Deploy:

```bash
vercel
```

Or connect your repository to Vercel through the web dashboard.

### Netlify

1. Install Netlify CLI:

```bash
npm i -g netlify-cli
```

2. Build and deploy:

```bash
npm run build
netlify deploy --prod
```

### Other Platforms

This app can be deployed to any platform that supports Next.js:

- AWS Amplify
- Azure Static Web Apps
- Cloudflare Pages
- Railway
- Render

## Usage

1. **Configure Settings:** Start by entering company information and uploading a logo in the Settings tab
2. **Log Data:** Switch between tabs to enter data for different aspects of the project
3. **Export Data:** Click "Export ZIP" to download all logs as CSV files with a summary report
4. **Clear Data:** Use "Clear All Data" to reset the application (warning: this cannot be undone)

## Data Storage

All data is stored locally in your browser's localStorage. No data is sent to any server. To clear all data, either:

- Use the "Clear All Data" button in the app
- Clear your browser's localStorage manually
- Clear your browser's cache and data

## License

MIT License - see [LICENSE](LICENSE) file for details

## Support

For issues or questions, please contact LINR.
