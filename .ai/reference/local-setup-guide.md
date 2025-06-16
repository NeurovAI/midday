# 🚀 Midday Local Development Setup Guide

## Prerequisites

- Node.js (✅ You have v23.7.0)
- Bun package manager (needs installation)
- Supabase account (free tier available)
- Upstash account (free tier available)

## Quick Start

### 1. Install Bun
```bash
curl -fsSL https://bun.sh/install | bash
# Restart your terminal or run:
source ~/.bashrc  # or ~/.zshrc
```

### 2. Install Dependencies
```bash
bun install
```

### 3. Set Up Environment Variables

#### Dashboard (.env.local in apps/dashboard/)
```bash
# Copy the example file
cp apps/dashboard/.env-example apps/dashboard/.env.local

# Edit with your values:
# Minimal setup for local development:
NEXT_PUBLIC_SUPABASE_URL=http://localhost:54321
NEXT_PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
NEXT_PUBLIC_URL=http://localhost:3001
NEXT_PUBLIC_API_URL=http://localhost:3003
SUPABASE_SERVICE_KEY=your_supabase_service_key
```

#### API (.env in apps/api/)
```bash
# Copy the example file
cp apps/api/.env-template apps/api/.env

# Edit with your values:
SUPABASE_URL=your_supabase_url
SUPABASE_SERVICE_KEY=your_supabase_service_key
SUPABASE_JWT_SECRET=your_jwt_secret
DATABASE_PRIMARY_URL=your_database_url
```

### 4. Available Development Commands

```bash
# Run all services in development mode
bun dev

# Run specific services
bun dev:dashboard    # Dashboard on http://localhost:3001
bun dev:api         # API on http://localhost:3003
bun dev:website     # Website on http://localhost:3000
bun dev:engine      # Engine on http://localhost:3002
bun dev:desktop     # Desktop app

# Build everything
bun build

# Run tests
bun test
```

## Service Setup Requirements

### Essential Services

#### Supabase (Database & Auth)
1. Create account at https://supabase.com
2. Create new project
3. Get your project URL and anon key from Settings > API
4. Set up database schema (migrations in apps/api/migrations/)

#### Upstash (Redis)
1. Create account at https://upstash.com
2. Create Redis database
3. Get REST URL and token

### Optional Services (for full features)

#### Banking Integrations
- **Plaid**: US/Canada bank connections
- **GoCardless**: European bank connections  
- **Teller**: US bank connections

#### AI Features
- **OpenAI**: For AI assistant features
- **Mistral AI**: Alternative AI provider

#### Other Services
- **Resend**: Email sending
- **Trigger.dev**: Background jobs
- **Novu**: Notifications
- **Dub**: URL shortening
- **Azure**: Document intelligence/OCR

## Minimal Working Setup

For a basic local development environment, you only need:

1. **Supabase** (free tier)
2. **Upstash** (free tier) 
3. Basic environment variables

This will give you:
- ✅ User authentication
- ✅ Database functionality
- ✅ Basic dashboard features
- ❌ Bank connections (requires provider APIs)
- ❌ AI features (requires OpenAI API)
- ❌ Email sending (requires Resend)

## Architecture Overview

The application runs multiple services:
- **Dashboard**: http://localhost:3001 (Main web app)
- **API**: http://localhost:3003 (Backend API)
- **Engine**: http://localhost:3002 (Financial data processing)
- **Website**: http://localhost:3000 (Marketing site)

## Troubleshooting

### Common Issues
1. **Bun not found**: Make sure to restart terminal after installation
2. **Port conflicts**: Check if ports 3000-3003 are available
3. **Database errors**: Verify Supabase connection and run migrations
4. **Missing dependencies**: Run `bun install` in project root

### Getting Help
- Documentation: https://docs.midday.ai
- Discord: https://go.midday.ai/anPiuRx
- GitHub Issues: https://github.com/midday-ai/midday/issues

## License Note
This project uses AGPL-3.0 license for non-commercial use. Commercial use requires a separate license.
