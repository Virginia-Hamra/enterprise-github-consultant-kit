# Architecture Analysis Skill

## Purpose
Comprehensive analysis of software architecture, system design patterns, and technical decisions for enterprise GitHub implementations. This skill supports **Solutions Architects** and **Technical Leads** in designing scalable, secure, and maintainable GitHub Enterprise platforms.

## Role Context
Use this skill when designing enterprise GitHub organization structures, assessing application architectures for cloud-native migrations, or evaluating technical decisions for large-scale platform implementations.

## Capabilities

### Enterprise GitHub Architecture
- **Organization Structure Design**: Multi-org vs single-org strategies for large enterprises
- **Repository Topology**: Monorepo vs polyrepo analysis for different business contexts
- **Access Patterns**: RBAC design, team hierarchy, and permission boundaries
- **Federation Strategy**: Cross-organization collaboration and dependency management
- **Compliance Boundaries**: Data residency, regulatory segregation, and tenant isolation

### Application Architecture Assessment
- **Microservices vs Monolith**: Evaluate architectural patterns for GitHub-hosted applications
- **API Design**: RESTful, GraphQL, event-driven architecture analysis
- **Data Architecture**: Database strategy, caching layers, and data flow patterns
- **Integration Patterns**: Service mesh, API gateways, and inter-service communication
- **Scalability Assessment**: Horizontal vs vertical scaling, load distribution strategies

### Cloud-Native Architecture
- **Kubernetes Architecture**: Namespace design, RBAC, network policies
- **Serverless Patterns**: Function-as-a-Service, event-driven workflows
- **Container Strategy**: Image management, registry patterns, multi-stage builds
- **Service Discovery**: DNS, service mesh, and load balancing approaches
- **Observability Architecture**: Logging, metrics, tracing, and alerting infrastructure

### Security Architecture
- **Zero Trust Design**: Network segmentation, identity-based access, least privilege
- **Secrets Management**: Vault integration, OIDC federation, credential lifecycle
- **Supply Chain Security**: SBOM generation, artifact signing, provenance tracking
- **Network Architecture**: VPN, private endpoints, egress control, firewall rules
- **Identity Federation**: SAML, OIDC, SCIM provisioning architecture

### Data Flow & Dependencies
- **Dependency Mapping**: Service dependencies, library dependencies, infrastructure dependencies
- **Data Lineage**: Track data sources, transformations, and destinations
- **Integration Points**: External systems, APIs, message queues, event buses
- **Failure Modes**: Analyze cascade failures, circuit breakers, fallback strategies
- **Change Impact**: Assess downstream effects of architectural changes

### Technical Debt & Modernization
- **Anti-Pattern Detection**: Identify code smells, architectural violations, outdated patterns
- **Modernization Roadmap**: Plan incremental refactoring and technology upgrades
- **Migration Strategy**: Strangler fig, big bang, or hybrid migration approaches
- **Risk Assessment**: Quantify technical debt impact on velocity and quality
- **Cost-Benefit Analysis**: Justify modernization investment with business outcomes

## Usage Scenarios

### Scenario 1: Enterprise Org Structure Design
**Objective**: Design GitHub Enterprise organization structure for a Fortune 500 company

```bash
# Analyze existing repository distribution
./scripts/analyze-repo-distribution.sh <enterprise-slug>

# Generate organization structure recommendations
./scripts/recommend-org-structure.sh --business-units 15 --teams 200 --repos 5000
```

**Design Considerations**:
- Business unit autonomy vs. platform standardization
- Shared services vs. federated ownership
- Compliance and data residency requirements
- Cross-org collaboration needs
- Billing and cost allocation strategy

**Output**: ADR (Architecture Decision Record) with recommended structure and rationale

### Scenario 2: Monorepo vs Polyrepo Analysis
**Objective**: Determine optimal repository strategy for a product suite

