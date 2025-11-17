# 4E Store - Discord-Integrated E-Commerce Platform

## Overview

4E Store is a premium gaming and digital products e-commerce platform with deep Discord integration. The application enables users to browse products, manage shopping carts, and place orders with real-time Discord notifications. Orders are tracked through a unique order code system, with status updates delivered via Discord bot to designated channels.

The platform features a modern tech/gaming aesthetic with purple-themed UI, inspired by Discord, Steam, and Epic Games Store design patterns.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

**Technology Stack:**
- **Framework:** React with TypeScript
- **Routing:** Wouter (lightweight client-side routing)
- **State Management:** TanStack Query (React Query) for server state
- **UI Components:** Shadcn/ui components built on Radix UI primitives
- **Styling:** Tailwind CSS with custom design tokens

**Design System:**
- Custom color palette focused on purple/violet tones for gaming aesthetic
- Typography hierarchy using Inter (primary) and Space Grotesk (accent/monospace)
- Glassmorphic effects and purple glow shadows for premium feel
- Mobile-first responsive design with specific breakpoints (md, lg)

**Key Pages:**
- Home: Hero section with gradient backgrounds and CTAs
- Products: Filterable catalog with search and category filters
- Dashboard: User order history and tracking
- Cart Overlay: Slide-in shopping cart interface
- Checkout Dialog: Multi-step checkout with Discord username collection

**State Management Approach:**
- Cart state stored in localStorage for persistence
- User authentication state stored in localStorage (Discord OAuth placeholder)
- Server data cached via React Query with no automatic refetching
- Order creation handled through mutation with optimistic updates

### Backend Architecture

**Technology Stack:**
- **Runtime:** Node.js with Express
- **Language:** TypeScript (ESM modules)
- **ORM:** Drizzle ORM
- **Database:** PostgreSQL (via Neon serverless driver)
- **Session Management:** Express sessions with PostgreSQL store

**API Design:**
- RESTful endpoints under `/api` prefix
- Resource-based routes (products, orders)
- JSON request/response format
- Basic error handling with Zod validation

**Key Endpoints:**
- `GET /api/products` - List all products
- `GET /api/products/:id` - Single product details
- `POST /api/products` - Create product (admin)
- `GET /api/orders` - List orders
- `GET /api/orders/:id` - Single order details
- `POST /api/orders` - Create new order
- `PATCH /api/orders/:id/status` - Update order status
- `PATCH /api/orders/:id/payment-link` - Add payment link

**Data Storage Strategy:**
- In-memory storage (MemStorage) as fallback/development mode
- Production uses PostgreSQL via Drizzle ORM
- Order codes generated with format: `4e-{random-4-digit}`

### Database Schema

**Core Tables:**

1. **Users Table**
   - Stores Discord OAuth user information
   - Fields: id (UUID), discordId (unique), discordUsername, discordAvatar
   - Purpose: User authentication and identification

2. **Products Table**
   - Digital product catalog
   - Fields: id (UUID), name, description, price (decimal), category, imageUrl, inStock (integer)
   - Purpose: Product inventory management

3. **Orders Table**
   - Order tracking and management
   - Fields: id (UUID), orderCode (unique), userId, customerDiscordUsername, customerDiscordId, status, totalAmount, paymentLink, timestamps
   - Status values: pending, in_progress, payment_pending, completed, cancelled
   - Purpose: Order lifecycle tracking

4. **Order Items Table**
   - Line items for each order
   - Fields: id (UUID), orderId, productId, productName, productPrice, quantity
   - Purpose: Detailed order composition

**Schema Design Decisions:**
- UUID primary keys for security and scalability
- Denormalized product data in order items to preserve historical prices
- Order code as separate unique field for customer-friendly identifiers
- Decimal type for monetary values to avoid floating-point issues

### Authentication & Authorization

**Current Implementation:**
- Simulated Discord OAuth (stored in localStorage)
- No actual authentication middleware
- User data structure: discordUsername, discordAvatar, discordId
- Session-based approach planned (express-session with PostgreSQL store)

**Security Considerations:**
- Admin endpoints currently unprotected (MVP stage)
- Production requires Discord OAuth2 integration
- Session store configured for PostgreSQL but not actively used

### External Dependencies

**Discord Bot Integration:**
- **Library:** discord.js v14
- **Purpose:** Real-time order notifications to Discord channels
- **Features:**
  - New order notifications with embedded rich content
  - Order status update notifications
  - Slash commands for order management (/order, /cancel, /complete, /setlink)
  - Owner/co-owner permission system
- **Configuration:**
  - DISCORD_BOT_TOKEN: Bot authentication
  - DISCORD_OWNER_ID: Primary admin user
  - DISCORD_CO_OWNER_ID: Secondary admin user
  - DISCORD_LOGS_CHANNEL_ID: Channel for order notifications
  - DISCORD_SERVER_ID: Target Discord server

**Database Service:**
- **Provider:** Neon (serverless PostgreSQL)
- **Driver:** @neondatabase/serverless
- **Connection:** Via DATABASE_URL environment variable
- **Migration Tool:** Drizzle Kit for schema management

**UI Component Library:**
- **Shadcn/ui:** Pre-built accessible components
- **Radix UI:** Primitive components for complex interactions
- **Configuration:** New York style variant with neutral base color

**Build Tools:**
- **Vite:** Frontend build tool and dev server
- **ESBuild:** Backend bundling for production
- **TypeScript:** Type checking across full stack
- **Replit Plugins:** Development tooling (error overlay, cartographer, dev banner)

**Additional Services:**
- **Fonts:** Google Fonts CDN (Inter, Space Grotesk)
- **Icons:** Lucide React icons, React Icons (Discord)