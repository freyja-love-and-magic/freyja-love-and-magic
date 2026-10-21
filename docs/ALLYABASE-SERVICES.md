# Allyabase Services

Allyabase is Planet Nine's Backend as a Service (BaaS) providing a collection of 12+ microservices that enable the entire ecosystem.

## Service Overview

Each service follows the pattern:
- `README.md` describing endpoints
- Server code in `src/server/node/*.js`
- Client SDKs in `src/client/javascript/*.js`
- Tests in `test/mocha/`

## Core Services

### **addie** - Payment Processing
**Location**: `/allyabase/addie/`

Payment processing and transaction splitting service.

**Key Features**:
- Multi-party payment splits (70% creator, 20% base, 10% site)
- Stripe integration with sessionless authentication
- Real payment intent creation with cryptographic signatures
- Transaction coordination across Planet Nine bases

**Main Endpoints**:
- `POST /payment-intent` - Create payment intent with splits
- `POST /confirm-payment` - Confirm payment completion
- `GET /transaction/:id` - Retrieve transaction details

### **aretha** - Limited-Run Products
**Location**: `/allyabase/aretha/`

Limited-run product allocation for tickets, event access, and time-based rentals.

**Key Features**:
- Inventory management for scarce resources
- Time-based access control
- Allocation fairness algorithms
- Integration with payment systems

### **bdo** - Big Dumb Object Storage
**Location**: `/allyabase/bdo/`

Distributed object storage system serving as the backbone for cross-service data sharing.

**Key Features**:
- Cryptographically signed object storage
- Per-object public key authentication
- Cross-base object sharing
- Simple PUT/GET operations with sessionless auth

**Main Endpoints**:
- `PUT /bdo` - Store object with public key
- `GET /bdo/:pubKey` - Retrieve object by public key
- `DELETE /bdo/:pubKey` - Delete object (owner only)

**Usage Pattern**:
```javascript
// Store object
const response = await bdoClient.put(objectData, userPubKey, signature);

// Retrieve object
const object = await bdoClient.get(publicKey);
```

### **continuebee** - Client State Verification
**Location**: `/allyabase/continuebee/`

Client state verification and synchronization service.

**Key Features**:
- State consistency across devices
- Cryptographic state verification
- Conflict resolution mechanisms
- Offline state management

### **dolores** - Social Feeds & Media
**Location**: `/allyabase/dolores/`

Short-form video storage and social media management.

**Key Features**:
- Video upload and transcoding
- Tagging and categorization
- Social feed algorithms
- PostWidget component system for consistent UI

**Key Components**:
- **PostWidget**: Universal UI component for consistent post display
- **Feed Management**: Algorithm-driven content curation
- **Media Processing**: Video transcoding and optimization

### **fount** - MAGIC Protocol Integration
**Location**: `/allyabase/fount/`

MAGIC protocol integration and rewards distribution.

**Key Features**:
- Spell resolution and execution
- Nineum (in-app currency) management
- Cross-service reward distribution
- MAGIC protocol gateway functionality

**Main Endpoints**:
- `POST /magic/spell/:spellName` - Execute MAGIC spell
- `GET /user/:uuid` - Get user balance and status
- `POST /user` - Create fount user
- `GET /spellbook/:pubKey` - Retrieve user spellbook

### **joan** - Account Recovery
**Location**: `/allyabase/joan/`

Account recovery system for cryptographic key management.

**Key Features**:
- Social recovery mechanisms
- Key derivation and restoration
- Recovery phrase management
- Multi-factor recovery options

### **julia** - P2P Messaging
**Location**: `/allyabase/julia/`

Peer-to-peer messaging associations and routing.

**Key Features**:
- End-to-end encrypted messaging
- Contact management and discovery
- Message routing across bases
- Group messaging support

### **minnie** - Email Handling
**Location**: `/allyabase/minnie/`

Email processing and delivery service.

**Key Features**:
- Transactional email delivery
- Template management
- Privacy-focused email routing
- Anti-spam and filtering

### **pref** - Preferences Storage
**Location**: `/allyabase/pref/`

Key/value preference storage with encryption.

**Key Features**:
- Encrypted preference storage
- Cross-device synchronization
- Hierarchical preference management
- Backup and restore capabilities

### **sanora** - Product Hosting
**Location**: `/allyabase/sanora/`

Lightweight product hosting and marketplace functionality.

**Key Features**:
- Digital product management (ebooks, courses, software)
- Marketplace discovery via `/products/base` endpoint
- Teleportable product feeds via `/teleportable-products`
- File upload and management
- Product metadata and pricing

**Main Endpoints**:
- `GET /products/base` - Get all products from base
- `GET /teleportable-products` - Get embeddable product feed
- `POST /product` - Create new product
- `GET /product/:id` - Get product details
- `POST /upload` - Upload product files

## Service Architecture Patterns

### Sessionless Authentication
All services use sessionless protocol for authentication:

