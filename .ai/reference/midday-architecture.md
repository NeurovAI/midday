# Midday Application Architecture

## 1. High-Level Architecture Overview

### Monorepo Structure

Midday is built as a monorepo using Turborepo, with the following organization:

```
midday/
├── apps/           # Main applications
│   ├── api/        # Backend API (tRPC + REST)
│   ├── dashboard/  # Main web application (Next.js)
│   ├── engine/     # Financial data processing (Cloudflare Workers)
│   ├── desktop/    # Desktop application (Tauri)
│   ├── website/    # Marketing website (Next.js)
│   ├── docs/       # Documentation site (Mintlify)
│   └── mobile/     # Mobile application (Expo)
├── packages/       # Shared packages
│   ├── ui/         # UI components
│   ├── supabase/   # Database client
│   ├── utils/      # Shared utilities
│   ├── documents/  # Document processing
│   ├── events/     # Event system
│   ├── inbox/      # Email integration
│   └── ...         # Other shared functionality
└── types/          # Global TypeScript types
```

### Core Applications

#### Dashboard (`apps/dashboard/`)
- **Technology**: Next.js, React, TypeScript, Tailwind CSS
- **Purpose**: Main web application for users
- **Deployment**: Vercel
- **Key Features**: Financial management UI, reporting, document management

#### API (`apps/api/`)
- **Technology**: Node.js, tRPC, Drizzle ORM
- **Purpose**: Backend business logic and data access
- **Deployment**: Fly.io with Docker
- **Key Features**: Dual REST/tRPC interfaces, database access, business logic

#### Engine (`apps/engine/`)
- **Technology**: Cloudflare Workers, TypeScript
- **Purpose**: Financial data processing at the edge
- **Deployment**: Cloudflare
- **Key Features**: Bank connections, transaction processing, data enrichment

#### Desktop (`apps/desktop/`)
- **Technology**: Tauri (Rust + React)
- **Purpose**: Cross-platform desktop application
- **Deployment**: Self-contained executables
- **Key Features**: Offline capabilities, system integration

#### Website (`apps/website/`)
- **Technology**: Next.js, React, TypeScript
- **Purpose**: Marketing website
- **Deployment**: Vercel
- **Key Features**: Product information, user acquisition

### Shared Packages

| Package | Purpose | Key Technologies |
|---------|---------|------------------|
| `ui` | Shared UI components | React, Tailwind, Shadcn |
| `supabase` | Database client | Supabase SDK |
| `utils` | Common utilities | TypeScript |
| `documents` | Document processing | PDF parsing, OCR |
| `events` | Event system | Pub/sub patterns |
| `inbox` | Email integration | Email parsing |
| `jobs` | Background jobs | Queue management |
| `kv` | Key-value storage | Redis, Upstash |
| `notification` | User notifications | Multi-channel alerts |

### Technology Stack

- **Languages**: TypeScript, JavaScript, Rust (Tauri)
- **Frontend**: React, Next.js, Tailwind CSS, Shadcn UI
- **Backend**: Node.js, tRPC, Drizzle ORM
- **Database**: PostgreSQL (via Supabase)
- **Authentication**: Supabase Auth
- **Storage**: Supabase Storage
- **Edge Computing**: Cloudflare Workers
- **Desktop**: Tauri (Rust + React)
- **Mobile**: React Native (Expo)
- **Package Manager**: Bun
- **Build System**: Turborepo
- **CI/CD**: GitHub Actions

### Deployment Platforms

- **Dashboard & Website**: Vercel
- **API**: Fly.io
- **Engine**: Cloudflare Workers
- **Database & Auth**: Supabase
- **Background Jobs**: Trigger.dev
- **KV Storage**: Upstash Redis

## 2. Detailed Data Flow Analysis

### Request Lifecycle

1. **Client Request Initiation**
   - Browser/Desktop app makes request to Dashboard (Next.js)
   - Dashboard may handle request via:
     - Server Components (React Server Components)
     - Client Components with API calls
     - Server Actions

