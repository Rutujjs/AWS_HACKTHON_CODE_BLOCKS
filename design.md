# Design Document: Adaptive Healthcare Support System for Autism Spectrum Disorder

## Overview

The Adaptive Healthcare Support System for Autism Spectrum Disorder (ASD) is a clinical decision-support platform that provides personalized, evidence-based interventions for individuals with ASD across six core domains: Communication, Social Interaction, Sensory Processing, Routine & Behavioral Rigidity, Emotional Regulation, and Adaptive & Physical Functioning.

## Production Architecture

### 1. Architectural Overview

The system follows a layered microservice architecture with clear separation of concerns:

**Architecture Layers**:
1. **Frontend Layer** - User-facing web and mobile applications
2. **API Gateway** - Unified GraphQL entry point with Apollo Gateway
3. **Backend Services** - Domain-driven microservices (Node.js + NestJS)
4. **AI & Intelligence Engine** - ML-powered assessment and recommendation engine (Python + FastAPI)
5. **Data Layer** - Persistent storage with PostgreSQL, Redis, and S3
6. **Reporting Module** - Clinical report generation and visualization
7. **Real-Time Communication** - WebSocket-based notifications and updates
8. **DevOps Infrastructure** - Containerized deployment with Kubernetes

### 2. Technology Stack

#### Frontend Layer
- **Web Application**: React.js + TypeScript
  - **Styling**: Tailwind CSS for utility-first responsive design
  - **Animations**: Framer Motion for smooth transitions, Lottie for complex animations
  - **State Management**: Apollo Client for GraphQL state management
  - **Visualization**: Chart.js for trend graphs and analytics
- **Mobile Application**: React Native + Expo
  - Cross-platform iOS and Android support
  - Native push notifications
  - Offline-first architecture for assessment continuity

**Rationale**: Type safety with TypeScript ensures robust code, modular component architecture supports cognitive-load-sensitive UI design, and consistent codebase across web and mobile reduces development overhead.

#### API Gateway Layer
- **GraphQL**: Apollo Gateway with schema federation
  - Unified API entry point for all client requests
  - Flexible client-driven queries reduce over-fetching
  - Real-time subscriptions for live updates
  - Query complexity analysis and rate limiting
  - Automatic persisted queries for performance

**Rationale**: GraphQL enables clients to request exactly the data they need, reducing bandwidth and improving performance for users with varying connectivity. Schema federation allows independent service evolution.

#### Backend Services Layer
- **Framework**: Node.js + NestJS
  - Modular, enterprise-grade architecture
  - Dependency injection for testability
  - Built-in support for microservices patterns
  - Decorators for clean, declarative code
- **Authentication**: JWT + OAuth 2.0
  - Stateless authentication with JWT tokens
  - OAuth 2.0 for third-party provider integration
  - Refresh token rotation for security
- **Authorization**: Role-Based Access Control (RBAC)
  - Fine-grained permissions for users, caregivers, providers
  - Attribute-based access control for privacy settings
  - Policy-based authorization for complex rules

**Microservices**:
1. **User Service** - Profile management, authentication, preferences
2. **Assessment Service** - Quiz administration, scoring, scheduling
3. **Persona Service** - Support level determination, profile generation
4. **Recommendation Service** - Evidence-based intervention generation
5. **Report Service** - Clinical and caregiver report generation
6. **Notification Service** - Multi-channel alerts and reminders
7. **Consultation Service** - Optional video consultation integration

**Rationale**: NestJS provides structure and scalability for complex healthcare applications. Microservices enable independent deployment and scaling of high-traffic components.

#### AI & Intelligence Layer
- **Framework**: Python + FastAPI
  - High-performance async API framework
  - Automatic OpenAPI documentation
  - Native async/await support for ML inference
- **Machine Learning**: 
  - **Scikit-learn**: Domain scoring algorithms, support level classification
  - **PyTorch**: Deep learning models for pattern recognition (if required)
  - **Pandas/NumPy**: Data processing and feature engineering
- **Explainable AI**: SHAP (SHapley Additive exPlanations)
  - Feature importance analysis for domain scores
  - Transparent decision-making for support level determination
  - Clinician-readable explanations for recommendations
  - Model interpretation for bias detection

**AI Services**:
1. **Assessment Scoring Engine** - Domain score calculation with validated algorithms
2. **Support Level Classifier** - Multi-factor support level determination
3. **Persona Generator** - Sensory profile and behavioral pattern analysis
4. **Recommendation Engine** - Evidence-based intervention matching
5. **Trend Analyzer** - Longitudinal pattern detection and forecasting
6. **Explainability Service** - SHAP-based model interpretation

**Rationale**: Python ecosystem provides mature ML libraries. FastAPI offers performance comparable to Node.js while maintaining Python's ML advantages. SHAP ensures clinical transparency and trust.

#### Data Layer
- **Primary Database**: PostgreSQL
  - ACID compliance for healthcare data integrity
  - JSONB support for flexible schema evolution
  - Full-text search for clinical notes
  - Time-series optimization for longitudinal data
  - Row-level security for multi-tenancy
- **Caching**: Redis
  - Session storage for authentication
  - Query result caching for performance
  - Rate limiting and throttling
  - Pub/Sub for real-time notifications
  - Leaderboard for gamification (child mode)
- **Object Storage**: Amazon S3
  - Clinical report PDFs
  - Visual supports and social stories
  - Assessment media files
  - Backup and archival storage
  - Lifecycle policies for compliance

**Rationale**: PostgreSQL provides reliability and HIPAA-compliant data handling. Redis enables sub-millisecond response times for cached data. S3 offers scalable, durable storage for large files.

#### Reporting & Visualization Layer
- **PDF Generation**: Puppeteer / PDFKit
  - Clinical-style report formatting
  - Dynamic chart rendering
  - Multi-page layouts with headers/footers
  - Accessibility-compliant PDFs
- **Data Visualization**: Chart.js
  - Interactive trend graphs
  - Domain score timelines
  - Meltdown frequency charts
  - Adherence metrics dashboards

**Rationale**: Puppeteer enables pixel-perfect PDF generation from HTML templates. Chart.js provides accessible, responsive visualizations.

#### Real-Time Communication Layer
- **WebSocket**: Socket.io
  - Real-time notifications for caregivers
  - Live assessment progress updates
  - Instant alerts for critical domain scores
  - Presence indicators for consultation
- **Notification Service**: 
  - Multi-channel delivery (email, SMS, push, in-app)
  - Template-based messaging
  - Delivery tracking and retry logic
  - Quiet hours and preference management
- **Video Consultation** (Optional Module):
  - WebRTC for peer-to-peer video
  - Recording and transcription
  - Screen sharing for visual supports
  - Integration with provider scheduling

**Rationale**: WebSocket enables instant communication for time-sensitive alerts. Multi-channel notifications ensure caregivers receive critical updates.

#### DevOps & Infrastructure Layer
- **Containerization**: Docker
  - Consistent environments across dev/staging/prod
  - Isolated service dependencies
  - Efficient resource utilization
  - Multi-stage builds for optimized images
