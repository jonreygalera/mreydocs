<p align="center"><a href="https://jonreygalera.vercel.app" target="_blank"><img src="../assets/logo.png" alt="Mreycode Logo"></a></p>
# mreycode-signal

A single-page, elegant, bento-style metrics dashboard supporting iframe integration per widget. Built with a **Vibe Coding** approach using the **Google Antigravity IDE**.

[![Next.js 16](https://img.shields.io/badge/Next.js-16-black?style=flat-square&logo=next.js)](https://nextjs.org/)
[![Tailwind CSS 4](https://img.shields.io/badge/Tailwind-4-38B2AC?style=flat-square&logo=tailwind-css)](https://tailwindcss.com/)
[![React 19](https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react)](https://reactjs.org/)

## Features

- **Bento Grid Layout**: Responsive and visually appealing data display.
- **Iframe Embedding**: Every widget can be embedded into other apps (like chatbots) with zero-padding and theme support.
- **Dynamic Themes**: Built-in support for Dark/Light modes, including specialized iframe theme overrides.
- **Live Data**: Integration with SWR for real-time data fetching and auto-refresh.
- **Vibe Coding Certified**: Developed with cutting-edge AI-assisted workflows.

## Tech Stack

- **Core**: Next.js 16 (App Router), React 19, TypeScript
- **Styling**: Tailwind CSS 4
- **Charts**: Recharts 3
- **Animations**: Framer Motion 12
- **Data Fetching**: SWR 2.4

---

## Configuration

### Dashboard Widgets

The dashboard is purely data-driven. You can configure widgets in `src/config/dashboard.ts`.

#### Widget Schema

```typescript
interface WidgetConfig {
  id: string; // Unique identifier (used for iframe routing)
  label: string; // Title shown on the dashboard
  type: WidgetType; // 'stat' | 'line' | 'bar' | 'area'
  api: string; // Endpoint to fetch data
  method?: "GET" | "POST"; // HTTP method
  headers?: Record<string, string>;
  body?: any; // Request body
  responsePath: string; // Dot-notation path to the value in response (e.g. 'data.value')
  xKey?: string; // X-axis property (for charts)
  yKey?: string; // Y-axis property (for charts)
  size?: "sm" | "md" | "lg" | "xl";
  refreshInterval?: number; // Auto-refresh in ms
  description?: string;
  sourceUrl?: string; // Clickable external source link
  prefix?: string; // Prefix for stat values (e.g. '$')
  suffix?: string; // Suffix for stat values (e.g. '%')
}
```

### Widget Types

#### 1. Statistical (`stat`)

Used for displaying a single numerical or string value.

- **Important Keys**: `responsePath`.
- **Sample Config**:

```typescript
{
  id: 'user-count',
  type: 'stat',
  label: 'Total Users',
  api: '/api/stats',
  responsePath: 'data.metrics.totalUsers', // Path to the single number
  prefix: '👥 '
}
```

#### 2. Charts (`line`, `bar`, `area`)

Used for visualizing time-series or comparative data.

- **Important Keys**: `responsePath` (should point to an array), `xKey` (label), `yKey` (value).
- **Sample Config (Line Chart)**:

```typescript
{
  id: 'revenue-chart',
  type: 'line',
  label: 'Revenue Trend',
  api: '/api/revenue',
  responsePath: 'result.series', // Points to an array of objects
  xKey: 'day',                   // The key for the horizontal axis
  yKey: 'revenue',               // The key for the vertical axis
  size: 'lg'
}
```

_Note: `area` and `bar` use the same configuration structure as `line` but render different visualization styles._

### Adding a New Widget

1. Open `src/config/dashboard.ts`.
2. Add a new object to the `dashboardWidgets` array following the schema above.
3. The dashboard will automatically pick up the change and render the new bento item.

---

## Iframe Integration

One of the core features of **mreycode-signal** is the ability to embed individual widgets anywhere.

### Dedicated Iframe Route

Every widget is reachable via:
`[YOUR_DOMAIN]/widget/[WIDGET_ID]/iframe`

This route is specialized for embedding:

- No navigation headers or footers.
- No container margins or padding (edge-to-edge rendering).
- No entry animations for instant visibility.

### Theme Overrides

You can force a specific theme for an embedded widget using the `theme` query parameter:

- **Light Mode**: `.../iframe?theme=light`
- **Dark Mode**: `.../iframe?theme=dark` (Default)

### Embed Snippet Example

```html
<iframe
  src="https://your-domain.com/widget/gold-futures-chart/iframe?theme=dark"
  width="100%"
  height="400"
  frameborder="0"
></iframe>
```

### Integrator Tool

Each widget has a detail page (`/widget/[id]`) that includes a **Live Preview** and **Embed Configurator**. Use this tool to toggle themes and copy the exact HTML snippet for your integration.

---

## Development

First, run the development server:

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

---

## Credits

- **Developer**: [jonreygalera](https://jonreygalera.vercel.app)
- **Repo**: [github.com/jonreygalera/mreycode-signal](https://github.com/jonreygalera/mreycode-signal)

Project created with **Google Antigravity IDE** using the Vibe Coding approach.