2. **API Request Processing**
   - Dashboard communicates with API via tRPC
   - API authenticates request via JWT token
   - Request is routed to appropriate tRPC procedure
   - Business logic is executed
   - Database queries are performed

3. **Database Interaction**
   - API connects to appropriate database replica
   - Query is executed via Drizzle ORM
   - Results are processed and transformed
   - Response is returned to client

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│  Dashboard  │────▶│     API     │────▶│  Database   │
│ (Browser/   │     │  (Next.js)  │     │  (tRPC/REST)│     │ (Supabase)  │
│  Desktop)   │◀────│             │◀────│             │◀────│             │
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
                           │                   │
                           ▼                   ▼
                    ┌─────────────┐     ┌─────────────┐
                    │   Engine    │     │  External   │
                    │ (Cloudflare)│     │  Services   │
                    └─────────────┘     └─────────────┘
```

### Authentication Flow

1. **User Login**
   - User submits credentials to Dashboard
   - Dashboard forwards to Supabase Auth
   - Supabase validates and returns JWT

2. **Session Management**
   - JWT stored in browser (secure cookie)
   - Token refreshed automatically
   - Session state managed by Supabase client

3. **API Authentication**
   - JWT included in Authorization header
   - API verifies token with Supabase
   - User/team context extracted from token

```
┌─────────┐     ┌─────────────┐     ┌─────────────┐
│  User   │────▶│  Dashboard  │────▶│  Supabase   │
│         │     │             │     │    Auth     │
│         │◀────│             │◀────│             │
└─────────┘     └─────────────┘     └─────────────┘
                       │                   ▲
                       ▼                   │
                ┌─────────────┐     ┌─────────────┐
                │     API     │────▶│   Token     │
                │             │     │ Verification│
                └─────────────┘     └─────────────┘
```

### Database Routing Strategy

The API implements a sophisticated primary/replica strategy:

1. **Write Operations**
   - Always routed to primary database
   - Ensures consistency and durability

2. **Read Operations**
   - Routed to nearest replica based on geography
   - Falls back to other replicas if nearest is unavailable
   - Uses region detection based on request headers

3. **Region-Based Routing**
   - Frankfurt (fra) - Europe primary
   - Virginia (iad) - US East
   - San Jose (sjc) - US West

```
┌─────────────┐
│     API     │
└─────────────┘
       │
       ├─────────────┐
       │             │
┌──────▼─────┐ ┌─────▼──────┐
│  Primary   │ │   Replica  │
│ (Writes)   │ │   Router   │
└────────────┘ └────────────┘
                      │
       ┌──────────────┼──────────────┐
       │              │              │
┌──────▼─────┐ ┌──────▼─────┐ ┌──────▼─────┐
│  Frankfurt │ │  Virginia  │ │  San Jose  │
│  Replica   │ │  Replica   │ │  Replica   │
└────────────┘ └────────────┘ └────────────┘
```

### Cross-Service Communication

1. **Dashboard → API**
   - Type-safe tRPC calls
   - Server-side data fetching in RSC
   - Client-side queries with React Query

2. **API → Engine**
   - REST API calls for financial processing
   - Webhook callbacks for async operations

3. **Engine → External Providers**
   - Bank API integrations (Plaid, GoCardless, etc.)
   - Webhook handling for provider events

## 3. Component Interaction Diagrams

### Frontend Component Hierarchy

```
┌─────────────────────────────────────────────────────┐
│                   Root Layout                        │
├─────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────────┐ │
│ │  Sidebar    │ │  Main Area  │ │  Right Sidebar  │ │
│ │             │ │             │ │                 │ │
│ └─────────────┘ └─────────────┘ └─────────────────┘ │
└─────────────────────────────────────────────────────┘
                       │
       ┌───────────────┼───────────────┐
       │               │               │