- **Orchestration**: Kubernetes
  - Automated deployment and scaling
  - Self-healing and health checks
  - Rolling updates with zero downtime
  - Resource limits and quotas
  - Namespace isolation for environments
- **Cloud Deployment**: AWS / GCP / Azure
  - Multi-region deployment for availability
  - Managed database services (RDS, Cloud SQL)
  - CDN for static assets (CloudFront, Cloud CDN)
  - Load balancing and auto-scaling
  - VPC isolation for security
- **CI/CD**: GitHub Actions / GitLab CI
  - Automated testing on pull requests
  - Continuous deployment to staging
  - Manual approval for production
  - Rollback capabilities
  - Secrets management
- **Monitoring**: Prometheus + Grafana
  - Service health metrics
  - API latency and throughput
  - Database performance
  - Custom business metrics (assessment completion rate)
  - Alerting for anomalies
- **Error Tracking**: Sentry
  - Real-time error reporting
  - Stack trace analysis
  - User context for debugging
  - Release tracking
  - Performance monitoring

**Rationale**: Kubernetes provides production-grade orchestration. Cloud-native services reduce operational overhead. Comprehensive monitoring ensures reliability and performance.

### 3. System Architecture Flow

```
User Interaction
       ↓
Assessment Submission
       ↓
Domain Scoring (AI Engine)
       ↓
Support Level Determination
       ↓
Persona Generation (Sensory Profile, Communication Preferences)
       ↓
Recommendation Engine (Evidence-Based Interventions)
       ↓
Adaptive UI Rendering (Age-Appropriate, Sensory-Friendly)
       ↓
Weekly Reassessment Trigger
       ↓
Report Generation (Provider & Caregiver)
       ↓
Feature Reconfiguration (Based on Support Level Changes)
```

### 4. Data Flow Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Client Applications                       │
│  ┌──────────────────┐         ┌──────────────────┐         │
│  │   Web App        │         │   Mobile App     │         │
│  │  (React + TS)    │         │  (React Native)  │         │
│  └────────┬─────────┘         └────────┬─────────┘         │
└───────────┼──────────────────────────────┼──────────────────┘
            │                              │
            │         HTTPS / WSS          │
            └──────────────┬───────────────┘
                           │
┌──────────────────────────▼───────────────────────────────────┐
│                   Apollo GraphQL Gateway                      │
│  • Schema Federation                                          │
│  • Authentication (JWT Validation)                            │
│  • Rate Limiting                                              │
│  • Query Complexity Analysis                                  │
└──────────────────────────┬───────────────────────────────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
┌───────▼────────┐ ┌───────▼────────┐ ┌──────▼───────┐
│ User Service   │ │ Assessment     │ │ Notification │
│                │ │ Service        │ │ Service      │
│ • Profile Mgmt │ │ • Quiz Admin   │ │ • Alerts     │
│ • Auth         │ │ • Scheduling   │ │ • Reminders  │
└───────┬────────┘ └───────┬────────┘ └──────┬───────┘
        │                  │                  │
        │                  │                  │
        └──────────────────┼──────────────────┘
                           │
                  ┌────────▼────────┐
                  │  AI Engine      │
                  │  (FastAPI)      │
                  │                 │
                  │ • Domain Scoring│
                  │ • Support Level │
                  │ • Persona Gen   │
                  │ • Recommendations│
                  │ • SHAP Analysis │
                  └────────┬────────┘
                           │
        ┌──────────────────┼──────────────────┐
        │                  │                  │
