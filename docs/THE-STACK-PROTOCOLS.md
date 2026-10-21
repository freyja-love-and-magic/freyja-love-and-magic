# The Stack - Core Protocols

The Stack provides foundational protocols that enable the Planet Nine ecosystem's interoperability, privacy, and decentralized functionality.

## Protocol Overview

The Stack consists of three core protocols:
1. **sessionless** - Authentication without passwords
2. **MAGIC** - Multi-device consensus and transactions
3. **teleportation** - Content discovery and verification

## sessionless Protocol

**Location**: `/sessionless/`

Authentication protocol using secp256k1 cryptographic keys, eliminating the need for passwords or personal information.

### Core Principles
- **No Shared Secrets**: No passwords or personal information required
- **Cryptographic Authentication**: secp256k1 elliptic curve signatures
- **Multi-Language Support**: JavaScript, Rust, Java, Python implementations
- **Stateless**: No server-side session management

### Key Methods

#### Key Generation
```javascript
// Generate cryptographic identity
const keys = sessionless.generateKeys(seedPhrase);
// Returns: { publicKey, privateKey, address }
```

#### Message Signing
```javascript
// Sign any message
const signature = sessionless.sign(message, privateKey);
// Returns cryptographic signature string
```

#### Signature Verification
```javascript
// Verify signatures
const isValid = sessionless.verifySignature(message, signature, publicKey);
// Returns: boolean
```

#### UUID Generation
```javascript
// Generate deterministic UUIDs
const uuid = sessionless.generateUUID();
// Returns: RFC 4122 compliant UUID
```

### Implementation Patterns

#### Client Authentication
```javascript
// Standard authentication flow
const message = JSON.stringify({
    action: 'authenticate',
    timestamp: Date.now(),
    nonce: Math.random()
});

const signature = sessionless.sign(message, privateKey);

const headers = {
    'X-Sessionless-Signature': signature,
    'X-Sessionless-PubKey': publicKey,
    'X-Sessionless-Message': message
};
```

#### Server Verification
```javascript
// Server-side verification
function verifyRequest(req) {
    const { signature, pubKey, message } = req.headers;

    if (!sessionless.verifySignature(message, signature, pubKey)) {
        throw new Error('Invalid signature');
    }

    const parsedMessage = JSON.parse(message);
    const age = Date.now() - parsedMessage.timestamp;

    if (age > 300000) { // 5 minute timeout
        throw new Error('Message too old');
    }

    return { verified: true, pubKey };
}
```

### Multi-Language Implementations

**JavaScript** (`/sessionless/src/sessionless.js`):
- Browser and Node.js compatible
- secp256k1 cryptographic operations
- Base64 encoding utilities

**Rust** (`/sessionless/src/lib.rs`):
- High-performance implementation
- WebAssembly compilation support
- Memory-safe cryptographic operations

**Python** (`/sessionless/sessionless.py`):
- Server-side integration
- Compatible with Django/Flask
- Scientific computing integration

**Java** (`/sessionless/Sessionless.java`):
- Enterprise application integration
- Android mobile support
- Spring Boot compatibility

## MAGIC Protocol

**Location**: `/MAGIC/`, `/magic-demo/`

Multi-device Asynchronous Generic Input/output Consensus protocol enabling secure transactions and interactions without traditional accounts.

### Core Concepts

#### Spells
Cryptographically signed actions that can be executed across the network:

```javascript
const spell = {
    name: 'buyProduct',
    cost: 500,        // Nineum cost
    mp: true,         // Uses MAGIC protocol
    destinations: [
        { stopName: 'merchant', stopURL: 'https://merchant.example.com/api/' },
        { stopName: 'fount', stopURL: 'https://fount.example.com/magic/' }
    ],
    resolver: 'fount' // Final resolution service
};
```

#### Spellbooks
Collections of available spells for users:

```javascript
const spellbook = {
    buyProduct: { /* spell definition */ },
    likePost: { /* spell definition */ },
    sendMessage: { /* spell definition */ }
};
```

#### Nineum Currency
In-app currency for MAGIC protocol transactions:
- **Earned**: Through content creation and engagement
- **Spent**: On spells and premium features
- **Transferred**: Between users and services

### Implementation Architecture

