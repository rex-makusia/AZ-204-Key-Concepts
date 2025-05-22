# AZ-204 Key Concepts Summary - Quick Study Reference

## 1. Azure Compute Solutions (25-30%)

### Container Solutions

**Docker & Container Images**
- **Dockerfile**: Blueprint for container images (FROM, RUN, COPY, EXPOSE, CMD)
- **Multi-stage builds**: Reduce image size, separate build/runtime environments
- **Base images**: Use official, minimal images (alpine, distroless)

**Azure Container Registry (ACR)**
- **Authentication**: Admin user, service principal, managed identity, Azure CLI
- **Commands**: `az acr build`, `docker push`, `az acr import`
- **ACR Tasks**: Automated builds on code commit, base image updates
- **Geo-replication**: Deploy images closer to regions

**Azure Container Instances (ACI)**
- **Use cases**: Burst scaling, isolated workloads, CI/CD agents
- **Networking**: Virtual network integration, public/private IP
- **Storage**: Azure Files for persistent volumes
- **Resource limits**: CPU (0.1-4 cores), Memory (0.1-16 GB per core)

**Azure Container Apps**
- **Environment**: Shared boundary for apps, manages infrastructure
- **Revisions**: Immutable snapshots, traffic splitting (blue-green)
- **Scaling**: HTTP requests, CPU/Memory, custom metrics, scale-to-zero
- **DAPR**: Service-to-service communication, state management, pub/sub

### App Service Web Apps

**Core Concepts**
- **App Service Plan**: Defines compute resources, pricing tier affects features
- **Deployment slots**: Staging environments, swap with production
- **Runtime stacks**: .NET, Node.js, Python, Java, PHP - each with version options

**Configuration**
- **Application settings**: Environment variables, connection strings
- **Custom domains**: CNAME/A records, SSL certificates (App Service Managed, Custom)
- **CORS**: Cross-origin resource sharing configuration
- **Authentication**: Built-in auth providers (Azure AD, Google, Facebook)

**Scaling & Performance**
- **Scale up**: Increase instance size (CPU, RAM)
- **Scale out**: Increase instance count (manual, auto, scheduled)
- **Autoscale rules**: CPU, memory, HTTP queue length, custom metrics
- **Health checks**: Automatic unhealthy instance replacement

**Monitoring & Diagnostics**
- **Application logs**: Application-generated logs
- **Web server logs**: IIS logs, failed request traces
- **Log streaming**: Real-time log viewing
- **Kudu**: Advanced deployment and diagnostic tools

### Azure Functions

**Function App Fundamentals**
- **Hosting plans**: Consumption (pay-per-execution), Premium (pre-warmed), Dedicated (App Service plan)
- **Runtime versions**: In-process vs isolated, language-specific considerations
- **Cold start**: Delay when function hasn't run recently (mitigated by Premium plan)

**Triggers & Bindings**
- **HTTP trigger**: RESTful APIs, webhook endpoints
- **Timer trigger**: CRON expressions (`0 0 12 * * *` = daily at noon)
- **Blob trigger**: Responds to blob creation/updates
- **Queue trigger**: Process messages from Storage Queue/Service Bus
- **Event Grid trigger**: React to Azure events

**Key Binding Types**
- **Input bindings**: Cosmos DB, Blob Storage, Table Storage
- **Output bindings**: Multiple outputs, return values
- **Connection strings**: Stored in application settings, use managed identity when possible

## 2. Azure Storage (15-20%)

### Azure Cosmos DB

**Core Concepts**
- **Multi-model**: Document (SQL API), Key-Value, Graph, Column-family
- **Global distribution**: Multi-region writes, automatic failover
- **Partitioning**: Partition key selection critical for performance

**Consistency Levels**
- **Strong**: Linearizability, highest consistency, highest latency
- **Bounded Staleness**: Configurable lag (time/operations)
- **Session**: Default, consistent within client session
- **Consistent Prefix**: Reads never see out-of-order writes
- **Eventual**: Lowest latency, eventual consistency