```bash
# Analyze current codebase characteristics
./scripts/analyze-codebase-coupling.sh /path/to/codebase

# Generate repository strategy recommendations
./scripts/recommend-repo-strategy.sh --analysis-report coupling-report.json
```

**Evaluation Factors**:
- Code coupling and dependency density
- Team structure and ownership boundaries
- Build and test performance requirements
- Deployment independence needs
- Code sharing and reuse patterns

**Output**: Repository strategy decision with migration plan

### Scenario 3: Microservices Architecture Review
**Objective**: Assess microservices architecture for anti-patterns and optimization opportunities

```bash
# Discover service dependencies
./scripts/map-service-dependencies.sh <org/repos>

# Analyze service boundaries
./scripts/analyze-service-boundaries.sh --dependency-graph deps.json

# Check for anti-patterns
./scripts/detect-architecture-antipatterns.sh --services ./services/
```

**Review Areas**:
- Service granularity (too coarse or too fine)
- Inter-service communication patterns
- Data ownership and consistency boundaries
- Deployment coupling and versioning strategy
- Observability and debugging complexity

**Output**: Architecture assessment report with refactoring recommendations

### Scenario 4: Cloud Migration Architecture
**Objective**: Design cloud-native architecture for legacy application migration

```bash
# Assess application cloud readiness
./scripts/assess-cloud-readiness.sh <app-repo>

# Generate target architecture
./scripts/design-cloud-architecture.sh --source-analysis readiness.json --cloud aws
```

**Assessment Areas**:
- 12-factor app compliance
- Stateful vs stateless components
- Persistence and storage requirements
- Network and security boundaries
- Observability and operations

**Output**: Target architecture diagrams and migration strategy

## Scripts

### `scripts/analyze-repo-distribution.sh`
Analyzes current repository distribution across organizations and teams.

**Usage**:
```bash
./analyze-repo-distribution.sh <enterprise-slug> [--output json|csv]
```

**Output**: Repository counts by org, team, visibility, activity level

### `scripts/recommend-org-structure.sh`
Generates GitHub organization structure recommendations.

**Usage**:
```bash
./recommend-org-structure.sh --business-units N --teams N --repos N [--compliance-boundary]
```

**Considers**:
- Business unit structure
- Compliance requirements
- Cost allocation needs
- Collaboration patterns

### `scripts/analyze-codebase-coupling.sh`
Analyzes code coupling and dependency relationships.

**Usage**:
```bash
./analyze-codebase-coupling.sh <path> [--language auto|java|python|typescript]
```

**Output**: Coupling matrix, dependency graph, modularity score

### `scripts/map-service-dependencies.sh`
Discovers and maps microservices dependencies.

**Usage**:
```bash
./map-service-dependencies.sh <org/repos> [--include-external]
```

**Requires**: `gh` CLI with read access to repositories

**Output**: Service dependency graph (JSON, DOT format)

### `scripts/detect-architecture-antipatterns.sh`
Scans codebase for common architectural anti-patterns.

**Usage**:
```bash
./detect-architecture-antipatterns.sh --services <path> [--rules custom-rules.yaml]
```

**Detects**:
- Circular dependencies
- God objects and classes
- Inappropriate intimacy between services
- Shotgun surgery (widespread changes for single feature)
- Data clumps and primitive obsession

### `scripts/assess-cloud-readiness.sh`
Evaluates application readiness for cloud migration.

**Usage**:
```bash
./assess-cloud-readiness.sh <app-repo> [--12factor]
```

**Checks**:
- Configuration externalization
- Stateless process design
- Port binding
- Concurrency model
- Disposability and startup time
- Dev/prod parity

### `scripts/design-cloud-architecture.sh`
Generates target cloud architecture recommendations.

**Usage**:
```bash
./design-cloud-architecture.sh --source-analysis <report> --cloud <aws|azure|gcp>
```

**Output**: Architecture diagrams, IaC templates, migration checklist

## Architecture Decision Records (ADRs)

