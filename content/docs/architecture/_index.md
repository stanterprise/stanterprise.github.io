---
title: Architecture
weight: 3
description: Understanding Observer's architecture and components
---

# Observer Architecture

Observer is built on a modern, event-driven architecture designed for scalability, reliability, and real-time test monitoring. The system supports two deployment modes: All-in-One (AIO) for simplicity and Distributed Mode for production scalability.

## System Overview

```mermaid
graph TB
    subgraph "Test Execution"
        A[Playwright Tests]
        B[@stanterprise/playwright-reporter]
    end
    
    subgraph "Ingestion Layer"
        C[Ingestion Service<br/>gRPC Port 50051]
    end
    
    subgraph "Message Streaming"
        D[NATS JetStream<br/>Event Bus]
    end
    
    subgraph "Processing Layer"
        E[Processor Service<br/>Event Consumer]
    end
    
    subgraph "Storage Layer"
        F[(MongoDB<br/>Test Data)]
    end
    
    subgraph "API Layer"
        G[API Service<br/>REST/GraphQL/WebSocket]
    end
    
    subgraph "Presentation Layer"
        H[Web UI<br/>React Dashboard]
        I[WebSocket<br/>Real-Time Updates]
    end
    
    A --> B
    B -->|Test Events<br/>gRPC| C
    C -->|Publish| D
    D -->|Subscribe| E
    E -->|Persist| F
    F --> G
    D -.->|Stream| G
    G -->|HTTP/GraphQL| H
    G -.->|WebSocket| I
    I --> H
    
    style C fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
    style D fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
    style E fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
    style G fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
```

## Core Components

### 1. Playwright Reporter (@stanterprise/playwright-reporter)

The test client that integrates with Playwright test framework:

- **Purpose**: Captures test execution events and sends them to Observer
- **Protocol**: gRPC (protobuf)
- **Events**: Test begin/end, step begin/end, failures, attachments
- **Features**:
  - Fire-and-forget async reporting
  - Retry logic with exponential backoff
  - Attachment processing (screenshots, videos, traces)
  - Sharding support for parallel execution
  - Custom metadata injection via environment variables

**Configuration**:
```typescript
reporter: [
  ['@stanterprise/playwright-reporter', {
    grpcAddress: 'localhost:50051',
    grpcMaxRetries: 3,
    grpcRetryDelay: 100,
    maxAttachmentSize: 10485760
  }]
]
```

### 2. Ingestion Service

The entry point for all test events:

- **Purpose**: Receives and validates test events via gRPC
- **Port**: 50051 (default, configurable)
- **Protocol**: gRPC (protobuf-based)
- **Scalability**: Stateless, horizontally scalable
- **Features**:
  - High-throughput event ingestion
  - Payload validation
  - Publishes to NATS JetStream
  - Optional dual-write to database

**Key characteristics**:
- No database dependency (stateless)
- Can scale to handle thousands of concurrent test runs
- Validates protobuf payloads before publishing

**Environment Variables**:
- `PORT`: gRPC listening port (default: 50051)
- `NATS_URL`: NATS server URL
- `NATS_STREAM`: JetStream stream name (default: tests_events)
- `NATS_SUBJECT_PREFIX`: Subject prefix (default: tests.events.v1)

### 3. NATS JetStream

Message streaming platform for event distribution:

- **Purpose**: Decouples ingestion from processing
- **Features**:
  - At-least-once delivery guarantee
  - Message persistence
  - Consumer groups for load distribution
  - Stream replay capabilities
- **Benefits**:
  - Enables horizontal scaling
  - Provides fault tolerance
  - Allows multiple consumers (processor, WebSocket relay)

**Configuration**:
```yaml
stream: tests_events
subjects:
  - tests.events.v1.>
retention: workqueue
```

### 4. Processor Service

Event processor that persists test data:

- **Purpose**: Consumes events from NATS and persists to MongoDB
- **Pattern**: Durable consumer with idempotent upsert
- **Scalability**: Can run multiple instances with consumer groups
- **Features**:
  - Idempotent event processing
  - Database migration handling
  - Structured test run hierarchy
  - Automatic retry on failures

**Data Model**:
```
Test Run
├── Metadata (run ID, timestamp, shard info)
├── Tests[]
│   ├── Test ID, name, status
│   ├── Steps[]
│   │   └── Step ID, name, duration, status
│   └── Attachments[]
│       └── Type, path, content
└── Summary (counts, durations)
```

**Environment Variables**:
- `MONGODB_URI`: MongoDB connection string (required)
- `NATS_URL`: NATS server URL
- `NATS_STREAM`: JetStream stream name
- `NATS_CONSUMER`: Durable consumer name (default: processor)

### 5. API Service

REST/GraphQL API and WebSocket server:

- **Purpose**: Provides data access and real-time streaming
- **Port**: 8080 (default, configurable)
- **Protocols**: HTTP, GraphQL, WebSocket
- **Features**:
  - REST endpoints for test listing and details
  - GraphQL API with interactive playground
  - WebSocket endpoint for real-time event streaming
  - Read-only database access

**Endpoints**:
- `GET /api/test-runs` - List test runs
- `GET /api/test-runs/:id` - Get test run details
- `GET /api/test-runs/:id/stats` - Get run statistics
- `POST /graphql` - GraphQL queries
- `GET /graphql` - GraphQL playground
- `GET /ws` - WebSocket connection for real-time events

**WebSocket Events**:
```json
{
  "type": "test.begin|test.end|step.begin|step.end",
  "timestamp": "2026-02-17T03:42:54Z",
  "data": { /* event-specific data */ }
}
```