#### Spell Casting Flow
1. **Detection**: Client detects spell element (`<button spell="buyProduct">`)
2. **Spellbook Lookup**: Retrieve user's available spells
3. **Cost Check**: Verify sufficient nineum balance
4. **Signature**: Cryptographically sign spell payload
5. **Execution**: Send to destinations for processing
6. **Resolution**: Final result from resolver service

#### Gateway Integration
```javascript
// MAGIC gateway implementation
const gateway = require('magic-gateway-js').default;

gateway.expressApp(
    app,           // Express app
    fountUser,     // User context
    spellbook,     // Available spells
    'gateway-name', // Gateway identifier
    sessionless,   // Authentication
    extraConfig,   // Additional configuration
    onSuccess      // Success callback
);
```

### Decision Tree Navigation System ✅

#### Magistack Selection Storage
```javascript
// Store user selections throughout navigation
const magistack = [];

function addSelection(choice) {
    magistack.push(choice);
    console.log('Current path:', magistack);
}

function lookupProduct() {
    // Use magistack to navigate catalog structure
    const product = catalog[magistack[0]][magistack[1]][magistack[2]];
    return product;
}
```

#### Nested Catalog Architecture
```javascript
// Products organized as nested decision trees
const catalog = {
    "adult": {
        "two-hour": {
            productId: "riding_lesson_adult_2h",
            bdoPubKey: "03a1b2c3...",
            price: 75,
            name: "Adult 2-Hour Riding Lesson"
        },
        "full-day": {
            productId: "riding_lesson_adult_full",
            bdoPubKey: "03d4e5f6...",
            price: 150,
            name: "Adult Full-Day Riding Experience"
        }
    },
    "child": {
        "one-hour": {
            productId: "riding_lesson_child_1h",
            bdoPubKey: "03g7h8i9...",
            price: 45,
            name: "Child 1-Hour Pony Ride"
        }
    }
};
```

#### Universal Spell Integration ✅
```javascript
// Dynamic loading across all nullary applications
function loadSpellSystems() {
    // Wait for environment configuration
    setTimeout(() => {
        if (typeof getServiceUrl === 'function') {
            // Load castSpell.js from fount service
            const fountUrl = getServiceUrl('fount');
            const script = document.createElement('script');
            script.src = `${fountUrl}/public/castSpell.js`;
            script.onload = () => console.log('✅ Spell system loaded');
            script.onerror = () => console.warn('⚠️ Spell system unavailable');
            document.head.appendChild(script);

        }
    }, 100);
}
```

### Spell Types

#### Selection Spells
Navigate decision trees while storing choices:
```javascript
const selectionSpell = {
    type: 'selection',
    choice: 'adult',
    nextCard: 'timeSpan',
    magistackUpdate: true
};
```

#### Lookup Spells
Resolve products based on stored selections:
```javascript
const lookupSpell = {
    type: 'lookup',
    useMagistack: true,
    resolveToProduct: true
};
```

#### MagiCard Spells
Navigate between BDO-stored cards:
```javascript
const magicardSpell = {
    type: 'magicard',
    targetPubKey: '03a1b2c3...',
    bdoNavigation: true
};
```

## Teleportation Protocol

**Location**: `/teleportation/`

Content discovery protocol enabling secure cross-service content sharing and verification.

### Complete Implementation (January 2025)

The teleportation system is fully operational with three integrated components:

#### 1. Content Providers (e.g., Sanora)
Generate cryptographically signed teleportable content:

```html
<!-- Teleportable HTML with signed content -->
<teleport signature="abc123..." pubkey="02a1b2c3...">
    <teleportal
        type="product"
        id="product_123"
        name="Advanced JavaScript Course"
        price="29.99"
        creator="Alice Developer"
        description="Complete JavaScript mastery course"
        image-url="https://example.com/course.jpg">
    </teleportal>
</teleport>
```

**Key Features**:
- **Cryptographic Signing**: All content signed with secp256k1 keys
- **Structured Data**: `<teleportal>` elements with metadata
- **Dynamic Generation**: Real-time content from database
- **Base Public Keys**: Stored in user objects for verification

#### 2. Teleportation Service (BDO)
Validates and serves teleported content:

```javascript
// Teleportation validation and serving
app.post('/teleport', async (req, res) => {
    const { url, pubKey } = req.body;

    // Fetch content from source
    const content = await fetch(url);
    const html = await content.text();

    // Verify cryptographic signature
    const isValid = verifyTeleportSignature(html, pubKey);

    if (isValid) {
        res.json({
            success: true,
            content: html,
            valid: true
        });
    } else {
        res.status(400).json({
            success: false,
            error: 'Invalid signature'
        });
    }
});
```

**Features**:
- **Signature Verification**: Validates content using provided public keys
- **Container Networking**: Supports `allyabase://` protocol for Docker environments
- **URL Translation**: Converts container URLs to accessible endpoints
- **Security**: Only serves verified, signed content

#### 3. Client Applications
Consume and display teleported content:

```javascript
// Request teleportation through BDO
async function getTeleportedContent(sourceUrl, pubKey) {
    const response = await fetch(`${bdoUrl}/teleport`, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ url: sourceUrl, pubKey })
    });

    const result = await response.json();

    if (result.valid) {
        // Parse teleportals and display content
        const teleportals = parseTeleportals(result.content);
        displayTeleportedContent(teleportals);
    }
}
```

### Container Networking (`allyabase://`)

Special protocol for Docker container environments:

```javascript
// URL translation for container networking
function translateContainerUrl(url) {
    if (url.startsWith('allyabase://')) {
        const service = url.split('//')[1];
        const portMap = {
            'sanora': '127.0.0.1:7243',
            'bdo': '127.0.0.1:3003',
            'fount': '127.0.0.1:3002'
        };
        return `http://${portMap[service]}`;
    }
    return url;
}
```

### Cross-Base Discovery

Enable marketplace aggregation across the network:

```javascript
// Discover content from multiple bases
const bases = [
    'https://base1.example.com',
    'https://base2.example.com',
    'https://base3.example.com'
];

const allProducts = await Promise.all(
    bases.map(base => getTeleportedContent(`${base}/teleportable-products`, base.pubKey))
);
```

## Protocol Integration Patterns

### Cross-Protocol Communication

#### sessionless + MAGIC
```javascript
// MAGIC spells authenticated with sessionless
const spellPayload = {
    spell: 'buyProduct',
    productId: 'item_123',
    timestamp: Date.now()
};

const signature = sessionless.sign(JSON.stringify(spellPayload), privateKey);
const magicRequest = { payload: spellPayload, signature };
```

#### MAGIC + BDO
```javascript
// Store spell results in BDO
const spellResult = await castSpell('processPayment', spellData);
const stored = await bdo.put(spellResult, userPubKey, signature);
```

### Environment Integration

All protocols support three-environment architecture:

```javascript
const protocolConfig = {
    DEV: {
        sessionless: 'https://dev.sessionless.allyabase.com',
        magic: 'https://dev.fount.allyabase.com',
        teleportation: 'https://dev.bdo.allyabase.com'
    },
    TEST: {
        sessionless: 'http://127.0.0.1:5115',
        magic: 'http://127.0.0.1:5116',
        teleportation: 'http://127.0.0.1:5117'
    },
    LOCAL: {
        sessionless: 'http://localhost:3001',
        magic: 'http://localhost:3002',
        teleportation: 'http://localhost:3003'
    }
};
```

## Security Considerations

### Cryptographic Standards
- **secp256k1**: Industry-standard elliptic curve
- **SHA-256**: Secure hashing for all signatures
- **Compressed Keys**: 02/03 prefix format for efficiency
- **Replay Protection**: Timestamp-based message validation

### Protocol-Specific Security

#### sessionless
- No server-side secret storage
- Client-side key generation and storage
- Message freshness validation
- Signature verification on every request

#### MAGIC
- Spell cost verification prevents spam
- Multi-destination routing for reliability
- Gateway validation and forwarding
- Nineum balance verification

#### Teleportation
- Cryptographic content signing
- Public key verification
- Content integrity validation
- Cross-origin security measures

#### Covenant
- Per-contract key isolation
- Multi-signature validation
- Step-by-step verification
- Participant authorization checks

The Stack protocols provide a robust foundation for decentralized, privacy-focused applications while maintaining security, scalability, and ease of use across the entire Planet Nine ecosystem.