**SDK Operations**
- **Client hierarchy**: CosmosClient → Database → Container → Item
- **CRUD operations**: CreateItemAsync, ReadItemAsync, UpsertItemAsync, DeleteItemAsync
- **Queries**: SQL-like syntax, parameterized queries
- **Bulk operations**: Better performance for large datasets

**Change Feed**
- **Real-time processing**: React to data changes
- **Change feed processor**: Distributed, scalable, fault-tolerant
- **Use cases**: Real-time analytics, event sourcing, cache invalidation

### Azure Blob Storage

**Blob Types**
- **Block blobs**: Text/binary data, up to 4.77 TB
- **Append blobs**: Logging scenarios, append-only
- **Page blobs**: VHD files, random read/write access

**Access Tiers**
- **Hot**: Frequently accessed, highest storage cost, lowest access cost
- **Cool**: 30+ days, lower storage cost, higher access cost
- **Archive**: 180+ days, lowest storage cost, highest access/retrieval cost

**Lifecycle Management**
- **Policies**: Automatic tier transitions based on age
- **Delete policies**: Clean up old data automatically
- **Conditions**: Last modified time, creation time, access time

**Key Operations**
- **Upload**: UploadAsync, OpenWrite (streaming)
- **Download**: DownloadToAsync, OpenRead (streaming)
- **Metadata**: Custom key-value pairs, system properties
- **Leasing**: Exclusive write access, prevent concurrent modifications

## 3. Azure Security (15-20%)

### Microsoft Identity Platform

**Authentication Flows**
- **Authorization Code Flow**: Web apps, SPAs with PKCE
- **Client Credentials Flow**: Service-to-service, daemon apps
- **Device Code Flow**: IoT devices, limited input devices

**MSAL (Microsoft Authentication Library)**
- **Token acquisition**: AcquireTokenSilent (cache), AcquireTokenInteractive
- **Token cache**: Automatic token refresh, persistent cache options
- **Scopes**: Incremental consent, least privilege principle

**Key Concepts**
- **App registrations**: Client ID, redirect URIs, certificates/secrets
- **Permissions**: Application (app-only) vs Delegated (on behalf of user)
- **Consent**: Admin consent for sensitive permissions

### Shared Access Signatures (SAS)

**SAS Types**
- **Account SAS**: Multiple services, resource types
- **Service SAS**: Single service (Blob, Queue, Table, File)
- **User delegation SAS**: Secured with Azure AD credentials

**SAS Components**
- **Permissions**: Read (r), Write (w), Delete (d), List (l), Add (a), Create (c), Update (u), Process (p)
- **Expiry time**: Always set, short-lived preferred
- **IP addresses**: Restrict access to specific IP ranges
- **Protocols**: HTTPS only recommended

### Azure Key Vault

**Secret Management**
- **Secrets**: Connection strings, passwords, API keys
- **Keys**: Encryption keys, HSM-backed or software-protected
- **Certificates**: SSL/TLS certificates, automatic renewal

**Access Patterns**
- **SDK**: KeyVaultSecret, KeyVaultKey, KeyVaultCertificate clients
- **Managed Identity**: Preferred authentication method
- **Caching**: Cache secrets to reduce Key Vault calls

### Managed Identities

**Types**
- **System-assigned**: Lifecycle tied to resource, automatic cleanup
- **User-assigned**: Independent lifecycle, can be shared across resources

**Implementation**
- **Token acquisition**: Azure Instance Metadata Service (IMDS)
- **SDK integration**: DefaultAzureCredential automatically uses managed identity
- **RBAC**: Assign appropriate roles (Reader, Contributor, custom roles)

## 4. Monitoring & Troubleshooting (5-10%)

### Application Insights

**Telemetry Types**
- **Requests**: HTTP requests, response times, success rates
- **Dependencies**: Database calls, HTTP calls, external services
- **Exceptions**: Unhandled exceptions, custom exception tracking
- **Custom events**: Business logic events, user actions