┌───────▼────────┐ ┌───────▼────────┐ ┌──────▼───────┐
│  PostgreSQL    │ │     Redis      │ │   Amazon S3  │
│                │ │                │ │              │
│ • User Data    │ │ • Sessions     │ │ • Reports    │
│ • Assessments  │ │ • Cache        │ │ • Media      │
│ • Personas     │ │ • Pub/Sub      │ │ • Backups    │
└────────────────┘ └────────────────┘ └──────────────┘
```

### 5. Adaptive Logic Model

#### Age-Based UI Mode
- **Child Mode** (Age < 12):
  - Visual-heavy design with large interactive elements
  - Simplified language (5-8 words per sentence)
  - Mandatory caregiver oversight
  - Gamification and reward systems
  - Visual supports (social stories, emotion charts)
  
- **Teen/Adult Mode** (Age ≥ 12):
  - Standard complexity with detailed information
  - Executive functioning tools (task lists, reminders)
  - Autonomy features and privacy controls
  - Self-advocacy resources
  - Optional caregiver visibility

#### Support Level Classification
- **Level 1** (Lower Support Needs):
  - Focus on skill development and independence
  - 1-2 recommendations per low-scoring domain
  - Check-ins every 3-4 days
  - Weekly caregiver summaries
  
- **Level 2** (Moderate Support Needs):
  - Balanced intervention across domains
  - 2-3 recommendations per low-scoring domain
  - Check-ins every 1-2 days
  - Daily caregiver summaries
  
- **Level 3** (Higher Support Needs):
  - Intensive strategies across all domains
  - 3-4 recommendations per low-scoring domain
  - Daily check-ins
  - Immediate caregiver alerts
  - Crisis resources enabled

#### Domain Score Influence
Domain scores (0-100) influence:
- **Recommendation Count**: Lower scores trigger more interventions
- **Intervention Intensity**: Scores < 30 activate intensive strategies
- **Monitoring Frequency**: Critical domains increase check-in frequency
- **Caregiver Alert Thresholds**: Scores < 20 trigger immediate alerts
- **Feature Availability**: Low sensory scores enable enhanced regulation tools

### 6. Security Architecture

#### Authentication & Authorization
- **JWT-Based Authentication**:
  - Access tokens (15-minute expiration)
  - Refresh tokens (7-day expiration with rotation)
  - Token revocation on logout
  - Device fingerprinting for security
  
- **OAuth 2.0 Provider Support**:
  - Google, Microsoft, Apple sign-in
  - PKCE flow for mobile apps
  - Scope-based permissions
  
- **RBAC Enforcement**:
  - User role: Access own data only
  - Caregiver role: Access linked users (with privacy settings)
  - Provider role: Access linked users (discipline-specific)
  - Admin role: System management (no PHI access)

#### Data Protection
- **Encryption at Rest**: AES-256-GCM for all sensitive data
- **Encryption in Transit**: TLS 1.3 for all communications
- **Database Encryption**: Transparent data encryption (TDE)
- **Key Management**: AWS KMS / Azure Key Vault / GCP KMS
- **Secure Communication**: mTLS between microservices

#### Compliance & Auditing
- **HIPAA Compliance**: 
  - Business Associate Agreements (BAAs)
  - Audit logging for all PHI access
  - Data retention policies
  - Breach notification procedures
  
- **Audit Trail Logging**:
  - User actions (assessment submission, recommendation updates)
  - Data access (who viewed what, when)
  - Administrative actions (configuration changes)
  - Security events (failed logins, permission changes)

### 7. Scalability Strategy

#### Horizontal Scaling
- **Containerized Services**: Docker enables easy replication
- **Stateless API Servers**: No session affinity required
- **Database Read Replicas**: Distribute read load across replicas
- **Redis Cluster Mode**: Sharded caching for high throughput
- **CDN for Static Assets**: Reduce origin server load

#### Performance Optimization
- **Caching Strategy**:
  - Redis for session and query caching
  - GraphQL query caching with Apollo
  - CDN caching for static assets (images, CSS, JS)
  - Database query result caching
  
- **Database Optimization**:
  - Indexing on frequently queried columns
  - Partitioning for time-series data (assessments, domain scores)
  - Connection pooling to reduce overhead
  - Query optimization with EXPLAIN ANALYZE
  
- **Load Balancing**:
  - Kubernetes service load balancing
  - Cloud provider load balancers (ALB, Cloud Load Balancing)
  - Geographic distribution for global users
  - Health checks and automatic failover

#### Auto-Scaling
- **Kubernetes HPA** (Horizontal Pod Autoscaler):
  - CPU-based scaling for compute-intensive services
  - Memory-based scaling for data-processing services
  - Custom metrics scaling (request queue depth)
  
- **Database Scaling**:
  - Vertical scaling for primary database
  - Read replicas for query distribution
  - Connection pooling with PgBouncer
  
- **Storage Scaling**:
  - S3 auto-scales with usage
  - Redis cluster expansion as needed

### 8. AI Explainability

The system integrates SHAP (SHapley Additive exPlanations) for transparent AI decision-making:

#### Feature Importance Analysis
- **Domain Score Influence**: Identify which domains most impact support level
- **Question-Level Attribution**: Show which assessment questions drive scores
- **Trend Impact**: Explain how historical patterns affect current recommendations

#### Clinician-Readable Explanations
- **Support Level Justification**: "Support Level 2 assigned because Sensory Processing (score: 45) and Emotional Regulation (score: 38) indicate moderate challenges requiring regular support."
- **Recommendation Rationale**: "Sensory regulation strategies recommended due to Sensory Processing score of 45, indicating hypersensitivity to auditory stimuli (Question 12: 'Often covers ears in noisy environments')."
- **Trend Explanations**: "Communication score improved from 52 to 68 over 4 weeks, correlated with consistent use of visual communication supports."

#### Transparency & Trust
- **Model Interpretation**: SHAP values for every prediction
- **Bias Detection**: Monitor for demographic disparities in recommendations
- **Audit Trail**: Log all AI decisions with explanations
- **Provider Override**: Clinicians can override AI recommendations with justification

### 9. Future Expansion

#### Sensor Integration
- **Wearable Devices**: Heart rate, skin conductance for stress detection
- **Environmental Sensors**: Noise level, light intensity monitoring
- **Activity Trackers**: Sleep patterns, physical activity correlation

#### Predictive Analytics
- **Overload Prediction**: Forecast sensory overload based on environmental factors
- **Meltdown Prevention**: Early warning system for emotional dysregulation
- **Intervention Optimization**: Predict most effective strategies per individual

#### Healthcare Integration
- **EHR Integration**: HL7 FHIR for clinical data exchange
- **Provider Portals**: Seamless integration with existing workflows
- **Telehealth Platforms**: Video consultation with therapists

#### Privacy-Preserving ML
- **Federated Learning**: Train models without centralizing sensitive data
- **Differential Privacy**: Aggregate insights while protecting individuals
- **Homomorphic Encryption**: Compute on encrypted data

### Core Design Principles

1. **Neurodiversity-Affirming**: Respects autism as neurological difference, not deficit
2. **Age-Adaptive**: Distinct interfaces for children (<12) and teens/adults (≥12)
3. **Domain-Specific**: Targets six ASD-specific functional areas
4. **Continuous Assessment**: Weekly reassessment to track dynamic changes
5. **Multi-Stakeholder**: Serves users, caregivers, and multidisciplinary care teams
6. **Non-Diagnostic**: Provides support for existing diagnoses, not diagnostic assessment
7. **Evidence-Based**: Recommendations grounded in occupational therapy, ABA, sensory integration, and speech-language pathology

### System Boundaries

**In Scope**:
- ASD-specific assessment across six domains
- Personalized recommendation generation
- Age-appropriate UI adaptation
- Weekly reassessment and trend tracking
- Report generation for caregivers and providers
- Adaptive feature reconfiguration based on support level
- Sensory profile management
- Meltdown/shutdown tracking

**Out of Scope**:
- ASD diagnosis or diagnostic assessment
- Medical treatment or medication management
- Crisis intervention services (provides resource links only)
- Replacement for professional therapy
- Real-time video/audio therapy sessions
- Insurance billing or claims processing
- Direct integration with medical devices

### Key Stakeholders

- **Primary Users**: Individuals with ASD (children, teens, adults)
- **Caregivers**: Parents, guardians, family members
- **Providers**: Occupational therapists, behavioral therapists, speech-language pathologists, psychologists, pediatricians



## Architecture

### System Architecture Overview

The system follows a layered architecture with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────┐
│                    Presentation Layer                        │
│  ┌──────────────────┐         ┌──────────────────┐         │
│  │  Child Mode UI   │         │ Teen/Adult UI    │         │
│  │  (Age < 12)      │         │  (Age ≥ 12)      │         │
│  └──────────────────┘         └──────────────────┘         │
│              │                          │                    │
│              └──────────┬───────────────┘                    │
│                         │                                    │
│                  ┌──────▼──────┐                            │
│                  │ UI_Adapter  │                            │
│                  └──────┬──────┘                            │
└─────────────────────────┼─────────────────────────────────┘
                          │
┌─────────────────────────┼─────────────────────────────────┐
│                  Application Layer                          │
│                         │                                    │
│  ┌──────────────────────┼──────────────────────────────┐   │
│  │                      │                               │   │
│  │  ┌───────────────────▼────────────────┐            │   │
│  │  │  ASD_Assessment_Engine             │            │   │
│  │  │  - Administers 6-domain assessment │            │   │
│  │  │  - Calculates domain scores        │            │   │
│  │  │  - Tracks reassessment schedule    │            │   │
│  │  └───────────────────┬────────────────┘            │   │
│  │                      │                               │   │
│  │  ┌───────────────────▼────────────────┐            │   │
│  │  │  ASD_Persona_Generator             │            │   │
│  │  │  - Creates sensory profiles        │            │   │
│  │  │  - Determines support level        │            │   │
│  │  │  - Identifies strengths/challenges │            │   │
│  │  └───────────────────┬────────────────┘            │   │
│  │                      │                               │   │
│  │  ┌───────────────────▼────────────────┐            │   │
│  │  │  ASD_Recommendation_Engine         │            │   │
│  │  │  - Generates domain-specific recs  │            │   │
│  │  │  - Matches evidence-based strategies│           │   │
│  │  │  - Adapts to age and support level │            │   │
│  │  └────────────────────────────────────┘            │   │
│  │                                                      │   │
│  │  ┌──────────────────────────────────────┐          │   │
│  │  │  Report_Generator                    │          │   │
│  │  │  - Creates provider reports          │          │   │
│  │  │  - Creates caregiver reports         │          │   │
│  │  │  - Tracks trends and patterns        │          │   │
│  │  └──────────────────────────────────────┘          │   │
│  │                                                      │   │
│  │  ┌──────────────────────────────────────┐          │   │
│  │  │  Feature_Reconfiguration_Engine      │          │   │
│  │  │  - Adjusts check-in frequency        │          │   │
│  │  │  - Modifies recommendation complexity│          │   │
│  │  │  - Updates notification thresholds   │          │   │
│  │  └──────────────────────────────────────┘          │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────┼─────────────────────────────────┘
                          │
┌─────────────────────────┼─────────────────────────────────┐
│                    Data Layer                               │
│                         │                                    │
│  ┌──────────────────────▼──────────────────────────────┐   │
│  │              Data Access Layer                       │   │
│  │  - User profiles and authentication                  │   │
│  │  - Assessment responses and scores                   │   │
│  │  - Personas and sensory profiles                     │   │
│  │  - Recommendations and adherence tracking            │   │
│  │  - Reports and longitudinal data                     │   │
│  │  - Meltdown/shutdown tracking                        │   │
│  └──────────────────────┬──────────────────────────────┘   │
│                         │                                    │
│  ┌──────────────────────▼──────────────────────────────┐   │
│  │         Persistent Storage (Database)                │   │
│  │  - Encrypted at rest (AES-256)                       │   │
│  │  - HIPAA-compliant data handling                     │   │
│  │  - Audit logging for all access                      │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

### Component Descriptions

#### UI_Adapter
Responsible for rendering age-appropriate interfaces and managing sensory accommodations.

**Responsibilities**:
- Determine UI mode based on user age
- Apply sensory preferences (color schemes, animation settings, quiet mode)
- Format content for age-appropriate complexity
- Manage navigation consistency and predictability
- Handle accessibility features (screen readers, keyboard navigation)

**Key Operations**:
- `determineUIMode(age: number): UIMode`
- `applySensoryPreferences(preferences: SensoryPreferences): void`
- `formatContent(content: Content, age: number, supportLevel: SupportLevel): FormattedContent`



#### ASD_Assessment_Engine
Administers assessments, calculates domain scores, and manages reassessment scheduling.

**Responsibilities**:
- Present assessment questions for six ASD-specific domains
- Calculate domain scores (0-100) using validated algorithms
- Schedule and trigger weekly reassessments
- Track assessment completion and progress
- Compare current scores to historical data
- Flag significant changes for review

**Key Operations**:
- `administerInitialAssessment(userId: string): Assessment`
- `administerReassessment(userId: string): Assessment`
- `calculateDomainScores(responses: AssessmentResponses): DomainScores`
- `identifyTrends(currentScores: DomainScores, historicalScores: DomainScores[]): Trends`
- `flagSignificantChanges(trends: Trends): Alert[]`

#### ASD_Persona_Generator
Creates comprehensive user personas including sensory profiles and support level determination.

**Responsibilities**:
- Analyze domain scores to determine support level (1-3)
- Generate detailed sensory profiles
- Identify primary challenge areas and strengths
- Document communication preferences and behavioral patterns
- Create emotional regulation profiles
- Select evidence-based intervention strategies

**Key Operations**:
- `determineSupportLevel(domainScores: DomainScores): SupportLevel`
- `generateSensoryProfile(assessmentData: AssessmentData): SensoryProfile`
- `identifyStrengthsAndChallenges(domainScores: DomainScores): StrengthsChallenges`
- `createPersona(assessmentData: AssessmentData, domainScores: DomainScores): Persona`

#### ASD_Recommendation_Engine
Generates personalized, evidence-based recommendations tailored to individual needs.

**Responsibilities**:
- Match persona characteristics to intervention strategies
- Generate domain-specific recommendations
- Prioritize based on lowest-scoring domains
- Adapt complexity to age and support level
- Include rationale and expected outcomes
- Use neurodiversity-affirming language

**Key Operations**:
- `generateRecommendations(persona: Persona): Recommendation[]`
- `prioritizeByDomain(recommendations: Recommendation[], domainScores: DomainScores): Recommendation[]`
- `adaptToAge(recommendation: Recommendation, age: number): Recommendation`
- `formatWithRationale(recommendation: Recommendation, domainScores: DomainScores): FormattedRecommendation`

#### Report_Generator
Produces structured reports for caregivers and multidisciplinary care teams.

**Responsibilities**:
- Generate weekly provider reports (discipline-specific)
- Generate weekly caregiver reports (plain language)
- Create trend visualizations
- Track meltdown/shutdown patterns
- Correlate environmental factors with functioning changes
- Include appropriate disclaimers

**Key Operations**:
- `generateProviderReport(userId: string, providerType: ProviderType): ProviderReport`
- `generateCaregiverReport(userId: string): CaregiverReport`
- `createTrendGraphs(historicalData: HistoricalData): Graph[]`
- `analyzeMeltdownPatterns(meltdownData: MeltdownData[]): MeltdownAnalysis`

#### Feature_Reconfiguration_Engine
Automatically adjusts system features based on support level changes.

**Responsibilities**:
- Adjust check-in frequency based on support level
- Modify recommendation complexity and quantity
- Update caregiver notification thresholds
- Enable/disable crisis resources
- Recalibrate sensory regulation tool prominence

**Key Operations**:
- `reconfigureFeatures(supportLevel: SupportLevel): FeatureConfiguration`
- `adjustCheckInFrequency(supportLevel: SupportLevel): number`
- `updateNotificationThresholds(supportLevel: SupportLevel): NotificationThresholds`
- `enableCrisisResources(supportLevel: SupportLevel): boolean`



## Components and Interfaces

### Core Interfaces

#### Assessment Interface

```typescript
interface Assessment {
  id: string;
  userId: string;
  assessmentType: 'initial' | 'reassessment';
  timestamp: Date;
  domains: DomainAssessment[];
  completed: boolean;
  environmentalFactors?: EnvironmentalFactor[];
}

