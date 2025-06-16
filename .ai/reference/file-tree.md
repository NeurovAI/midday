# 🌳 Midday Application File Tree Structure

This document provides a comprehensive overview of the Midday application's file structure. Midday is a modern financial management platform built with a monorepo architecture using TypeScript, Next.js, Tauri, and various other technologies.

## 📁 Project Root

```
📁 midday/
├── 📄 LICENSE                    # Project license
├── 📄 README.md                  # Main project documentation
├── 📄 SECURITY.md               # Security policy and guidelines
├── 📄 biome.json                # Biome linter/formatter configuration
├── 🔒 bun.lock                  # Bun package manager lock file
├── 🖼️ github.png                # GitHub repository image
├── 📦 package.json              # Root package.json for monorepo
├── ⚙️ tsconfig.json             # Root TypeScript configuration
├── ⚙️ turbo.json                # Turborepo configuration
├── 📁 .ai/                      # AI assistant configuration and reference
│   ├── 📁 llms/                 # LLM configurations
│   ├── 📁 reference/            # Reference documentation
│   ├── 📁 rules/                # AI assistant rules
│   └── 📁 tasks/                # Task management
│       ├── 📁 backlog/          # Backlog tasks
│       ├── 📁 doing/            # In-progress tasks
│       └── 📁 done/             # Completed tasks
├── 📁 apps/                     # Main applications
├── 📁 packages/                 # Shared packages and libraries
└── 📁 types/                    # Global TypeScript type definitions
    ├── 📄 images.d.ts           # Image type declarations
    └── 📄 jsx.d.ts              # JSX type declarations
```

## 🚀 Applications (`apps/`)

### 🌐 API (`apps/api/`)
*Backend API service built with tRPC and Drizzle ORM*

```
📁 apps/api/
├── 🐳 Dockerfile               # Docker container configuration
├── 📄 README.md                # API documentation
├── ⚙️ drizzle.config.ts        # Database ORM configuration
├── ✈️ fly-preview.yml          # Fly.io preview deployment config
├── ✈️ fly.toml                 # Fly.io production deployment config
├── 📦 package.json             # API dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
├── 📁 migrations/              # Database migration files
└── 📁 src/                     # Source code
    ├── 📄 index.ts             # Main entry point
    ├── 📁 db/                  # Database layer
    │   ├── 📄 index.ts         # Database connection
    │   ├── 📄 replicas.ts      # Database replica configuration
    │   ├── 📄 schema.ts        # Database schema definitions
    │   └── 📁 queries/         # Database query functions
    ├── 📁 rest/                # REST API endpoints
    │   ├── 📄 types.ts         # REST API types
    │   ├── 📁 middleware/      # REST middleware
    │   └── 📁 routers/         # REST route handlers
    ├── 📁 schemas/             # Validation schemas
    │   ├── 📄 api-keys.ts      # API key schemas
    │   ├── 📄 apps.ts          # Application schemas
    │   ├── 📄 bank-accounts.ts # Bank account schemas
    │   ├── 📄 bank-connections.ts # Bank connection schemas
    │   ├── 📄 customers.ts     # Customer schemas
    │   ├── 📄 documents.ts     # Document schemas
    │   ├── 📄 inbox.ts         # Inbox schemas
    │   ├── 📄 institutions.ts  # Institution schemas
    │   ├── 📄 invoice.ts       # Invoice schemas
    │   ├── 📄 metrics.ts       # Metrics schemas
    │   ├── 📄 search.ts        # Search schemas
    │   ├── 📄 tags.ts          # Tag schemas
    │   ├── 📄 team.ts          # Team schemas
    │   ├── 📄 tracker-entries.ts # Time tracker entry schemas
    │   ├── 📄 tracker-projects.ts # Time tracker project schemas
    │   ├── 📄 transactions.ts  # Transaction schemas
    │   └── 📄 users.ts         # User schemas
    ├── 📁 services/            # External service integrations
    │   ├── 📄 resend.ts        # Email service
    │   └── 📄 supabase.ts      # Supabase integration
    ├── 📁 trpc/                # tRPC configuration
    │   ├── 📄 init.ts          # tRPC initialization
    │   ├── 📁 middleware/      # tRPC middleware
    │   └── 📁 routers/         # tRPC route handlers
    └── 📁 utils/               # Utility functions
        ├── 📄 api-keys.ts      # API key utilities
        ├── 📄 auth.ts          # Authentication utilities
        ├── 📄 geo.ts           # Geolocation utilities
        ├── 📄 health.ts        # Health check utilities
        ├── 📄 logger.ts        # Logging utilities
        ├── 📄 parse.ts         # Parsing utilities
        ├── 📄 scopes.ts        # Permission scope utilities
        ├── 📄 search.ts        # Search utilities
        ├── 📁 cache/           # Caching utilities
        └── 📄 validate-response.ts # Response validation
```

