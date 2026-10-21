# Development Environment

Planet Nine provides a comprehensive development environment supporting multiple deployment scenarios and testing configurations.

## Environment Architecture

### Three-Environment System

All Planet Nine components support unified environment switching:

#### **DEV Environment**
- **Purpose**: Production development servers
- **URLs**: `https://dev.*.allyabase.com`
- **Usage**: Testing against shared development infrastructure
- **Characteristics**: Stable, shared, persistent data

#### **TEST Environment**
- **Purpose**: Local 3-base ecosystem testing
- **URLs**: `http://127.0.0.1:5114-5122` (Docker containers)
- **Usage**: Complete ecosystem validation with isolated data
- **Characteristics**: Ephemeral, isolated, full ecosystem

#### **LOCAL Environment**
- **Purpose**: Individual service development
- **URLs**: `http://localhost:3000-3008`
- **Usage**: Single service development and debugging
- **Characteristics**: Minimal, fast iteration, service-specific

### Port Allocation Strategy

#### Core Allyabase Services (3000-3006)
```
BDO (Big Dumb Object): Port 3003
Fount (MAGIC Protocol): Port 3002
Addie (Payment Processing): Port 3005
Pref (Preferences): Port 3001
Continuebee (State Verification): Port 3000
Joan (Account Recovery): Port 3004
Julia (P2P Messaging): Port 3006
```

#### Independent Services
```
Dolores (Social Feeds): Port 3007
Prof (Profile Management): Port 3008
Sanora (Product Hosting): Port 7243
Minnie (Email Handling): Port 3009
Aretha (Limited Products): Port 3010
```

#### Special Services
```
Nexus Portal (Web Showcase): Port 3333
The Advancement Test Server: Port 3456
```

#### TEST Environment (Docker Containers)
```
Base 1: Ports 5114-5116 (BDO: 5117, Sanora: 5118)
Base 2: Ports 5119-5121 (BDO: 5122, Sanora: 5123)
Base 3: Ports 5124-5126 (BDO: 5127, Sanora: 5128)
```

## Configuration Management

### Environment Configuration Files

**Location**: `/shared/services/environment-config.js`

```javascript
const ENVIRONMENTS = {
    dev: {
        bdo: 'https://dev.bdo.allyabase.com',
        sanora: 'https://dev.sanora.allyabase.com',
        addie: 'https://dev.addie.allyabase.com',
        fount: 'https://dev.fount.allyabase.com',
        dolores: 'https://dev.dolores.allyabase.com',
    },
    test: {
        bdo: 'http://127.0.0.1:5117',
        sanora: 'http://127.0.0.1:5118',
        addie: 'http://127.0.0.1:5116',
        fount: 'http://127.0.0.1:5116',
        dolores: 'http://127.0.0.1:5119',
    },
    local: {
        bdo: 'http://localhost:3003',
        sanora: 'http://localhost:7243',
        addie: 'http://localhost:3005',
        fount: 'http://localhost:3002',
        dolores: 'http://localhost:3007',
    }
};
```

### Quick Environment Switching

All nullary applications support instant environment switching:

```javascript
// Browser console commands
ninefyEnv.switch('test')        // Switch Ninefy to test
rhapsoldEnv.switch('dev')       // Switch Rhapsold to dev
screenaryEnv.switch('local')    // Switch Screenary to local

// Check current environment
ninefyEnv.current()             // Returns current environment
ninefyEnv.list()               // Lists available environments
```

### Environment Persistence

- **Storage**: `localStorage` with key `nullary-env`
- **Persistence**: Across app restarts and browser sessions
- **Default**: Apps default to `dev` environment if no preference stored
- **Instant**: Switching without code changes or rebuilds

## Development Workflows

### Local Service Development

#### Starting Individual Services
```bash
# Start BDO service
cd allyabase/bdo
npm install
node src/server/node/bdo.js

# Start Sanora service
cd allyabase/sanora
npm install
node src/server/node/sanora.js

# Start with PM2 for all services
pm2 start ecosystem.config.js
```