interface DomainAssessment {
  domain: ASDDomain;
  questions: Question[];
  responses: Response[];
  score: number; // 0-100
}

enum ASDDomain {
  COMMUNICATION = 'communication',
  SOCIAL_INTERACTION = 'social_interaction',
  SENSORY_PROCESSING = 'sensory_processing',
  ROUTINE_BEHAVIORAL_RIGIDITY = 'routine_behavioral_rigidity',
  EMOTIONAL_REGULATION = 'emotional_regulation',
  ADAPTIVE_PHYSICAL_FUNCTIONING = 'adaptive_physical_functioning'
}

interface Question {
  id: string;
  domain: ASDDomain;
  text: string;
  childModeText?: string; // Simplified version for age < 12
  responseType: 'likert' | 'multiple_choice' | 'yes_no' | 'frequency';
  options?: string[];
  visualSupport?: VisualSupport; // For child mode
}

interface Response {
  questionId: string;
  value: string | number;
  timestamp: Date;
}

interface EnvironmentalFactor {
  type: 'school_transition' | 'family_change' | 'seasonal' | 'medical' | 'other';
  description: string;
  timestamp: Date;
}
```

#### Persona Interface

```typescript
interface Persona {
  id: string;
  userId: string;
  timestamp: Date;
  age: number;
  supportLevel: SupportLevel;
  domainScores: DomainScores;
  primaryChallenges: ASDDomain[]; // Two lowest-scoring domains
  strengthAreas: ASDDomain[]; // Two highest-scoring domains
  sensoryProfile: SensoryProfile;
  communicationPreferences: CommunicationPreferences;
  behavioralPatterns: BehavioralPatterns;
  emotionalRegulationProfile: EmotionalRegulationProfile;
  socialInteractionStyle: SocialInteractionStyle;
  recommendedStrategies: InterventionStrategy[];
  environmentalAccommodations: Accommodation[];
}