### 📊 Dashboard (`apps/dashboard/`)
*Main web application built with Next.js*

```
📁 apps/dashboard/
├── 📄 README.md                # Dashboard documentation
├── 📄 image-loader.ts          # Custom image loader configuration
├── ⚙️ next.config.mjs          # Next.js configuration
├── 📦 package.json             # Dashboard dependencies
├── ⚙️ postcss.config.cjs       # PostCSS configuration
├── ⚙️ tailwind.config.ts       # Tailwind CSS configuration
├── ⚙️ tsconfig.json            # TypeScript configuration
├── ⚙️ vercel.json              # Vercel deployment configuration
├── 📁 public/                  # Static assets
└── 📁 src/                     # Source code
    ├── 📄 middleware.ts        # Next.js middleware
    ├── 📁 actions/             # Server actions
    │   ├── 📁 ai/              # AI-related actions
    │   ├── 📁 institutions/    # Institution actions
    │   ├── 📁 transactions/    # Transaction actions
    │   └── 📄 [various-actions].ts # Individual action files
    ├── 📁 app/                 # Next.js App Router
    │   ├── 📄 favicon.ico      # Application favicon
    │   ├── 📄 global-error.tsx # Global error boundary
    │   ├── 📁 [locale]/        # Internationalized routes
    │   └── 📁 api/             # API routes
    ├── 📁 components/          # React components
    │   ├── 📁 assistant/       # AI assistant components
    │   ├── 📁 base-currency/   # Currency components
    │   ├── 📁 charts/          # Chart components
    │   ├── 📁 chat/            # Chat interface components
    │   ├── 📁 forms/           # Form components
    │   ├── 📁 inbox/           # Inbox components
    │   ├── 📁 invoice/         # Invoice components
    │   ├── 📁 modals/          # Modal components
    │   ├── 📁 search/          # Search components
    │   ├── 📁 sheets/          # Sheet/drawer components
    │   ├── 📁 tables/          # Table components
    │   ├── 📁 tracker/         # Time tracker components
    │   ├── 📁 vault/           # Document vault components
    │   ├── 📁 widgets/         # Dashboard widgets
    │   └── 📄 [various-components].tsx # Individual component files
    ├── 📁 hooks/               # Custom React hooks
    │   └── 📄 [various-hooks].ts # Hook implementations
    ├── 📁 lib/                 # Library utilities
    │   ├── 📄 download.ts      # Download utilities
    │   └── 📁 tools/           # Tool utilities
    ├── 📁 locales/             # Internationalization
    │   ├── 📄 client.ts        # Client-side i18n
    │   ├── 📄 server.ts        # Server-side i18n
    │   ├── 📄 en.ts            # English translations
    │   └── 📄 sv.ts            # Swedish translations
    ├── 📁 store/               # State management (Zustand)
    │   ├── 📄 assistant.ts     # AI assistant state
    │   ├── 📄 export.ts        # Export state
    │   ├── 📄 invoice.ts       # Invoice state
    │   ├── 📄 search.ts        # Search state
    │   ├── 📄 token-modal.ts   # Token modal state
    │   ├── 📄 transactions.ts  # Transaction state
    │   └── 📄 vault.ts         # Vault state
    ├── 📁 styles/              # Global styles
    │   └── 📄 globals.css      # Global CSS
    ├── 📁 trpc/                # tRPC client configuration
    │   ├── 📄 client.tsx       # Client-side tRPC
    │   ├── 📄 query-client.ts  # React Query configuration
    │   └── 📄 server.tsx       # Server-side tRPC
    ├── 📁 types/               # TypeScript type definitions
    │   └── 📄 react-table.d.ts # React Table types
    └── 📁 utils/               # Utility functions
        ├── 📄 canvas-factory.ts # Canvas utilities
        ├── 📄 categories.ts    # Category utilities
        ├── 📄 columns.ts       # Table column utilities
        ├── 📄 connection-status.ts # Connection status utilities
        ├── 📄 constants.ts     # Application constants
        ├── 📄 desktop.ts       # Desktop app utilities
        ├── 📄 dub.ts           # Dub.co integration
        ├── 📄 environment.ts   # Environment utilities
        ├── 📄 format.ts        # Formatting utilities
        ├── 📄 logger.ts        # Logging utilities
        ├── 📄 logos.ts         # Logo utilities
        ├── 📄 pdf-to-img.ts    # PDF to image conversion
        ├── 📄 plans.ts         # Subscription plan utilities
        ├── 📄 polar.ts         # Polar integration
        ├── 📄 process.ts       # Process utilities
        ├── 📄 resend.ts        # Email utilities
        ├── 📄 teller.ts        # Teller integration
        ├── 📄 tracker.ts       # Time tracker utilities
        └── 📄 upload.ts        # File upload utilities
```