**Environment Variables**:
- `PORT`: HTTP listening port (default: 8080)
- `MONGODB_URI`: MongoDB connection string (required)
- `NATS_URL`: NATS server URL (optional, for WebSocket)
- `NATS_STREAM`: JetStream stream name
- `NATS_WS_CONSUMER`: WebSocket consumer name (default: websocket)

### 6. Web UI

Modern React-based dashboard:

- **Purpose**: Visualize test runs and monitor execution
- **Technology**: React, TypeScript, Tailwind CSS
- **Port**: 3000 (development), 80 (production/Nginx)
- **Features**:
  - Real-time test execution monitoring
  - Test run listing with status and timing
  - Responsive, mobile-friendly design
  - Environment-based configuration

**Key Views**:
- Test run list with filters
- Test run details with step breakdown
- Real-time execution status
- Failure analysis

## Data Flow

### Test Execution Flow

1. **Test Start**: Playwright test begins execution
2. **Event Capture**: Reporter captures test.begin event
3. **gRPC Send**: Event sent to Ingestion Service via gRPC
4. **Validation**: Ingestion validates protobuf payload
5. **Publish**: Event published to NATS JetStream
6. **Process**: Processor consumes event from NATS
7. **Persist**: Processor saves to MongoDB
8. **Stream**: API service relays event via WebSocket
9. **Display**: Web UI receives WebSocket event and updates UI

### Query Flow

1. **User Request**: User opens Web UI or makes API call
2. **API Call**: Web UI queries API Service
3. **Database Query**: API Service queries MongoDB
4. **Response**: Data returned to Web UI
5. **Render**: UI displays test run information

## Deployment Modes

### All-in-One (AIO) Mode

Single container with all services embedded:

**Use Cases**:
- Local development
- CI/CD environments
- Quick demos and testing

**Container**:
```bash
docker run -d \
  -p 3000:80 \
  -p 50051:50051 \
  -v observer-data:/data \
  ghcr.io/stanterprise/observer/aio:latest
```

**Includes**:
- Ingestion service
- NATS JetStream (embedded)
- Processor service
- API service
- MongoDB (embedded)
- Web UI (Nginx)

### Distributed Mode

Separate containers for each service:

**Use Cases**:
- Production deployments
- High-scale test environments
- Multi-tenant setups

**Services**:
- `observer-ingestion`: gRPC ingestion service
- `observer-processor`: Event processor
- `observer-api`: REST/GraphQL/WebSocket API
- `observer-web`: React UI (Nginx)
- `mongodb`: Database (external)
- `nats`: Message broker (external)

**Deployment**:
```bash
# Via Helm
helm install observer oci://ghcr.io/stanterprise/observer/charts/observer

# Via Docker Compose
docker compose --profile dist up -d
```

## Scalability

Observer scales horizontally at every layer:

### Ingestion Layer
- **Stateless**: No local state, can run unlimited replicas
- **Load Balancing**: Use load balancer or Kubernetes service
- **Throughput**: Thousands of concurrent connections

### Processing Layer
- **Consumer Groups**: Multiple processor instances share workload
- **Partitioning**: NATS distributes messages across consumers
- **Idempotency**: Safe to process same event multiple times

### Storage Layer
- **MongoDB**: Horizontal scaling via sharding
- **Indexing**: Optimized indexes for common queries
- **Retention**: Configurable data retention policies

### API Layer
- **Stateless**: Multiple API instances behind load balancer
- **Caching**: Query result caching for performance
- **WebSocket**: Each connection handled independently

## Performance Characteristics

- **Ingestion**: 10,000+ events/second per ingestion node
- **Processing**: 5,000+ events/second per processor node
- **Query Latency**: <100ms for recent test runs
- **WebSocket**: Real-time event delivery (<50ms latency)
- **Storage**: Efficient document-based storage for test hierarchies

## Technology Stack

- **Language**: Go (services), TypeScript (reporter, Web UI)
- **Messaging**: NATS JetStream
- **Database**: MongoDB
- **API**: REST, GraphQL (gqlgen)
- **Frontend**: React, TypeScript, Tailwind CSS
- **Deployment**: Docker, Kubernetes (Helm)
- **Protocol**: gRPC (protobuf) for ingestion, HTTP/WebSocket for API

## Security Considerations

- **gRPC**: TLS support for encrypted communication
- **Authentication**: Token-based authentication (roadmap)
- **Network**: Ingestion and API can be isolated
- **Database**: Connection encryption and auth
- **NATS**: TLS and token authentication support

## High Availability

### Data Durability
- **NATS JetStream**: Persistent message storage
- **MongoDB**: Replica sets for redundancy
- **Idempotency**: Safe event replay on failure

### Fault Tolerance
- **Service Restarts**: Automatic recovery from crashes
- **Message Replay**: Reprocess missed events
- **Graceful Degradation**: Continue operation with reduced functionality

## Future Enhancements

- [ ] Remove database from ingestion (fully stateless)
- [ ] Complete GraphQL API implementation
- [ ] Object storage for large attachments (S3/MinIO)
- [ ] Authentication and authorization layer
- [ ] Metrics export (Prometheus)
- [ ] Distributed tracing (OpenTelemetry)

## Next Steps

- [Getting Started](/docs/getting-started/) - Set up Observer
- [Installation](/docs/install/) - Deployment guides
- [Playwright Reporter](/docs/integrations/playwright-reporter/) - Configure the reporter
- [Demo](/docs/demo/) - Try Observer with examples