┌──────▼─────┐  ┌──────▼─────┐  ┌──────▼─────┐
│ Dashboard  │  │ Transactions│  │  Settings  │
│  Module    │  │   Module    │  │   Module   │
└────────────┘  └────────────┘  └────────────┘
       │               │               │
┌──────▼─────┐  ┌──────▼─────┐  ┌──────▼─────┐
│  Charts    │  │Transaction │  │ User/Team  │
│ Components │  │   List     │  │  Settings  │
└────────────┘  └────────────┘  └────────────┘
```

### Backend Service Dependencies

```
┌─────────────────────────────────────────────────────┐
│                      API                             │
├─────────────────────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────┐ ┌─────────────────┐ │
│ │  tRPC       │ │  REST       │ │  Database       │ │
│ │  Routers    │ │  Endpoints  │ │  Access Layer   │ │
│ └─────────────┘ └─────────────┘ └─────────────────┘ │
└─────────────────────────────────────────────────────┘
          │               │               │
    ┌─────┘         ┌─────┘         ┌─────┘
    │               │               │
┌────▼──────┐ ┌─────▼─────┐ ┌───────▼───────┐
│ Supabase  │ │  Engine   │ │ External      │
│ Services  │ │ Services  │ │ Integrations  │
└───────────┘ └───────────┘ └───────────────┘
```

### API Gateway Patterns

The API serves as a gateway with clear service boundaries:

1. **Authentication Gateway**
   - Validates all requests
   - Enforces permission checks
   - Manages user context

2. **Data Access Gateway**
   - Centralizes database operations
   - Implements caching strategies
   - Handles data transformations

3. **Integration Gateway**
   - Coordinates with external services
   - Normalizes provider-specific implementations
   - Handles retries and error recovery

## 4. CRUD Operation Flows

### Transaction Processing Pipeline

1. **Creation**
   - Bank transaction imported via Engine
   - Normalized and enriched with metadata
   - Stored in database via API
   - Categorized automatically or manually

2. **Retrieval**
   - Fetched via tRPC query
   - Filtered and paginated
   - Joined with related entities

3. **Update**
   - Modified via tRPC mutation
   - Validated against business rules
   - Audit trail recorded

4. **Deletion**
   - Soft-deleted via status flag
   - Hard deletion restricted to admins
   - Cascading updates to related records

### Document Management Workflow

1. **Upload**
   - Document uploaded to Dashboard
   - Stored in Supabase Storage
   - Metadata recorded in database

2. **Processing**
   - OCR performed via Documents package
   - Text extracted and indexed
   - Relevant data identified (dates, amounts, etc.)

3. **Association**
   - Linked to transactions, customers, etc.
   - Tags and categories applied
   - Searchable via full-text search

4. **Retrieval & Sharing**
   - Accessed via secure URLs
   - Permission-based sharing
   - Version history maintained

### User and Team Management

1. **User Creation**
   - Sign-up via Dashboard
   - Account created in Supabase Auth
   - Profile data stored in database

2. **Team Creation**
   - Team created by user
   - Initial user assigned as admin
   - Team settings configured

3. **User-Team Association**
   - Users invited to teams
   - Roles and permissions assigned
   - Access controlled via team membership

4. **Settings Management**
   - User preferences stored and retrieved
   - Team settings managed by admins
   - Subscription and billing information updated

## 5. Error Handling Architecture

### Global Error Boundaries

1. **Frontend Error Boundaries**
   - React error boundaries at strategic levels
   - Fallback UI components
   - Error reporting to monitoring services

2. **API Error Handling**
   - Structured error responses
   - HTTP status codes mapped to error types
   - Detailed error information in development

3. **Engine Error Management**
   - Worker-specific error handling
   - Dead letter queues for failed operations
   - Automatic retry with backoff

### Error Logging and Monitoring

1. **Centralized Logging**
   - Structured logs with context
   - Error severity classification
   - Correlation IDs across services

2. **Real-time Monitoring**
   - Performance metrics collection
   - Error rate alerting
   - User impact assessment

3. **Debugging Tools**
   - Development mode enhanced errors
   - Request tracing across services
   - Replay capabilities for complex flows

### Retry Mechanisms

1. **Transient Failures**
   - Automatic retry with exponential backoff
   - Circuit breaker pattern for external services
   - Idempotency keys for safe retries

2. **Fallback Strategies**
   - Degraded functionality modes
   - Cached data fallbacks
   - User-initiated retry options

3. **Recovery Procedures**
   - Automated recovery for known error patterns
   - Manual intervention workflows for critical failures
   - Data reconciliation processes

## 6. Database Schema Overview

### Core Entities

| Entity | Purpose | Key Relationships |
|--------|---------|-------------------|
| `users` | User accounts | teams, profiles |
| `teams` | Organization units | users, subscriptions |
| `bank_accounts` | Financial accounts | transactions, teams |
| `transactions` | Financial activities | bank_accounts, categories |
| `documents` | Stored files | transactions, teams |
| `categories` | Transaction grouping | transactions, teams |
| `customers` | Client information | invoices, teams |
| `invoices` | Billing documents | customers, teams |
| `time_entries` | Time tracking | projects, users |

### Entity Relationships

```
users ──┬── teams ──┬── bank_accounts ── transactions
        │           │
        │           ├── documents
        │           │
        │           ├── customers ── invoices
        │           │
        └── profiles └── time_entries ── projects