```javascript
// Standard authentication pattern
const signature = sessionless.sign(message, privateKey);
const headers = {
    'X-Sessionless-Signature': signature,
    'X-Sessionless-PubKey': publicKey
};
```

### BDO Integration Pattern
Services store and retrieve data using BDO:

```javascript
// Store data in BDO
const bdoResponse = await bdo.put(data, userPubKey, signature);

// Retrieve data from BDO
const storedData = await bdo.get(publicKey);

// Return BDO reference for other services
return {
    success: true,
    bdoUuid: bdoResponse.uuid,
    pubKey: userPubKey
};
```

### Error Handling Standard
```javascript
// Consistent error response format
const errorResponse = {
    success: false,
    error: 'DESCRIPTIVE_ERROR_CODE',
    message: 'Human-readable error message',
    timestamp: new Date().toISOString()
};
```

## Development Environment

### Local Development
```bash
# Start individual service
cd allyabase/[service-name]
npm install
node src/server/node/[service].js

# Or use PM2 for all services
pm2 start ecosystem.config.js
```

### Docker Development
```bash
# Complete 3-base ecosystem
cd allyabase/deployment/docker
./test-complete-ecosystem.sh

# Individual service testing
docker build -t [service-name] .
docker run -p [port]:[port] [service-name]
```

### Port Allocation

**Core Services (3000-3006)**:
- **BDO**: Port 3003 - Object storage
- **Fount**: Port 3002 - MAGIC protocol
- **Addie**: Port 3005 - Payment processing
- **Pref**: Port 3001 - Preferences
- **Continuebee**: Port 3000 - State verification
- **Joan**: Port 3004 - Account recovery
- **Julia**: Port 3006 - P2P messaging

**Independent Services**:
- **Dolores**: Port 3007 - Social feeds
- **Prof**: Port 3008 - Profile management
- **Sanora**: Port 7243 - Product hosting
- **Minnie**: Port 3009 - Email handling
- **Aretha**: Port 3010 - Limited products

## Testing Framework

### Service Testing
```bash
# Test individual service
cd allyabase/[service]/test/mocha
npm test

# Test all services
./test-em-all.sh

# Integration testing
cd deployment/docker
./test-all-services.sh [base-port]
```

### Test Patterns
```javascript
// Standard test structure
describe('Service Name', () => {
    before(async () => {
        // Setup test environment
    });

    describe('Authentication', () => {
        it('should accept valid sessionless signatures', async () => {
            // Test sessionless auth
        });
    });

    describe('Core Functionality', () => {
        it('should handle main use case', async () => {
            // Test primary functionality
        });
    });

    after(async () => {
        // Cleanup
    });
});
```

## Service Dependencies

### Dependency Graph
```
BDO (foundation)
├── Sanora (products) → BDO
├── Dolores (posts) → BDO
├── Fount (MAGIC) → BDO
└── Pref (preferences) → BDO

Addie (payments)
├── → Sanora (product info)
├── → BDO (transaction records)
└── → Fount (nineum handling)

Julia (messaging)
├── → BDO (contact storage)
└── → Joan (recovery)
```

### Cross-Service Communication
```javascript
// Services communicate via HTTP APIs
const sanoraClient = {
    getProduct: async (productId) => {
        return await fetch(`${sanoraUrl}/product/${productId}`);
    }
};

const bdoClient = {
    store: async (data, pubKey, signature) => {
        return await fetch(`${bdoUrl}/bdo`, {
            method: 'PUT',
            headers: {
                'X-Sessionless-Signature': signature,
                'X-Sessionless-PubKey': pubKey
            },
            body: JSON.stringify(data)
        });
    }
};
```

## Security Considerations

### Authentication Security
- All requests authenticated with sessionless signatures
- Public key verification for all operations
- No shared secrets or password-based auth
- Rate limiting on all endpoints

### Data Security
- Cryptographic signatures for all stored data
- Per-object access control in BDO
- Encrypted data storage where appropriate
- Audit trails for sensitive operations

### Network Security
- HTTPS enforcement in production
- CORS configuration for cross-origin requests
- Input validation and sanitization
- SQL injection prevention (where applicable)

## Deployment Patterns

### Production Deployment
```bash
# Individual service deployment
docker build -t [service-name]:latest .
docker run -d -p [port]:[port] [service-name]:latest

# Kubernetes deployment
kubectl apply -f k8s-deployment.yaml

# PM2 process management
pm2 start ecosystem.config.js --env production
```

### Environment Configuration
```javascript
// Environment-specific configuration
const config = {
    development: {
        port: process.env.PORT || 3000,
        cors: { origin: '*' },
        logging: 'debug'
    },
    production: {
        port: process.env.PORT || 3000,
        cors: { origin: process.env.ALLOWED_ORIGINS?.split(',') },
        logging: 'error'
    }
};
```

The allyabase service architecture provides a robust, scalable foundation for the Planet Nine ecosystem, with each service designed for independence while enabling powerful cross-service integrations through standardized patterns and protocols.