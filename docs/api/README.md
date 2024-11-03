# Spatio Protocol API Documentation

## Overview

The Spatio Protocol API provides a comprehensive set of tools for building spatial-aware applications. This documentation covers the core APIs, extensions, and implementation patterns.

## Quick Start

```typescript
import { SpatioNode, SpatialAddress } from '@spatioprotocol/core';

// Initialize a node
const node = new SpatioNode({
    nodeId: 'my-node',
    config: {
        spatialRange: 1000,  // meters
        updateFrequency: 60  // Hz
    }
});

// Start broadcasting presence
await node.broadcast({
    position: currentPosition,
    radius: 100,
    ttl: 3600
});
```

## Core APIs

### SpatioNode

Primary interface for spatial operations.

```typescript
interface SpatioNode {
    // Lifecycle
    start(): Promise<void>;
    stop(): Promise<void>;
    
    // Presence
    broadcast(config: BroadcastConfig): Promise<void>;
    subscribe(filter: SpatialFilter): Promise<Subscription>;
    
    // Queries
    query(params: QueryParams): Promise<QueryResult>;
    
    // State Management
    updateState(state: SpatialState): Promise<void>;
    syncState(options: SyncOptions): Promise<void>;
}
```

### Spatial Addressing

Spatial location and addressing system.

```typescript
interface SpatialAddress {
    position: Vector3;
    orientation?: Quaternion;
    precision: number;
    confidence: number;
    timestamp: number;
}
```

### State Management

State synchronization and management.

```typescript
interface StateManager {
    // State Operations
    get(key: string): Promise<any>;
    set(key: string, value: any): Promise<void>;
    delete(key: string): Promise<void>;
    
    // Sync
    sync(): Promise<void>;
    subscribe(callback: StateCallback): Subscription;
}
```

## Extensions

### Commerce Extension

Support for spatial commerce operations.

```typescript
interface CommerceNode extends SpatioNode {
    // Vendor Operations
    registerVendor(config: VendorConfig): Promise<void>;
    updateStatus(status: VendorStatus): Promise<void>;
    
    // Order Management
    createOrder(order: Order): Promise<OrderResult>;
    updateOrder(orderId: string, update: OrderUpdate): Promise<void>;
}
```

### Presence Extension

Enhanced presence management.

```typescript
interface PresenceNode extends SpatioNode {
    // Presence Management
    setPresence(presence: Presence): Promise<void>;
    getPresence(entityId: string): Promise<Presence>;
    
    // Discovery
    findNearby(radius: number): Promise<Entity[]>;
    watchArea(bounds: BoundingVolume): Subscription;
}
```

### Gaming Extension

Gaming-specific functionality.

```typescript
interface GameNode extends SpatioNode {
    // Session Management
    createSession(config: GameConfig): Promise<GameSession>;
    joinSession(sessionId: string): Promise<void>;
    
    // Game State
    updateGameState(state: GameState): Promise<void>;
    syncGameState(): Promise<void>;
}
```

## Common Patterns

### Broadcasting Presence

```typescript
// Simple broadcast
await node.broadcast({
    position: myPosition,
    radius: 100
});

// Advanced broadcast with metadata
await node.broadcast({
    position: myPosition,
    radius: 100,
    metadata: {
        type: 'vendor',
        capacity: 50,
        status: 'active'
    }
});
```

### Spatial Queries

```typescript
// Basic nearby query
const results = await node.query({
    center: myPosition,
    radius: 1000,
    types: ['vendor', 'player']
});

// Complex spatial query
const results = await node.query({
    bounds: BoundingBox.fromCenter(myPosition, 1000),
    filter: {
        types: ['vendor'],
        attributes: {
            status: 'active',
            capacity: { gt: 0 }
        }
    },
    sort: {
        by: 'distance',
        order: 'asc'
    }
});
```

### State Synchronization

```typescript
// Basic state sync
await node.syncState({
    scope: 'global',
    strategy: 'latest'
});

// Advanced state sync
await node.syncState({
    scope: 'spatial',
    bounds: myBounds,
    strategy: 'merge',
    resolution: {
        spatial: 1.0,  // meter
        temporal: 100  // ms
    }
});
```

## Error Handling

```typescript
try {
    await node.broadcast(config);
} catch (error) {
    if (error instanceof SpatialError) {
        // Handle spatial-specific errors
        console.error('Spatial error:', error.code);
    } else {
        // Handle general errors
        console.error('General error:', error);
    }
}
```

## Best Practices

1. **Resource Management**
   - Implement proper cleanup
   - Handle subscription lifecycle
   - Manage bandwidth usage

2. **Error Handling**
   - Always catch spatial errors
   - Implement retry logic
   - Validate inputs

3. **Performance**
   - Use appropriate update frequencies
   - Implement batching
   - Handle state efficiently

## Type Definitions

Complete TypeScript definitions are available in the `@spatioprotocol/types` package.

## Examples

See the `/examples` directory for complete implementation examples:
- Basic presence broadcasting
- Spatial commerce implementation
- Gaming integration
- State synchronization

## Further Reading

- [Protocol Specification](../spec/README.md)
- [Implementation Guide](../implementation/README.md)
- [Security Guidelines](../security/README.md)

## Support

- GitHub Issues: [Report bugs](https://github.com/spatioprotocol/core/issues)
- Discord: [Join our community](https://discord.gg/spatioprotocol)
- Documentation: [Full documentation](https://docs.spatioprotocol.org)

---