```

### Indexing Strategy

1. **Primary Keys**
   - UUID for all entities
   - Sequential IDs avoided for security

2. **Foreign Keys**
   - Enforced at database level
   - Indexed for join performance

3. **Performance Indexes**
   - Compound indexes for common queries
   - Full-text search indexes for search functionality
   - Temporal indexes for date-based queries

### Migration Patterns

1. **Schema Evolution**
   - Versioned migrations in `apps/api/migrations/`
   - Forward-only migration strategy
   - Zero-downtime compatible changes

2. **Data Migrations**
   - Separate from schema migrations
   - Background processing for large datasets
   - Progress tracking and resumability

## 7. API Endpoint Documentation

### REST Endpoints

The API exposes REST endpoints primarily for:

1. **Public API Access**
   - `/api/v1/transactions`
   - `/api/v1/accounts`
   - `/api/v1/documents`

2. **Webhook Handlers**
   - `/webhooks/providers/:provider`
   - `/webhooks/payments/:gateway`

3. **Health and Monitoring**
   - `/health`
   - `/metrics`

### tRPC Procedures

Internal applications use tRPC procedures organized by domain:

1. **User Management**
   - `user.me`
   - `user.update`
   - `user.preferences`

2. **Financial Operations**
   - `transactions.list`
   - `transactions.create`
   - `transactions.update`
   - `transactions.delete`

3. **Team Management**
   - `team.create`
   - `team.update`
   - `team.invite`
   - `team.members`

### OpenAPI Specifications

The Engine component provides OpenAPI documentation at:
- Development: `http://localhost:3002/openapi`
- Production: `https://engine.midday.ai/openapi`

This documentation is automatically generated and kept in sync with the implementation.

## 8. Security Architecture

### Authentication Mechanisms

1. **User Authentication**
   - JWT-based authentication via Supabase Auth
   - Social login providers (Google, GitHub)
   - MFA support for enhanced security

2. **API Authentication**
   - Bearer token authentication
   - API key authentication for service accounts
   - Short-lived tokens with automatic refresh

3. **Service-to-Service Authentication**
   - Mutual TLS for critical services
   - Service account tokens with limited scope
   - IP-restricted access for internal services

### Authorization and Permission Models

1. **Role-Based Access Control**
   - Predefined roles (Admin, Member, Viewer)
   - Permission inheritance through roles
   - Custom role definitions for enterprise customers

2. **Resource-Level Permissions**
   - Team-based isolation of data
   - Object-level permission checks
   - Attribute-based access control for sensitive data

3. **Permission Enforcement**
   - Centralized middleware for consistent checks
   - Database-level row