### 🖥️ Desktop (`apps/desktop/`)
*Desktop application built with Tauri and React*

```
📁 apps/desktop/
├── 📄 README.md                # Desktop app documentation
├── 📄 index.html               # HTML entry point
├── 📦 package.json             # Desktop app dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
├── ⚙️ tsconfig.node.json       # Node.js TypeScript configuration
├── ⚙️ vite.config.ts           # Vite build configuration
├── 📁 src/                     # Frontend source code
│   ├── 📄 main.tsx             # React entry point
│   └── 📄 vite-env.d.ts        # Vite environment types
└── 📁 src-tauri/               # Tauri backend
    ├── 🦀 Cargo.lock           # Rust dependencies lock file
    ├── 🦀 Cargo.toml           # Rust project configuration
    ├── 🦀 build.rs             # Rust build script
    ├── ⚙️ tauri.conf.json      # Tauri configuration
    ├── ⚙️ tauri.dev.conf.json  # Development configuration
    ├── ⚙️ tauri.staging.conf.json # Staging configuration
    ├── 📁 capabilities/        # Tauri capabilities
    │   ├── ⚙️ default.json     # Default capabilities
    │   └── ⚙️ desktop.json     # Desktop-specific capabilities
    ├── 📁 icons/               # Application icons
    │   ├── 📁 dev/             # Development icons
    │   ├── 📁 production/      # Production icons
    │   ├── 📁 staging/         # Staging icons
    │   └── 🖼️ tray-icon.png    # System tray icon
    ├── 📁 images/              # Application images
    │   └── 🖼️ installer.png    # Installer image
    └── 📁 src/                 # Rust source code
        ├── 🦀 lib.rs           # Library entry point
        └── 🦀 main.rs          # Main application entry point
```

### 📚 Documentation (`apps/docs/`)
*Documentation site built with Mintlify*

```
📁 apps/docs/
├── 📄 README.md                # Documentation README
├── 📄 examples.mdx             # Usage examples
├── 📄 integrations.mdx         # Integration guides
├── 📄 introduction.mdx         # Introduction documentation
├── 📄 local-development.mdx    # Local development guide
├── 📄 self-hosting.mdx         # Self-hosting guide
├── ⚙️ mint.json               # Mintlify configuration
├── 📦 package.json             # Documentation dependencies
├── 📁 api-reference/           # API reference documentation
├── 📁 images/                  # Documentation images
└── 📁 logos/                   # Logo assets
```

### ⚡ Engine (`apps/engine/`)
*Financial data processing engine built with Cloudflare Workers*

```
📁 apps/engine/
├── 📄 README.md                # Engine documentation
├── 📦 package.json             # Engine dependencies
├── ⚙️ tsconfig.build.json      # Build TypeScript configuration
├── ⚙️ tsconfig.json            # TypeScript configuration
├── ⚙️ wrangler.toml            # Cloudflare Workers configuration
├── 📁 tasks/                   # Background tasks
└── 📁 src/                     # Source code
    ├── 📄 index.ts             # Main entry point
    ├── 📄 middleware.ts        # Middleware functions
    ├── 📁 common/              # Common utilities
    │   ├── 📄 bindings.ts      # Cloudflare bindings
    │   └── 📄 schema.ts        # Common schemas
    ├── 📁 providers/           # Financial data providers
    │   ├── 📄 index.ts         # Provider exports
    │   ├── 📄 interface.ts     # Provider interface
    │   ├── 📄 types.ts         # Provider types
    │   ├── 📁 enablebanking/   # EnableBanking provider
    │   ├── 📁 gocardless/      # GoCardless provider
    │   ├── 📁 plaid/           # Plaid provider
    │   └── 📁 teller/          # Teller provider
    ├── 📁 routes/              # API routes
    │   ├── 📁 accounts/        # Account endpoints
    │   ├── 📁 auth/            # Authentication endpoints
    │   ├── 📁 connections/     # Connection endpoints
    │   ├── 📁 enrich/          # Data enrichment endpoints
    │   ├── 📁 health/          # Health check endpoints
    │   ├── 📁 institutions/    # Institution endpoints
    │   ├── 📁 rates/           # Exchange rate endpoints
    │   └── 📁 transactions/    # Transaction endpoints
    └── 📁 utils/               # Utility functions
        ├── 📄 account.test.ts  # Account utility tests
        ├── 📄 account.ts       # Account utilities
        ├── 📄 countries.ts     # Country utilities
        ├── 📄 enrich.ts        # Data enrichment utilities
        ├── 📄 error.ts         # Error handling utilities
        ├── 📄 logger.ts        # Logging utilities
        ├── 📄 logo.ts          # Logo utilities
        ├── 📄 paginate.ts      # Pagination utilities
        ├── 📄 prompt.ts        # AI prompt utilities
        ├── 📄 rates.ts         # Exchange rate utilities
        ├── 📄 retry.ts         # Retry utilities
        └── 📄 search.ts        # Search utilities
```