enum SupportLevel {
  LEVEL_1 = 1, // Lower support needs
  LEVEL_2 = 2, // Moderate support needs
  LEVEL_3 = 3  // Higher support needs
}

interface DomainScores {
  communication: number;
  socialInteraction: number;
  sensoryProcessing: number;
  routineBehavioralRigidity: number;
  emotionalRegulation: number;
  adaptivePhysicalFunctioning: number;
}

interface SensoryProfile {
  hypersensitivities: SensoryPattern[];
  hyposensitivities: SensoryPattern[];
  sensorySeekingBehaviors: string[];
  overloadTriggers: string[];
  effectiveRegulationStrategies: string[];
}

interface SensoryPattern {
  modality: 'auditory' | 'visual' | 'tactile' | 'olfactory' | 'gustatory' | 'vestibular' | 'proprioceptive';
  severity: 'mild' | 'moderate' | 'severe';
  specificTriggers: string[];
}

interface CommunicationPreferences {
  verbalAbility: 'verbal' | 'minimally_verbal' | 'nonspeaking';
  aacUse: boolean;
  aacType?: string;
  processingTimeNeeds: 'standard' | 'extended' | 'significant';
  pragmaticChallenges: string[];
  preferredCommunicationModes: string[];
}

interface BehavioralPatterns {
  routineNeeds: 'flexible' | 'moderate' | 'rigid';
  transitionDifficulty: 'minimal' | 'moderate' | 'significant';
  repetitiveBehaviors: string[];
  restrictedInterests: string[];
  stimmingPatterns: string[];
}

interface EmotionalRegulationProfile {
  meltdownFrequency: number; // per week
  meltdownTriggers: string[];
  meltdownDuration: number; // average minutes
  shutdownFrequency: number; // per week
  shutdownTriggers: string[];
  anxietyLevel: 'low' | 'moderate' | 'high';
  currentCopingStrategies: string[];
}

interface SocialInteractionStyle {
  peerEngagementLevel: 'minimal' | 'moderate' | 'active';
  socialMotivation: 'low' | 'moderate' | 'high';
  groupParticipationPreference: 'avoids' | 'tolerates' | 'enjoys';
  socialCueInterpretation: 'significant_difficulty' | 'moderate_difficulty' | 'mild_difficulty';
}

interface InterventionStrategy {
  type: 'sensory_regulation' | 'social_skills' | 'communication_scaffolding' | 
        'behavioral_support' | 'emotional_regulation' | 'executive_functioning';
  description: string;
  evidenceBase: string;
}

interface Accommodation {
  category: 'sensory' | 'routine' | 'communication' | 'social' | 'physical';
  description: string;
  setting: 'home' | 'school' | 'work' | 'community' | 'all';
}
```



#### Recommendation Interface

```typescript
interface Recommendation {
  id: string;
  userId: string;
  personaId: string;
  timestamp: Date;
  domain: ASDDomain;
  priority: 'high' | 'medium' | 'low';
  title: string;
  description: string;
  rationale: string; // Why this recommendation based on assessment data
  expectedOutcomes: string[];
  timeframe: string;
  evidenceBase: string;
  ageAdapted: boolean;
  targetAge: number;
  supportLevel: SupportLevel;
  steps: RecommendationStep[];
  visualSupports?: VisualSupport[]; // For child mode
  status: RecommendationStatus;
  adherenceData?: AdherenceData;
}

interface RecommendationStep {
  order: number;
  description: string;
  childModeDescription?: string;
  visualSupport?: VisualSupport;
}

enum RecommendationStatus {
  NOT_STARTED = 'not_started',
  STARTED = 'started',
  COMPLETED = 'completed',
  HELPFUL = 'helpful',
  NOT_HELPFUL = 'not_helpful',
  NOT_APPLICABLE = 'not_applicable'
}

interface AdherenceData {
  statusChanges: StatusChange[];
  feedback: string[];
  outcomeRating?: number; // 1-5
}

interface StatusChange {
  status: RecommendationStatus;
  timestamp: Date;
  notes?: string;
}

interface VisualSupport {
  type: 'social_story' | 'visual_schedule' | 'emotion_chart' | 'icon' | 'diagram';
  url?: string;
  description: string;
}
```

#### Report Interfaces

```typescript
interface ProviderReport {
  id: string;
  userId: string;
  providerType: ProviderType;
  reportPeriod: DateRange;
  timestamp: Date;
  domainTrends: DomainTrend[];
  currentSupportLevel: SupportLevel;
  supportLevelChanges: SupportLevelChange[];
  sensoryData?: SensoryReportData; // For OTs
  behavioralData?: BehavioralReportData; // For behavioral therapists
  communicationData?: CommunicationReportData; // For SLPs
  overallFunctioning?: OverallFunctioningData; // For psychologists/pediatricians
  recommendations: Recommendation[];
  adherenceMetrics: AdherenceMetrics;
  flaggedConcerns: Concern[];
  environmentalCorrelations: EnvironmentalCorrelation[];
  disclaimers: string[];
}

enum ProviderType {
  OCCUPATIONAL_THERAPIST = 'occupational_therapist',
  BEHAVIORAL_THERAPIST = 'behavioral_therapist',
  SPEECH_LANGUAGE_PATHOLOGIST = 'speech_language_pathologist',
  PSYCHOLOGIST = 'psychologist',
  PEDIATRICIAN = 'pediatrician'
}

interface DomainTrend {
  domain: ASDDomain;
  currentScore: number;
  previousScore: number;
  change: number;
  trend: 'improving' | 'stable' | 'declining';
  historicalData: DataPoint[];
}

interface DataPoint {
  timestamp: Date;
  value: number;
}

interface SupportLevelChange {
  fromLevel: SupportLevel;
  toLevel: SupportLevel;
  timestamp: Date;
  triggeringFactors: string[];
}

interface SensoryReportData {
  sensoryProfile: SensoryProfile;
  overloadFrequency: number;
  overloadTriggers: string[];
  regulationStrategyEffectiveness: StrategyEffectiveness[];
  motorCoordinationTrends: DataPoint[];
}

interface BehavioralReportData {
  meltdownFrequency: number;
  meltdownDuration: number;
  meltdownTriggers: string[];
  shutdownFrequency: number;
  routineStabilityScore: number;
  transitionDifficultyScore: number;
  behavioralInterventionOutcomes: InterventionOutcome[];
}

interface CommunicationReportData {
  verbalCommunicationTrend: DataPoint[];
  nonverbalCommunicationTrend: DataPoint[];
  pragmaticLanguageChallenges: string[];
  socialInteractionPatterns: string[];
  aacUsagePatterns?: AACUsageData;
}

