# Planet Nine Ecosystem - Claude Context

## ⚡ COMMANDMENTS ⚡

1. **NO MOCKING**: Never implement mock data, fake signatures, or placeholder functionality. Everything must be real and production-ready.
2. **NO FALLBACKS**: When something fails, it fails clearly with informative error messages. No fallback data that masks real issues.
3. **NO GIT COMMANDS**: Never run git commands (add, commit, push, etc.) unless explicitly requested by the user. Always ask permission first.

## Overview

The Planet Nine ecosystem is a comprehensive collection of open-source software designed to create an interoperable, privacy-focused alternative to traditional web services. The ecosystem consists of three main layers: **allyabase** (backend services), **The Stack** (core protocols), and **the-nullary** (client applications).

## Core Philosophy

- **Interoperability over Federation**: Unlike federated systems (like ActivityPub), Planet Nine aims for true interoperability where users can connect across services without being locked into specific platforms
- **Privacy by Design**: Uses cryptographic keys for authentication without requiring personal information like emails or passwords
- **Open Source & Self-Hostable**: All components can be self-hosted, avoiding vendor lock-in
- **No Advertising Model**: Designed to work without surveillance capitalism or advertising revenue
- **Production Code Only**: No mock data, test stubs, or demo content in production applications - all implementations must use real service integrations

## 📚 Detailed Documentation

This main document provides an overview. For detailed information, see our modular documentation:

- **[Allyabase Services](docs/ALLYABASE-SERVICES.md)** - Complete backend service documentation (12+ microservices)
- **[The Stack Protocols](docs/THE-STACK-PROTOCOLS.md)** - sessionless, MAGIC, teleportation protocols
- **[The Nullary Apps](docs/THE-NULLARY-APPS.md)** - All client applications and SVG component system
- **[Development Environment](docs/DEVELOPMENT-ENVIRONMENT.md)** - Setup, testing, and deployment
- **[Testing Ecosystem](docs/TESTING-ECOSYSTEM.md)** - Complete 6-phase testing system

## Architecture Overview

### 1. **allyabase** (Backend as a Service)
**Location**: `/allyabase/`

12+ microservices providing backend functionality:
- **BDO**: Big Dumb Object storage - cryptographically signed distributed storage
- **Sanora**: Product hosting with marketplace and teleportation endpoints
- **Addie**: Payment processing with multi-party transaction splitting
- **Fount**: MAGIC protocol integration and nineum currency management
- **Dolores**: Social feeds and PostWidget component system
- **Plus**: Joan (recovery), Julia (P2P messaging), Minnie (email), Pref (preferences), Aretha (limited products), Continuebee (state verification)

### 2. **The Stack** (Core Protocols)
Foundational protocols enabling interoperability:

#### **sessionless** - Passwordless authentication using secp256k1 keys
#### **MAGIC** - Multi-device consensus protocol for secure transactions
#### **teleportation** - Content discovery and verification across bases

### 3. **the-nullary** (Client Applications)
**Location**: `/the-nullary/`

16+ cross-platform applications built with Tauri and SVG-first architecture:
- **Rhapsold**: Minimalist blogging platform (flagship reference implementation)
- **Ninefy**: Digital goods marketplace with type-specific product forms
- **StackChat**: P2P messaging with cross-base communication
- **MyBase**: Personal base management and content aggregation
- **Nexus**: Web-based ecosystem portal showcasing all Planet Nine services
- **Plus**: Screenary, IDO This, Grocary, Viewary, Lexary, Photary, Blogary, Viewaris, Eventary, MagiCard

### 4. **the-advancement** (Browser Extensions)
**Location**: `/the-advancement/`

Privacy-focused browser extensions serving as the consumer gateway:
- **Safari Extension**: Complete Planet Nine integration with native cryptography
- **Chrome Extension**: Input detection and typing simulation
- **Payment Processing**: Multi-party Stripe integration (70% creator, 20% base, 10% site)
- **Ad Covering System**: Dual-mode experience (peaceful plants OR interactive monsters)
- **MAGIC Protocol**: Universal spell casting across applications
- **Emojicoding**: Revolutionary UUID ↔ emoji conversion system

## Quick Start

### Running Services Locally
```bash
# Docker (recommended)
cd allyabase/deployment/docker
./test-complete-ecosystem.sh

# Individual services
cd allyabase/[service-name]
npm install && node src/server/node/[service].js

# PM2 for all services
pm2 start ecosystem.config.js
```