#### Testing Individual Services
```bash
# Test specific service
cd allyabase/bdo/test/mocha
npm test

# Test all services
./test-em-all.sh

# Run specific test file
npm test -- --grep "BDO storage operations"
```

### Complete Ecosystem Testing

#### Docker 3-Base System ✅
**Location**: `/allyabase/deployment/docker/`

```bash
# Start complete 3-base ecosystem
./test-complete-ecosystem.sh

# Test with specific options
./test-complete-ecosystem.sh --continue-on-failure --apps=core

# Test infrastructure only
./test-all-bases.sh --build

# Test specific base services
./test-all-services.sh 1000  # Test Base 1 services
```

#### Testing Phases
1. **Infrastructure Setup**: Start 3 allyabase instances
2. **Service Validation**: Test all microservices across all bases
3. **Application Testing**: Validate all nullary applications
4. **Cross-Base Interaction**: Test P2P and content aggregation
5. **Nexus Portal**: Visual demonstration with browser automation
6. **Integration Validation**: Generate reports and validate health

### Nullary Application Development

#### Tauri Development Setup
```bash
# Navigate to app directory
cd the-nullary/rhapsold/rhapsold

# Install dependencies
npm install

# Start development server
npm run tauri dev

# Build for production
npm run tauri build
```

#### Environment-Specific Development
```bash
# Develop against dev servers
npm run dev:dev

# Develop against test ecosystem
npm run dev:test

# Develop against local services
npm run dev:local
```

#### Shared Component Development
```bash
# Check shared components first
ls the-nullary/shared/components/

# Update shared component
edit the-nullary/shared/components/text.js

# Sync to all apps
cd the-nullary/shared/scripts
node sync-shared-code.js

# Test across multiple apps
cd the-nullary/rhapsold && npm run tauri dev &
cd the-nullary/ninefy && npm run tauri dev &
```

## Testing Infrastructure

### Automated Testing System ✅

#### Complete Ecosystem Tests
```bash
# Full ecosystem validation
cd allyabase/deployment/docker
./test-complete-ecosystem.sh

# Test specific components
./test-complete-ecosystem.sh --services-only
./test-complete-ecosystem.sh --apps-only
./test-complete-ecosystem.sh --integration-only
```

#### Service-Level Testing
```bash
# Test all allyabase services
./test-all-services.sh [base-port]

# Test specific service across all bases
./test-service.sh bdo

# Test cross-service integration
./test-integration.sh
```

#### Application Testing
```bash
# Test all nullary applications
cd nullary-tests
./test-all-apps.sh

# Test specific application
./test-rhapsold.sh 1000
./test-stackchat.sh 1000

# Test cross-base functionality
./test-cross-base.sh
```

### Test Data Management

#### Seeding Test Data
```bash
# Seed complete test environment
cd allyabase/deployment/docker
./seed-test-data.sh

# Seed specific service
./seed-service-data.sh sanora

# Create test users and content
./create-test-users.sh
```

#### Test Data Cleanup
```bash
# Clean all test data
./cleanup-test-data.sh

# Reset specific base
./reset-base.sh 1000

# Preserve specific data
./cleanup-test-data.sh --preserve-users
```

## Build and Deployment

### Service Building

#### Individual Service Build
```bash
# Build specific service
cd allyabase/bdo
docker build -t planet-nine/bdo:latest .

# Build all services
./build-em-all.sh

# Build with specific tag
docker build -t planet-nine/bdo:v1.2.3 .
```

#### Docker Compose Deployment
```bash
# Deploy all services
docker-compose up -d

# Deploy specific services
docker-compose up -d bdo sanora addie

# Scale services
docker-compose up -d --scale bdo=3
```

### Application Building

#### Tauri Cross-Platform Builds
```bash
# Build for current platform
npm run tauri build

# Build for specific platform
npm run tauri build -- --target x86_64-pc-windows-msvc

# Build universal binary (macOS)
npm run tauri build -- --target universal-apple-darwin
```