interface OverallFunctioningData {
  allDomainTrends: DomainTrend[];
  emotionalRegulationPatterns: EmotionalPattern[];
  adaptiveFunctioningTrends: DataPoint[];
  sleepPatterns?: SleepData;
  eatingPatterns?: EatingData;
}

interface CaregiverReport {
  id: string;
  userId: string;
  reportPeriod: DateRange;
  timestamp: Date;
  summary: string; // Plain language overview
  improvements: Improvement[];
  concerns: Concern[];
  sensoryStrategies: string[];
  routineRecommendations: string[];
  socialSupport: string[];
  communicationTips: string[];
  emotionalRegulationSupport: string[];
  meltdownShutdownSummary: MeltdownShutdownSummary;
  environmentalAccommodations: Accommodation[];
  recommendationsProvided: Recommendation[];
  conversationStarters: string[];
  providerQuestions: string[];
  strengthsCelebration: string[];
}

interface Improvement {
  domain: ASDDomain;
  description: string;
  positiveReinforcement: string;
}

interface Concern {
  domain: ASDDomain;
  description: string;
  severity: 'low' | 'medium' | 'high';
  actionableGuidance: string[];
  professionalConsultationRecommended: boolean;
}

interface MeltdownShutdownSummary {
  meltdownCount: number;
  shutdownCount: number;
  identifiedTriggers: string[];
  patterns: string[];
  preventionStrategies: string[];
}
```



#### User and Configuration Interfaces

```typescript
interface User {
  id: string;
  dateOfBirth: Date;
  age: number;
  diagnosisDate?: Date;
  linkedCaregivers: string[]; // Caregiver user IDs
  linkedProviders: string[]; // Provider user IDs
  sensoryPreferences: SensoryPreferences;
  privacySettings: PrivacySettings;
  currentPersonaId: string;
  currentSupportLevel: SupportLevel;
  assessmentSchedule: AssessmentSchedule;
  featureConfiguration: FeatureConfiguration;
}

interface SensoryPreferences {
  colorScheme: 'standard' | 'high_contrast' | 'muted' | 'custom';
  animationLevel: 'full' | 'reduced' | 'none';
  soundEnabled: boolean;
  quietMode: boolean;
  textSize: 'small' | 'medium' | 'large' | 'extra_large';
}

interface PrivacySettings {
  caregiverVisibility: 'full' | 'limited' | 'none'; // For users 12+
  dataSharing: {
    providers: boolean;
    research: boolean; // Anonymized data
  };
  notificationPreferences: NotificationPreferences;
}

interface NotificationPreferences {
  method: 'email' | 'app' | 'sms' | 'multiple';
  frequency: 'immediate' | 'daily' | 'weekly';
  quietHours?: TimeRange;
}

interface AssessmentSchedule {
  lastAssessment: Date;
  nextAssessment: Date;
  reassessmentFrequency: number; // days
  remindersSent: number;
}

interface FeatureConfiguration {
  checkInFrequency: number; // days
  recommendationComplexity: 'simple' | 'moderate' | 'detailed';
  maxActiveRecommendations: number;
  crisisResourcesEnabled: boolean;
  intensiveSensoryToolsEnabled: boolean;
  caregiverNotificationThreshold: NotificationThreshold;
}

enum NotificationThreshold {
  IMMEDIATE = 'immediate', // Level 3
  DAILY = 'daily',         // Level 2
  WEEKLY = 'weekly'        // Level 1
}

interface Caregiver {
  id: string;
  linkedUsers: string[]; // User IDs they support
  notificationPreferences: NotificationPreferences;
  reportDeliveryMethod: 'email' | 'app' | 'portal';
}

interface Provider {
  id: string;
  type: ProviderType;
  linkedUsers: string[]; // User IDs they treat
  specializations: string[];
  reportAccessLevel: 'full' | 'discipline_specific';
}
```

### Data Flow Diagrams

#### Initial Assessment Flow

```
User Registration
       ↓
Collect Date of Birth
       ↓
Calculate Age → Determine UI Mode (Child/Teen-Adult)
       ↓
Link Caregiver (if age < 12)
       ↓
Display Non-Diagnostic Disclaimer
       ↓
ASD_Assessment_Engine.administerInitialAssessment()
       ↓
Present 6 Domain Questions (age-adapted)
       ↓
Collect Responses
       ↓
ASD_Assessment_Engine.calculateDomainScores()
       ↓
ASD_Persona_Generator.determineSupportLevel()
       ↓
ASD_Persona_Generator.createPersona()
  - Generate Sensory Profile
  - Identify Strengths/Challenges
  - Document Communication Preferences
  - Create Behavioral Patterns
  - Build Emotional Regulation Profile
       ↓
ASD_Recommendation_Engine.generateRecommendations()
  - Match to Persona
  - Prioritize by Domain Scores
  - Adapt to Age and Support Level
       ↓
UI_Adapter.formatContent()
       ↓
Display Recommendations to User
       ↓
Schedule Weekly Reassessment
       ↓
Feature_Reconfiguration_Engine.reconfigureFeatures()
```

#### Weekly Reassessment Flow

```
7 Days Since Last Assessment
       ↓
Send Reassessment Reminder (sensory-friendly)
       ↓
ASD_Assessment_Engine.administerReassessment()
       ↓
Present Shortened 6 Domain Questions
       ↓
Collect Responses + Environmental Factors
       ↓
ASD_Assessment_Engine.calculateDomainScores()
       ↓
ASD_Assessment_Engine.identifyTrends()
  - Compare to Previous Scores
  - Flag Changes > 15 points
  - Alert if Sensory/Emotional < 30
       ↓
ASD_Persona_Generator.determineSupportLevel()
       ↓
Support Level Changed?
  ├─ Yes → Update Support Level
  │         ↓
  │    Feature_Reconfiguration_Engine.reconfigureFeatures()
  │         ↓
  │    Notify Caregiver & Provider
  │
  └─ No → Continue
       ↓
ASD_Persona_Generator.updatePersona()
  - Refresh Sensory Profile
  - Update Communication Preferences
  - Revise Behavioral Patterns
       ↓
Domain Scores Changed Significantly?
  ├─ Yes → ASD_Recommendation_Engine.regenerateRecommendations()
  └─ No → Keep Current Recommendations
       ↓
Report_Generator.generateProviderReport()
Report_Generator.generateCaregiverReport()
       ↓
Deliver Reports
       ↓
Schedule Next Reassessment
```



## Data Models

### Core Entities

#### User Entity

```typescript
class User {
  id: string;
  dateOfBirth: Date;
  diagnosisDate?: Date;
  linkedCaregivers: string[];
  linkedProviders: string[];
  sensoryPreferences: SensoryPreferences;
  privacySettings: PrivacySettings;
  createdAt: Date;
  updatedAt: Date;
  
  // Computed properties
  get age(): number {
    return this.calculateAge(this.dateOfBirth);
  }
  
  get uiMode(): UIMode {
    return this.age < 12 ? UIMode.CHILD : UIMode.TEEN_ADULT;
  }
  
  get currentPersona(): Persona | null {
    // Fetch most recent persona
  }
  
  get currentSupportLevel(): SupportLevel | null {
    return this.currentPersona?.supportLevel;
  }
  