### 🌐 Website (`apps/website/`)
*Marketing website built with Next.js*

```
📁 apps/website/
├── 📄 README.md                # Website documentation
├── 📄 image-loader.ts          # Custom image loader
├── ⚙️ next.config.mjs          # Next.js configuration
├── 📦 package.json             # Website dependencies
├── ⚙️ postcss.config.cjs       # PostCSS configuration
├── ⚙️ tailwind.config.ts       # Tailwind CSS configuration
├── ⚙️ tsconfig.json            # TypeScript configuration
├── ⚙️ vercel.json              # Vercel deployment configuration
├── 📁 public/                  # Static assets
└── 📁 src/                     # Source code
    └── [similar structure to dashboard with marketing-focused components]
```

## 📦 Packages (`packages/`)

### 🏪 App Store (`packages/app-store/`)
*Application marketplace functionality*

```
📁 packages/app-store/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [app store implementation]
```

### 🖥️ Desktop Client (`packages/desktop-client/`)
*Desktop application client library*

```
📁 packages/desktop-client/
├── 📦 package.json             # Package dependencies
└── 📁 src/                     # Source code
    └── [desktop client implementation]
```

### 📄 Documents (`packages/documents/`)
*Document processing and management*

```
📁 packages/documents/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [document processing implementation]
```

### 📧 Email (`packages/email/`)
*Email templates and sending functionality*

```
📁 packages/email/
├── 📄 README.md                # Email package documentation
├── 📦 package.json             # Package dependencies
├── 📄 render.ts                # Email rendering
├── ⚙️ tsconfig.json            # TypeScript configuration
├── ⚙️ vercel.json              # Vercel configuration
├── 📁 components/              # Email components
├── 📁 emails/                  # Email templates
├── 📁 locales/                 # Email translations
└── 📁 public/                  # Email assets
```

### 🔐 Encryption (`packages/encryption/`)
*Data encryption and security utilities*

```
📁 packages/encryption/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [encryption implementation]
```

### ⚡ Engine Client (`packages/engine-client/`)
*Client library for the financial engine*

```
📁 packages/engine-client/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [engine client implementation]
```

### 📡 Events (`packages/events/`)
*Event handling and messaging system*

```
📁 packages/events/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [event system implementation]
```

### 📥 Import (`packages/import/`)
*Data import functionality*

```
📁 packages/import/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [import functionality implementation]
```

### 📨 Inbox (`packages/inbox/`)
*Inbox and message management*

```
📁 packages/inbox/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [inbox implementation]
```

### 🧾 Invoice (`packages/invoice/`)
*Invoice generation and management*

```
📁 packages/invoice/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [invoice implementation]
```

### ⚙️ Jobs (`packages/jobs/`)
*Background job processing with Trigger.dev*

```
📁 packages/jobs/
├── 📦 package.json             # Package dependencies
├── ⚙️ trigger.config.ts        # Trigger.dev configuration
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [job processing implementation]
```

### 📍 Location (`packages/location/`)
*Location and geolocation services*

```
📁 packages/location/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [location services implementation]
```

### 🔔 Notification (`packages/notification/`)
*Notification system*

```
📁 packages/notification/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [notification implementation]
```

### 🗄️ Supabase (`packages/supabase/`)
*Supabase database client and utilities*

```
📁 packages/supabase/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [Supabase integration implementation]
```

### ⚙️ TypeScript Config (`packages/tsconfig/`)
*Shared TypeScript configurations*

