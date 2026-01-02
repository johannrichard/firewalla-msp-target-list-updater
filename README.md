# Firewalla ASN Target List Updater

Updates Target Lists in Firewalla MSP using an ASN / the ASN from the title

## Overview

This tool automatically updates Firewalla target lists with IP prefixes from Autonomous System Numbers (ASNs). It fetches BGP routing data from bgp.tools and updates target lists whose names contain ASN numbers.

## Requirements

- Node.js 12 or higher
- Firewalla MSP account with API token

## Installation

```bash
npm install
```

## Building

Build the standalone bundled executable:

```bash
npm run build
```

This creates a single bundled file at `dist/asn-updater.js` that can be run with Node.js.

## Usage

### Environment Variables

You can set environment variables in two ways:

1. **Using a .env file** (recommended):
   ```bash
   # Copy the example file and edit it
   cp .env.example .env
   # Edit .env with your actual credentials
   ```

2. **Using shell environment variables**:
   ```bash
   export MSP_DOMAIN=mydomain.firewalla.net
   export FIREWALLA_TOKEN=your-token
   ```

Required environment variables:
- `MSP_DOMAIN`: Your Firewalla MSP domain (e.g., `mydomain.firewalla.net`)
- `FIREWALLA_TOKEN`: Your Firewalla API personal access token
- `BGP_TOOLS_USER_AGENT`: User-Agent string for bgp.tools API
  - Format: `"organization bgp.tools - contact@email.com"`
  - Example: `"acmeco bgp.tools - contact@acme.co"`
  - **Required by bgp.tools** - Default or generic user agents may be blocked

### Running the Tool

```bash
# Run with .env file
node dist/asn-updater.js

# Run with environment variables on command line
MSP_DOMAIN=mydomain.firewalla.net FIREWALLA_TOKEN=your-token \
  BGP_TOOLS_USER_AGENT="acmeco bgp.tools - contact@acme.co" \
  node dist/asn-updater.js

# Preview changes without updating (dry run)
node dist/asn-updater.js --dry-run

# Show help
node dist/asn-updater.js --help
```

### bgp.tools Fair Use Policy

This tool uses the bgp.tools API to fetch BGP routing data. Please note the following requirements:

- **You MUST set a descriptive User-Agent** to identify yourself
- Default or generic user agents are **NOT supported** and may be blocked
- Preferred format: `"organization bgp.tools - contact@email.com"`
- Example: `"acmeco bgp.tools - contact@acme.co"`
- **Do NOT scrape HTML pages** - use API endpoints only
- Users who violate these guidelines may be banned without notice
- See: https://bgp.tools/kb/api

### Caching and Recommended Cron Schedule

The tool implements a simple file-based cache stored in a `.cache` directory alongside the script. This reduces load on bgp.tools and avoids unnecessary downloads.

- `.cache/bgp-table.jsonl` — BGP routing table (large); refreshed at most once per hour.
- `.cache/asns.csv` — ASN names/metadata (small CSV); refreshed at most once every 24 hours.

You can run the script hourly; the updater will only download the ASN CSV once per 24 hours while it may refresh the BGP table hourly. Example crontab entries:

```bash
# Run hourly (script caches ASN CSV for 24h)
0 * * * * cd /path/to/script && /usr/bin/node dist/asn-updater.js >> asn-updater.log 2>&1

# Run daily at 3 AM
0 3 * * * cd /path/to/script && /usr/bin/node dist/asn-updater.js >> asn-updater.log 2>&1
```

### Command-Line Options

- `--dry-run`, `-d`: Preview changes without updating target lists
- `--help`, `-h`: Show help message

### How It Works

1. Fetches all Firewalla boxes in your MSP account
2. Fetches all target lists (global and per-box)
3. Identifies target lists with ASN numbers in their names (e.g., "AS288", "AS565")
4. Downloads BGP routing table data from bgp.tools
5. Extracts IP prefixes for each ASN
6. Updates target lists with the current IP prefixes

### Target List Naming

Target lists should include ASN numbers in their names:

- `AS288` - Single ASN
- `AS288, AS565` - Multiple ASNs
- `Block AS5677 Traffic` - ASN in description

## Development

### Code Quality

```bash
# Check code formatting and linting
npm run check

# Auto-fix issues
npm run check:fix

# Format only
npm run format

# Lint only
npm run lint
npm run lint:fix
```

### Project Structure

- `src/asn-updater.ts` - Main TypeScript source code
- `dist/asn-updater.js` - Bundled executable (generated)
- `biome.json` - Biome configuration for linting and formatting
- `tsconfig.json` - TypeScript configuration

## License

MIT