  private calculateAge(dob: Date): number {
    const today = new Date();
    let age = today.getFullYear() - dob.getFullYear();
    const monthDiff = today.getMonth() - dob.getMonth();
    if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < dob.getDate())) {
      age--;
    }
    return age;
  }
}
```

#### Assessment Entity

```typescript
class Assessment {
  id: string;
  userId: string;
  assessmentType: 'initial' | 'reassessment';
  timestamp: Date;
  completed: boolean;
  domainAssessments: DomainAssessment[];
  environmentalFactors: EnvironmentalFactor[];
  
  // Computed properties
  get domainScores(): DomainScores {
    return {
      communication: this.getScoreForDomain(ASDDomain.COMMUNICATION),
      socialInteraction: this.getScoreForDomain(ASDDomain.SOCIAL_INTERACTION),
      sensoryProcessing: this.getScoreForDomain(ASDDomain.SENSORY_PROCESSING),
      routineBehavioralRigidity: this.getScoreForDomain(ASDDomain.ROUTINE_BEHAVIORAL_RIGIDITY),
      emotionalRegulation: this.getScoreForDomain(ASDDomain.EMOTIONAL_REGULATION),
      adaptivePhysicalFunctioning: this.getScoreForDomain(ASDDomain.ADAPTIVE_PHYSICAL_FUNCTIONING)
    };
  }
  
  get lowestScoringDomains(): ASDDomain[] {
    // Return two domains with lowest scores
  }
  
  get highestScoringDomains(): ASDDomain[] {
    // Return two domains with highest scores
  }
  
  private getScoreForDomain(domain: ASDDomain): number {
    const domainAssessment = this.domainAssessments.find(da => da.domain === domain);
    return domainAssessment?.score || 0;
  }
}
```

#### Persona Entity

```typescript
class Persona {
  id: string;
  userId: string;
  assessmentId: string;
  timestamp: Date;
  age: number;
  supportLevel: SupportLevel;
  domainScores: DomainScores;
  primaryChallenges: ASDDomain[];
  strengthAreas: ASDDomain[];
  sensoryProfile: SensoryProfile;
  communicationPreferences: CommunicationPreferences;
  behavioralPatterns: BehavioralPatterns;
  emotionalRegulationProfile: EmotionalRegulationProfile;
  socialInteractionStyle: SocialInteractionStyle;
  recommendedStrategies: InterventionStrategy[];
  environmentalAccommodations: Accommodation[];
  
  // Methods
  requiresIntensiveSupport(): boolean {
    return this.supportLevel === SupportLevel.LEVEL_3;
  }
  
  hasSensoryConcerns(): boolean {
    return this.domainScores.sensoryProcessing < 30;
  }
  
  hasEmotionalRegulationConcerns(): boolean {
    return this.domainScores.emotionalRegulation < 30;
  }
  
  getCriticalDomains(): ASDDomain[] {
    const critical: ASDDomain[] = [];
    Object.entries(this.domainScores).forEach(([domain, score]) => {
      if (score < 30) {
        critical.push(domain as ASDDomain);
      }
    });
    return critical;
  }
}
```

#### Recommendation Entity

```typescript
class Recommendation {
  id: string;
  userId: string;
  personaId: string;
  timestamp: Date;
  domain: ASDDomain;
  priority: 'high' | 'medium' | 'low';
  title: string;
  description: string;
  rationale: string;
  expectedOutcomes: string[];
  timeframe: string;
  evidenceBase: string;
  ageAdapted: boolean;
  targetAge: number;
  supportLevel: SupportLevel;
  steps: RecommendationStep[];
  visualSupports: VisualSupport[];
  status: RecommendationStatus;
  adherenceData: AdherenceData;
  createdAt: Date;
  updatedAt: Date;
  
  // Methods
  updateStatus(newStatus: RecommendationStatus, notes?: string): void {
    this.status = newStatus;
    this.adherenceData.statusChanges.push({
      status: newStatus,
      timestamp: new Date(),
      notes
    });
    this.updatedAt = new Date();
  }
  
  addFeedback(feedback: string): void {
    this.adherenceData.feedback.push(feedback);
    this.updatedAt = new Date();
  }
  
  isActive(): boolean {
    return this.status === RecommendationStatus.NOT_STARTED || 
           this.status === RecommendationStatus.STARTED;
  }
  