```
📁 packages/tsconfig/
├── 📦 package.json             # Package dependencies
├── ⚙️ base.json                # Base TypeScript config
├── ⚙️ nextjs.json              # Next.js TypeScript config
└── ⚙️ react-library.json       # React library TypeScript config
```

### 🎨 UI (`packages/ui/`)
*Shared UI component library built with Radix UI and Tailwind CSS*

```
📁 packages/ui/
├── 📄 README.md                # UI package documentation
├── 📦 package.json             # Package dependencies
├── ⚙️ postcss.config.js        # PostCSS configuration
├── ⚙️ tailwind.config.ts       # Tailwind CSS configuration
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    ├── 📄 globals.css          # Global CSS styles
    ├── 📁 components/          # UI components
    │   ├── 📄 accordion.tsx    # Accordion component
    │   ├── 📄 alert-dialog.tsx # Alert dialog component
    │   ├── 📄 alert.tsx        # Alert component
    │   ├── 📄 avatar.tsx       # Avatar component
    │   ├── 📄 badge.tsx        # Badge component
    │   ├── 📄 button.tsx       # Button component
    │   ├── 📄 calendar.tsx     # Calendar component
    │   ├── 📄 card.tsx         # Card component
    │   ├── 📄 chart.tsx        # Chart component
    │   ├── 📄 checkbox.tsx     # Checkbox component
    │   ├── 📄 dialog.tsx       # Dialog component
    │   ├── 📄 form.tsx         # Form component
    │   ├── 📄 input.tsx        # Input component
    │   ├── 📄 select.tsx       # Select component
    │   ├── 📄 table.tsx        # Table component
    │   ├── 📄 toast.tsx        # Toast component
    │   ├── 📁 editor/          # Rich text editor components
    │   └── 📄 [many-more-components].tsx # Additional UI components
    ├── 📁 hooks/               # Custom hooks
    │   ├── 📄 index.ts         # Hook exports
    │   ├── 📄 use-enter-submit.ts # Enter submit hook
    │   ├── 📄 use-media-query.ts # Media query hook
    │   └── 📄 use-resize-observer.ts # Resize observer hook
    └── 📁 utils/               # Utility functions
        ├── 📄 index.ts         # Utility exports
        ├── 📄 cn.ts            # Class name utility
        └── 📄 truncate.ts      # Text truncation utility
```

### 🛠️ Utils (`packages/utils/`)
*Shared utility functions and helpers*

```
📁 packages/utils/
├── 📦 package.json             # Package dependencies
├── ⚙️ tsconfig.json            # TypeScript configuration
└── 📁 src/                     # Source code
    └── [utility function implementations]
```

## 🏗️ Architecture Overview

### Technology Stack
- **Frontend**: React, Next.js, TypeScript, Tailwind CSS
- **Backend**: tRPC, Drizzle ORM, Supabase
- **Desktop**: Tauri (Rust + React)
- **Engine**: Cloudflare Workers
- **Database**: PostgreSQL (via Supabase)
- **Deployment**: Vercel (web), Fly.io (API), Cloudflare (engine)
- **Monorepo**: Turborepo with Bun package manager

### Key Features
- 💰 Financial transaction management
- 🏦 Bank account integration (multiple providers)
- 📊 Financial analytics and reporting
- 🧾 Invoice generation and management
- ⏱️ Time tracking
- 📄 Document management and OCR
- 🤖 AI-powered financial insights
- 🖥️ Cross-platform desktop application
- 📱 Responsive web interface
- 🔐 Enterprise-grade security

### Development Workflow
- **Linting/Formatting**: Biome
- **Type Checking**: TypeScript
- **Testing**: Built-in test utilities
- **CI/CD**: GitHub Actions (implied)
- **Package Management**: Bun with workspaces
- **Build System**: Turborepo for efficient builds

### Financial Data Providers
- **Plaid**: US/Canada bank connections
- **GoCardless**: European bank connections
- **Teller**: US bank connections
- **EnableBanking**: European bank connections
- **Teller**: Additional US provider

### Deployment Architecture
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Vercel        │    │    Fly.io       │    │  Cloudflare     │
│   (Dashboard)   │◄──►│    (API)        │◄──►│  (Engine)       │
│   (Website)     │    │                 │    │                 │
└─────────────────┘    └─────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         ▼                       ▼                       ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Supabase      │    │   External      │    │   External      │
│   (Database)    │    │   Services      │    │   APIs          │
│   (Auth)        │    │   (Email, etc.) │    │   (Banks, etc.) │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

This file tree represents a sophisticated, production-ready financial management platform with a well-organized monorepo structure, comprehensive feature set, and modern development practices.
