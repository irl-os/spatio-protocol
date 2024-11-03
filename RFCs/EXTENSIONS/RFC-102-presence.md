# RFCs/EXTENSIONS/RFC-102-presence.md

# RFC-102: Real-time Presence Protocol
- Status: PROPOSED
- Type: EXTENSION
- Created: 2024-11-03
- Authors: Spatio Protocol Team
- Dependencies: ["RFC-001", "RFC-002", "RFC-003"]
- Replaces: None

## Abstract

This RFC defines the standard for real-time presence broadcasting and discovery within the Spatio Protocol. It establishes patterns for efficient presence management, spatial awareness, and dynamic entity discovery without requiring fixed infrastructure.

## Motivation

Current presence systems lack:
- Efficient spatial broadcasting
- Dynamic presence scoping
- Presence privacy controls
- Resource-aware scaling
- Context-aware discovery

## Specification

### 1. Presence Model

#### 1.1 Presence Entity
```rust
struct PresenceEntity {
    // Core identification
    entity_id: uuid,
    entity_type: EntityType,
    spatial_address: SpatialAddress,
    
    // Presence state
    status: PresenceStatus,
    visibility: VisibilityLevel,
    broadcast_config: BroadcastConfig,
    
    // Optional context
    context: PresenceContext,
    metadata: HashMap<String, Value>
}

enum EntityType {
    STATIC,         // Fixed location
    MOBILE,         // Moving entity
    TEMPORARY,      // Time-limited
    RECURRING,      // Regular pattern
    CUSTOM
}
```

#### 1.2 Presence Status
```rust
struct PresenceStatus {
    state: ActivityState,
    timestamp: uint64,
    duration: Option<Duration>,
    confidence: float32,
    update_frequency: uint16
}

enum ActivityState {
    ACTIVE,
    INACTIVE,
    BUSY,
    AWAY,
    CUSTOM(uint8)
}
```

### 2. Broadcasting Protocol

#### 2.1 Broadcast Configuration
```rust
struct BroadcastConfig {
    radius: float32,               // Meters
    frequency: uint16,             // Updates per minute
    precision: PrecisionLevel,
    power_mode: PowerMode,
    privacy_settings: PrivacySettings
}

enum PowerMode {
    HIGH_ACCURACY,    // Maximum precision
    BALANCED,         // Power/accuracy trade-off
    LOW_POWER,       // Minimal battery usage
    ADAPTIVE         // Context-dependent
}
```

#### 2.2 Broadcast Message
```rust
struct PresenceBroadcast {
    entity: PresenceEntity,
    broadcast_id: uuid,
    timestamp: uint64,
    ttl: uint32,
    spatial_scope: SpatialScope,
    payload: BroadcastPayload
}

struct SpatialScope {
    bounds: BoundingVolume,
    resolution: float32,
    max_propagation: uint16
}
```

### 3. Discovery Protocol

#### 3.1 Presence Query
```rust
struct PresenceQuery {
    spatial_bounds: BoundingVolume,
    entity_filter: EntityFilter,
    temporal_window: TimeWindow,
    resolution: QueryResolution,
    subscription: Option<SubscriptionConfig>
}

struct QueryResolution {
    spatial: float32,      // Meters
    temporal: uint32,      // Milliseconds
    attribute: float32     // Precision 0-1
}
```

#### 3.2 Subscription Management
```rust
struct SubscriptionConfig {
    update_frequency: uint16,
    priority: Priority,
    filter_updates: bool,
    batch_updates: bool,
    expiration: Option<uint64>
}
```

### 4. Presence Optimization

#### 4.1 Update Optimization
```rust
struct UpdateOptimization {
    coalescing: CoalescingStrategy,
    batching: BatchingConfig,
    compression: CompressionMethod,
    priority_queue: PriorityQueue
}

struct CoalescingStrategy {
    spatial_threshold: float32,    // Minimum movement
    temporal_threshold: uint32,    // Minimum time
    attribute_threshold: float32   // Minimum change
}
```

#### 4.2 Resource Management
```rust
struct ResourceManager {
    bandwidth_limit: uint32,
    update_budget: UpdateBudget,
    cache_policy: CachePolicy,
    scaling_rules: ScalingRules
}
```

### 5. Privacy Controls

#### 5.1 Privacy Settings
```rust
struct PrivacySettings {
    visibility_control: VisibilityControl,
    location_precision: PrecisionControl,
    temporal_retention: RetentionPolicy,
    data_minimization: MinimizationPolicy
}

struct VisibilityControl {
    public: bool,
    whitelist: Option<Vec<uuid>>,
    blacklist: Option<Vec<uuid>>,
    context_rules: Vec<ContextRule>
}
```

#### 5.2 Anonymization
```rust
struct AnonymizationConfig {
    method: AnonymizationMethod,
    k_anonymity: uint16,
    noise_level: float32,
    cloaking_area: float32
}
```

### 6. State Management

#### 6.1 Presence State
```rust
struct PresenceState {
    entities: HashMap<uuid, PresenceEntity>,
    spatial_index: SpatialIndex,
    update_log: UpdateLog,
    metrics: PresenceMetrics
}
```

#### 6.2 State Synchronization
```rust
struct PresenceSync {
    sync_method: SyncMethod,
    consistency_level: ConsistencyLevel,
    conflict_resolution: ConflictStrategy,
    state_validation: ValidationMethod
}
```

### 7. Implementation Requirements

#### 7.1 Required Features
1. Basic presence broadcasting
2. Spatial discovery
3. Privacy controls
4. Resource management
5. State synchronization

#### 7.2 Optional Features
1. Advanced optimization
2. Custom anonymization
3. Predictive presence
4. Complex filtering

### 8. Usage Patterns

#### 8.1 Common Patterns
```python
# Initialize presence
async def initialize_presence():
    presence = PresenceEntity(
        entity_type=EntityType.MOBILE,
        broadcast_config=BroadcastConfig(
            radius=100,
            frequency=30
        )
    )
    await node.broadcast_presence(presence)

# Query nearby entities
async def discover_nearby():
    query = PresenceQuery(
        spatial_bounds=current_bounds,
        entity_filter=EntityFilter(
            types=[EntityType.MOBILE]
        )
    )
    results = await node.query_presence(query)
```

## Security Considerations

1. Privacy Protection
- Location privacy
- Activity privacy
- Identity protection

2. Broadcasting Security
- Spoofing prevention
- Replay protection
- Authorization

3. Resource Protection
- DoS prevention
- Bandwidth control
- CPU protection

## Implementation

### Example Implementation
```python
# Presence manager
class PresenceManager:
    async def broadcast(self, entity: PresenceEntity):
        config = self.optimize_broadcast(entity)
        await self.node.broadcast(
            PresenceBroadcast(
                entity=entity,
                config=config
            )
        )
    
    async def handle_discovery(self, query: PresenceQuery):
        results = await self.spatial_index.query(query)
        return self.filter_results(results, query.privacy)
```

## Copyright

This document is licensed under the MIT License.

---