**Key Features**
- **Live Metrics**: Real-time performance monitoring
- **Application Map**: Visualize dependencies and bottlenecks
- **Smart Detection**: AI-powered anomaly detection
- **Availability tests**: Synthetic monitoring from multiple locations

**Implementation**
- **Auto-instrumentation**: Minimal code changes, automatic dependency tracking
- **Custom telemetry**: TrackEvent, TrackMetric, TrackException
- **Sampling**: Reduce telemetry volume, maintain statistical accuracy

## 5. Azure Services Integration (20-25%)

### API Management

**Core Components**
- **APIs**: Group related operations, version management
- **Products**: Package APIs for different audiences
- **Subscriptions**: Access control, usage tracking
- **Policies**: Request/response transformation, rate limiting

**Key Policies**
- **Authentication**: validate-jwt, authentication-basic
- **Rate limiting**: rate-limit, quota
- **Transformation**: set-header, set-body, json-to-xml
- **Backend**: set-backend-service, retry

### Event-Based Solutions

**Azure Event Grid**
- **Publishers**: ARM, Storage, Service Bus, Custom applications
- **Event types**: System events (resource changes) vs custom events
- **Subscriptions**: Filter events by subject, event type
- **Handlers**: Functions, Logic Apps, WebHooks, Storage Queues

**Azure Event Hubs**
- **Partitions**: Parallel processing, ordered within partition
- **Consumer groups**: Multiple independent consumers
- **Capture**: Archive events to Blob Storage/Data Lake
- **Throughput units**: Scale ingestion capacity

### Message-Based Solutions

**Azure Service Bus**
- **Queues**: Point-to-point communication, FIFO with sessions
- **Topics/Subscriptions**: Publish-subscribe pattern, message filtering
- **Features**: Dead letter queue, duplicate detection, scheduled delivery
- **Sessions**: Message ordering, state management

**Azure Storage Queues**
- **Simple**: Basic queue operations, HTTP-based
- **Visibility timeout**: Hide messages during processing
- **Poison messages**: Handle repeatedly failed messages
- **Scale**: Up to 64KB message size, unlimited queue size

### Microsoft Graph

**Common Operations**
- **Users**: /users, /me (current user)
- **Groups**: /groups, group membership
- **Mail**: /me/messages, send mail
- **Calendar**: /me/events, calendar operations
- **Files**: /me/drive, OneDrive operations

**Best Practices**
- **Batch requests**: Combine multiple requests, reduce latency
- **Pagination**: Handle large result sets with @odata.nextLink
- **Throttling**: Implement retry logic with exponential backoff
- **Permissions**: Use least privilege, prefer delegated over application permissions

## Quick Memory Aids

**Consistency Levels (Cosmos DB)**: **S**trong → **B**ounded → **S**ession → **C**onsistent → **E**ventual
**(S)trongest (B)ut (S)lower, (C)heap (E)ventually**

**SAS Permissions**: **RWDLACU+P**
**(R)ead (W)rite (D)elete (L)ist (A)dd (C)reate (U)pdate + (P)rocess**

**App Service Scaling**: **Up** = bigger instances, **Out** = more instances

**Function Hosting**: **C**onsumption (cheapest), **P**remium (performance), **D**edicated (control)

**Storage Tiers**: **H**ot (expensive storage, cheap access), **C**ool (cheaper storage), **A**rchive (cheapest storage, expensive access)

## Common Gotchas

- **Cosmos DB partition key**: Cannot be changed after creation, choose wisely
- **Function cold starts**: Use Premium plan for production workloads
- **SAS tokens**: Always use HTTPS, set expiry times
- **Managed Identity**: Preferred over connection strings/keys
- **App Service slots**: Sticky settings don't swap automatically
- **Event Grid vs Event Hubs**: Grid for events, Hubs for telemetry/streaming
- **Service Bus vs Storage Queue**: Service Bus for enterprise messaging, Storage Queue for simple scenarios