#### Web Application Builds
```bash
# Build nexus portal
cd the-nullary/nexus
npm run build

# Build static sites
npm run build:static

# Deploy to CDN
npm run deploy:production
```

## Environment Variables

### Service Configuration
```bash
# Common environment variables
export NODE_ENV=development
export PORT=3003
export CORS_ORIGIN="*"
export LOG_LEVEL=debug

# Database configuration
export DB_HOST=localhost
export DB_PORT=5432
export DB_NAME=planet_nine_dev

# Crypto configuration
export SESSIONLESS_KEYS_PATH="/path/to/keys"
export BDO_STORAGE_PATH="/path/to/storage"
```

### Application Configuration
```bash
# Nullary app configuration
export TAURI_ENV=development
export DEFAULT_ENVIRONMENT=dev
export API_BASE_URL=https://dev.bdo.allyabase.com

# The Advancement configuration
export STRIPE_PUBLISHABLE_KEY=pk_test_...
export HOME_BASE_URL=https://dev.bdo.allyabase.com
export DEBUG_MODE=true
```

## Development Tools

### Key Utilities

#### File Management
- **`build-em-all.sh`**: Build all services
- **`test-em-all.sh`**: Run all tests
- **`ecosystem.config.js`**: PM2 configuration

#### Data Management
- **`data/`**: Local file storage for development
- **`artifacts/`**: Generated content and exports
- **`images/`**: Shared image assets

#### Environment Scripts
- **`switch-environment.sh`**: Bulk environment switching
- **`health-check.sh`**: Service health monitoring
- **`port-scan.sh`**: Check port availability

### IDE Configuration

#### VS Code Setup
```json
{
    "recommendations": [
        "rust-lang.rust-analyzer",
        "tauri-apps.tauri-vscode",
        "bradlc.vscode-tailwindcss"
    ],
    "settings": {
        "rust-analyzer.cargo.target": "x86_64-unknown-linux-gnu",
        "tauri.dev.port": 1420
    }
}
```

#### Debug Configuration
```json
{
    "type": "node",
    "request": "launch",
    "name": "Debug Service",
    "program": "${workspaceFolder}/src/server/node/service.js",
    "env": {
        "NODE_ENV": "development",
        "DEBUG": "*"
    }
}
```

## Performance Monitoring

### Service Monitoring
```bash
# Monitor service performance
npm run monitor:services

# Check resource usage
docker stats

# Monitor logs
pm2 logs

# Performance profiling
npm run profile:service bdo
```

### Application Monitoring
```bash
# Monitor Tauri apps
npm run monitor:tauri

# Check bundle sizes
npm run analyze:bundle

# Performance testing
npm run test:performance
```

## Troubleshooting

### Common Issues

#### Port Conflicts
```bash
# Check port usage
lsof -i :3003

# Kill process on port
kill -9 $(lsof -t -i:3003)

# Use alternative ports
PORT=3004 npm start
```

#### Environment Switching Issues
```bash
# Clear environment cache
localStorage.removeItem('nullary-env')

# Reset to default environment
ninefyEnv.switch('dev')

# Check service availability
curl -f http://localhost:3003/health
```

#### Build Issues
```bash
# Clean build cache
npm run clean

# Rebuild dependencies
rm -rf node_modules package-lock.json
npm install

# Clear Tauri cache
npm run tauri build -- --clean
```

### Debugging Patterns

#### Service Debugging
```javascript
// Enable debug logging
process.env.DEBUG = 'planet-nine:*';

// Add request logging
app.use((req, res, next) => {
    console.log(`${req.method} ${req.path}`);
    next();
});
```

#### Application Debugging
```javascript
// Tauri debug mode
window.__TAURI_DEBUG__ = true;

// Environment debugging
console.log('Current environment:', getEnvironmentConfig());

// Service connectivity testing
await testServiceConnectivity();
```

The development environment provides comprehensive support for all stages of Planet Nine development, from individual service development to complete ecosystem testing and deployment.