When making significant architectural decisions, create ADRs using this template:

```markdown
# ADR-XXX: [Decision Title]

## Status
[Proposed | Accepted | Deprecated | Superseded]

## Context
[What is the issue we're trying to solve? What constraints exist?]

## Decision
[What is the change we're proposing/have agreed to?]

## Consequences
[What becomes easier or harder because of this change?]

## Alternatives Considered
[What other options did we evaluate?]

## References
[Links to related documents, discussions, or research]
```

Store ADRs in the `.github/architecture-decisions/` directory of the customer repository.

## Deliverables Template

When completing an architecture analysis, provide:

1. **Executive Summary**
   - Current state overview
   - Key findings and risks
   - Recommended approach and rationale

2. **Architecture Diagrams**
   - Current state (C4 model: Context, Container, Component)
   - Target state architecture
   - Migration transition states
   - Data flow diagrams
   - Network architecture diagrams

3. **Decision Records**
   - ADRs for all major decisions
   - Trade-offs and justification
   - Risk mitigation strategies

4. **Implementation Roadmap**
   - Phased implementation plan
   - Dependencies and prerequisites
   - Resource requirements
   - Timeline estimates (avoid specific dates)

5. **Quality Attributes**
   - Performance requirements and targets
   - Scalability projections
   - Security posture
   - Availability and disaster recovery
   - Cost projections

6. **Reference Implementation**
   - Proof-of-concept code examples
   - IaC templates
   - Configuration samples
   - Testing strategies

## Best Practices

### When Designing Organization Structures
- Balance autonomy with governance
- Plan for growth (don't over-optimize for current state)
- Consider billing and cost attribution early
- Design for cross-org collaboration patterns
- Establish clear ownership boundaries

### When Evaluating Architecture Patterns
- Avoid "resume-driven development" (choosing tech for learning, not fit)
- Consider operational complexity, not just development elegance
- Evaluate team skill sets and learning curves
- Assess vendor lock-in and exit costs
- Plan for observability and debugging from day one

### When Modernizing Legacy Systems
- Use strangler fig pattern for incremental migration
- Establish anti-corruption layers at boundaries
- Prioritize high-risk or high-value components first
- Maintain rollback capabilities
- Measure business impact, not just technical metrics

### When Creating Architecture Diagrams
- Use consistent notation (C4, UML, or customer standard)
- Label all components, connections, and boundaries
- Show security zones and trust boundaries
- Include data flow direction and protocols
- Version and date all diagrams

## Integration with Other Skills

- **DevOps Review**: Architecture decisions inform CI/CD design
- **Security Audit**: Security architecture shapes compliance approach
- **Migration Planner**: Architecture determines migration complexity
- **Governance Enforcer**: Org structure drives policy design

## Customer Engagement Tips

### Discovery Questions
- "What are your key scalability concerns?"
- "How do you currently handle multi-tenancy?"
- "What compliance requirements constrain your architecture?"
- "How do teams currently collaborate across organizational boundaries?"
- "What technical debt is slowing you down the most?"

### Communication Strategies
- Use visual diagrams heavily (architects think visually)
- Relate technical decisions to business outcomes
- Acknowledge trade-offs explicitly (no perfect solutions)
- Provide multiple options with pros/cons
- Get early feedback on direction before deep work

### Red Flags to Watch For
- "We need microservices" without clear justification
- Over-engineering for hypothetical future requirements
- Ignoring operational complexity in design
- No clear ownership or accountability model
- Architecture by committee with no technical leader

## Reference Materials

- [C4 Model for Architecture Diagrams](https://c4model.com/)
- [Architecture Decision Records](https://adr.github.io/)
- [12-Factor App Methodology](https://12factor.net/)
- [GitHub Enterprise Architecture Patterns](https://docs.github.com/en/enterprise-cloud@latest)
- [Cloud Architecture Center (AWS/Azure/GCP)]()