  wasHelpful(): boolean {
    return this.status === RecommendationStatus.HELPFUL || 
           this.status === RecommendationStatus.COMPLETED;
  }
}
```

### Database Schema

#### Users Table

```sql
CREATE TABLE users (
  id UUID PRIMARY KEY,
  date_of_birth DATE NOT NULL,
  diagnosis_date DATE,
  sensory_preferences JSONB,
  privacy_settings JSONB,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_users_dob ON users(date_of_birth);
```

#### Caregivers Table

```sql
CREATE TABLE caregivers (
  id UUID PRIMARY KEY,
  notification_preferences JSONB,
  report_delivery_method VARCHAR(50),
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

#### User_Caregiver_Links Table

```sql
CREATE TABLE user_caregiver_links (
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  caregiver_id UUID REFERENCES caregivers(id) ON DELETE CASCADE,
  relationship VARCHAR(100),
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  PRIMARY KEY (user_id, caregiver_id)
);

CREATE INDEX idx_ucl_user ON user_caregiver_links(user_id);
CREATE INDEX idx_ucl_caregiver ON user_caregiver_links(caregiver_id);
```

#### Providers Table

```sql
CREATE TABLE providers (
  id UUID PRIMARY KEY,
  type VARCHAR(50) NOT NULL, -- ProviderType enum
  specializations TEXT[],
  report_access_level VARCHAR(50),
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);
```

#### User_Provider_Links Table

```sql
CREATE TABLE user_provider_links (
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  provider_id UUID REFERENCES providers(id) ON DELETE CASCADE,
  active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  PRIMARY KEY (user_id, provider_id)
);

CREATE INDEX idx_upl_user ON user_provider_links(user_id);
CREATE INDEX idx_upl_provider ON user_provider_links(provider_id);
```

#### Assessments Table

```sql
CREATE TABLE assessments (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  assessment_type VARCHAR(20) NOT NULL, -- 'initial' or 'reassessment'
  timestamp TIMESTAMP NOT NULL,
  completed BOOLEAN DEFAULT FALSE,
  domain_assessments JSONB NOT NULL,
  environmental_factors JSONB,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_assessments_user ON assessments(user_id);
CREATE INDEX idx_assessments_timestamp ON assessments(timestamp DESC);
CREATE INDEX idx_assessments_type ON assessments(assessment_type);
```

#### Domain_Scores Table

```sql
CREATE TABLE domain_scores (
  id UUID PRIMARY KEY,
  assessment_id UUID REFERENCES assessments(id) ON DELETE CASCADE,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  timestamp TIMESTAMP NOT NULL,
  communication DECIMAL(5,2) NOT NULL,
  social_interaction DECIMAL(5,2) NOT NULL,
  sensory_processing DECIMAL(5,2) NOT NULL,
  routine_behavioral_rigidity DECIMAL(5,2) NOT NULL,
  emotional_regulation DECIMAL(5,2) NOT NULL,
  adaptive_physical_functioning DECIMAL(5,2) NOT NULL,
  created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_domain_scores_user ON domain_scores(user_id);
CREATE INDEX idx_domain_scores_timestamp ON domain_scores(timestamp DESC);
CREATE INDEX idx_domain_scores_assessment ON domain_scores(assessment_id);
```

#### Personas Table

```sql
CREATE TABLE personas (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  assessment_id UUID REFERENCES assessments(id) ON DELETE CASCADE,
  timestamp TIMESTAMP NOT NULL,
  age INTEGER NOT NULL,
  support_level INTEGER NOT NULL, -- 1, 2, or 3
  domain_scores JSONB NOT NULL,
  primary_challenges TEXT[],
  strength_areas TEXT[],
  sensory_profile JSONB NOT NULL,
  communication_preferences JSONB NOT NULL,
  behavioral_patterns JSONB NOT NULL,
  emotional_regulation_profile JSONB NOT NULL,
  social_interaction_style JSONB NOT NULL,
  recommended_strategies JSONB,
  environmental_accommodations JSONB,
  created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_personas_user ON personas(user_id);
CREATE INDEX idx_personas_timestamp ON personas(timestamp DESC);
CREATE INDEX idx_personas_support_level ON personas(support_level);
```



#### Recommendations Table

```sql
CREATE TABLE recommendations (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  persona_id UUID REFERENCES personas(id) ON DELETE CASCADE,
  timestamp TIMESTAMP NOT NULL,
  domain VARCHAR(50) NOT NULL,
  priority VARCHAR(20) NOT NULL,
  title TEXT NOT NULL,
  description TEXT NOT NULL,
  rationale TEXT NOT NULL,
  expected_outcomes TEXT[],
  timeframe VARCHAR(100),
  evidence_base TEXT,
  age_adapted BOOLEAN DEFAULT FALSE,
  target_age INTEGER,
  support_level INTEGER,
  steps JSONB,
  visual_supports JSONB,
  status VARCHAR(50) NOT NULL DEFAULT 'not_started',
  adherence_data JSONB,
  created_at TIMESTAMP NOT NULL DEFAULT NOW(),
  updated_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_recommendations_user ON recommendations(user_id);
CREATE INDEX idx_recommendations_persona ON recommendations(persona_id);
CREATE INDEX idx_recommendations_domain ON recommendations(domain);
CREATE INDEX idx_recommendations_status ON recommendations(status);
CREATE INDEX idx_recommendations_timestamp ON recommendations(timestamp DESC);
```

#### Meltdown_Shutdown_Events Table

```sql
CREATE TABLE meltdown_shutdown_events (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  event_type VARCHAR(20) NOT NULL, -- 'meltdown' or 'shutdown'
  timestamp TIMESTAMP NOT NULL,
  duration_minutes INTEGER,
  triggers TEXT[],
  context TEXT,
  recovery_strategies TEXT[],
  caregiver_notes TEXT,
  created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_mse_user ON meltdown_shutdown_events(user_id);
CREATE INDEX idx_mse_timestamp ON meltdown_shutdown_events(timestamp DESC);
CREATE INDEX idx_mse_type ON meltdown_shutdown_events(event_type);
```

#### Reports Table

```sql
CREATE TABLE reports (
  id UUID PRIMARY KEY,
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  report_type VARCHAR(50) NOT NULL, -- 'provider' or 'caregiver'
  provider_type VARCHAR(50), -- Only for provider reports
  report_period_start DATE NOT NULL,
  report_period_end DATE NOT NULL,
  timestamp TIMESTAMP NOT NULL,
  content JSONB NOT NULL,
  delivered BOOLEAN DEFAULT FALSE,
  delivery_timestamp TIMESTAMP,
  created_at TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_reports_user ON reports(user_id);
CREATE INDEX idx_reports_type ON reports(report_type);
CREATE INDEX idx_reports_timestamp ON reports(timestamp DESC);
CREATE INDEX idx_reports_period ON reports(report_period_start, report_period_end);
```

#### Audit_Logs Table

```sql
CREATE TABLE audit_logs (
  id UUID PRIMARY KEY,
  user_id UUID,
  actor_id UUID NOT NULL, -- User, Caregiver, or Provider who performed action
  actor_type VARCHAR(50) NOT NULL, -- 'user', 'caregiver', 'provider'
  action VARCHAR(100) NOT NULL,
  resource_type VARCHAR(50),
  resource_id UUID,
  details JSONB,
  ip_address INET,
  timestamp TIMESTAMP NOT NULL DEFAULT NOW()
);

CREATE INDEX idx_audit_user ON audit_logs(user_id);
CREATE INDEX idx_audit_actor ON audit_logs(actor_id);
CREATE INDEX idx_audit_timestamp ON audit_logs(timestamp DESC);
CREATE INDEX idx_audit_action ON audit_logs(action);
```

### Data Relationships

```
users (1) ←→ (M) user_caregiver_links (M) ←→ (1) caregivers
users (1) ←→ (M) user_provider_links (M) ←→ (1) providers
users (1) ←→ (M) assessments
assessments (1) ←→ (1) domain_scores
assessments (1) ←→ (1) personas
users (1) ←→ (M) personas
personas (1) ←→ (M) recommendations
users (1) ←→ (M) recommendations
users (1) ←→ (M) meltdown_shutdown_events
users (1) ←→ (M) reports
users (1) ←→ (M) audit_logs
```

### Data Encryption and Security

All sensitive data is encrypted at rest using AES-256 encryption:

**Encrypted Fields**:
- User date of birth
- Assessment responses
- Sensory profiles
- Communication preferences
- Behavioral patterns
- Meltdown/shutdown event details
- Caregiver notes
- Provider observations

**Encryption Implementation**:
```typescript
class EncryptionService {
  private algorithm = 'aes-256-gcm';
  private keyDerivation = 'pbkdf2';
  
  encrypt(plaintext: string, key: Buffer): EncryptedData {
    const iv = crypto.randomBytes(16);
    const cipher = crypto.createCipheriv(this.algorithm, key, iv);
    const encrypted = Buffer.concat([
      cipher.update(plaintext, 'utf8'),
      cipher.final()
    ]);
    const authTag = cipher.getAuthTag();
    
    return {
      ciphertext: encrypted.toString('base64'),
      iv: iv.toString('base64'),
      authTag: authTag.toString('base64')
    };
  }
  
  decrypt(encryptedData: EncryptedData, key: Buffer): string {
    const decipher = crypto.createDecipheriv(
      this.algorithm,
      key,
      Buffer.from(encryptedData.iv, 'base64')
    );
    decipher.setAuthTag(Buffer.from(encryptedData.authTag, 'base64'));
    
    const decrypted = Buffer.concat([
      decipher.update(Buffer.from(encryptedData.ciphertext, 'base64')),
      decipher.final()
    ]);
    
    return decrypted.toString('utf8');
  }
}
```

**Access Control**:
- Role-based access control (RBAC) for all data operations
- Users can only access their own data
- Caregivers can access linked users' data (with privacy settings respected for users 12+)
- Providers can access linked users' data within their discipline scope
- All data access is logged in audit_logs table