### Building Client Apps
```bash
# Nullary applications
cd the-nullary/[app-name]
npm run tauri dev

# Browser extensions
cd the-advancement/src/extensions/safari
swift build -c release
```

### Testing
```bash
# Complete ecosystem testing (6-phase validation)
cd allyabase/deployment/docker
./test-complete-ecosystem.sh

# Individual service tests
cd allyabase/[service]/test/mocha
npm test
```

## Key Features & Milestones

### ✅ **Complete Commerce Infrastructure** (January 2025)
- **Decentralized Payment Processing**: Multi-party transaction splitting without intermediaries
- **Cross-Base Commerce**: Purchase from any Planet Nine base through The Advancement extension
- **Real Cryptography**: Production-ready secp256k1 implementation with compressed keys
- **Teleported Marketplace**: Products discoverable across the entire Planet Nine network

### ✅ **MAGIC Protocol Implementation** (January 2025)
- **Universal Spell Casting**: Complete spell system across all 16+ nullary applications
- **Background Management**: Centralized spellbook with Swift cryptographic integration
- **Decision Tree Navigation**: Intelligent menu systems with automatic product resolution
- **Real Nineum Integration**: Live currency tracking and spell cost management

### ✅ **Complete Testing Ecosystem** (January 2025)
- **6-Phase Validation**: Infrastructure → Services → Apps → Cross-Base → Portal → Integration
- **3-Base Docker System**: Complete multi-base testing with port isolation
- **Cross-Base P2P Testing**: StackChat messaging validation across different bases
- **Content Aggregation**: MyBase pulling content from multiple independent bases

### ✅ **SVG-First Client Architecture** (January 2025)
- **16+ Applications**: Complete suite of cross-platform apps with shared component system
- **Layered UI System**: Sophisticated multi-layer interfaces with HUD overlays
- **Environment Switching**: Instant dev/test/local environment switching across all apps
- **Universal Base Management**: Shared base discovery and management across all applications

## Development Environment

### Environment Configuration System
All components support unified environment switching:

- **`dev`** - Production dev server (https://dev.*.allyabase.com)
- **`test`** - Local 3-base test ecosystem (localhost:5111-5122)
- **`local`** - Standard local development (localhost:3000-3008)

### Port Allocation
**Core Services**: BDO (3003), Fount (3002), Addie (3005), Sanora (7243)
**Test Environment**: 3-base Docker system (ports 5114-5128)
**Applications**: Nexus Portal (3333), The Advancement Test Server (3456)

### Key Files & Directories
- `build-em-all.sh`: Build all services
- `test-em-all.sh`: Run all tests
- `ecosystem.config.js`: PM2 configuration
- `data/`: Local file storage for development
- `artifacts/`: Generated content and exports

## Production Readiness

### Current Status (January 2025)
- **✅ Safari Extension**: Production-ready with complete ecosystem integration
- **✅ Payment Infrastructure**: Multi-party commerce without intermediaries
- **✅ Testing System**: 6-phase validation ensuring ecosystem reliability
- **✅ Cross-Base Functionality**: P2P messaging and content aggregation working
- **✅ Real Cryptography**: Production secp256k1 with compressed keys throughout

### Component Development Rules
1. **Shared Code First**: Always check `/the-nullary/shared/` before adding app-specific functionality
2. **No Mock Data**: All implementations must use real service integrations
3. **Environment Support**: All components must support dev/test/local environments
4. **Testing Required**: New features require tests and integration validation

## Ecosystem Impact

Planet Nine represents a significant effort to rebuild web infrastructure with **privacy, interoperability, and user control** as primary concerns. Key achievements:

- **First Working Decentralized Commerce**: Complete payment infrastructure without intermediaries
- **Universal Cryptographic Authentication**: Sessionless protocol across all components
- **Cross-Base Interoperability**: True interoperability vs traditional federation
- **Complete Testing Infrastructure**: 6-phase validation ensuring production readiness
- **Privacy-First Design**: No surveillance, tracking, or advertising-based revenue models

---

**Planet Nine** demonstrates that it's possible to build comprehensive, production-ready alternatives to traditional web services while maintaining user privacy, enabling true interoperability, and providing creators with fair compensation without surveillance capitalism.
