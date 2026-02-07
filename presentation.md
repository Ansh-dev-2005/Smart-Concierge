# Smart Concierge
## Intelligent Conversational Interface for Campus Resource Management

---

## Overview

**Smart Concierge** adds an intelligent conversational interface to the Campus Resource Management system

- Natural text and voice interactions
- Contextual awareness and personalization
- Multi-step workflow guidance
- Cross-platform support (web & kiosk)

---

## Key Features

### 🎯 Intent Understanding
- Natural language query processing
- Entity extraction (mentors, resources, dates)
- Multi-intent detection
- Clarification when needed

### 💡 Smart Suggestions
- Context-aware recommendations
- Personalized based on user patterns
- Workflow-driven suggestions
- 3-5 ranked suggestions per interaction

---

## Key Features (cont.)

### 🔄 Workflow Orchestration
- Multi-step guided workflows
- State persistence and resumption
- Input validation at each step
- Error recovery options

### 🧠 Learning & Adaptation
- Pattern recognition from user interactions
- Workflow success tracking
- Time-based behavior analysis
- Privacy-preserving pattern storage

---

## Architecture Overview

```
┌─────────────────────────────────────────┐
│         Frontend Layer                  │
│  Next.js Web App | Electron Kiosk      │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│         Backend Layer                   │
│  NestJS API | Smart Concierge Service  │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│         AI Layer                        │
│  Voice Agent | Intent | RAG | LLM      │
└──────────────┬──────────────────────────┘
               │
┌──────────────▼──────────────────────────┐
│         Data Layer                      │
│  PostgreSQL + pgvector | Redis         │
└─────────────────────────────────────────┘
```

---

## Core Components

### 1. ConciergeController
- HTTP endpoint handling
- Query processing
- Workflow management
- Voice integration

### 2. ConciergeService
- Core orchestration logic
- Pipeline coordination
- Response generation

---

## Core Components (cont.)

### 3. IntentAnalyzer
- Enhanced intent classification
- Entity extraction
- Confidence scoring
- Clarification logic

### 4. ContextEngine
- User context management
- Recent activity tracking
- Conversation history
- Active workflow state

---

## Core Components (cont.)

### 5. SuggestionEngine
- Contextual recommendations
- Relevance scoring
- Pattern-based suggestions
- RAG-powered insights

### 6. WorkflowOrchestrator
- Multi-step workflow execution
- State management
- Pause/resume capability
- Error handling

---

## Core Components (cont.)

### 7. LearningModule
- Interaction recording
- Pattern analysis
- Success rate tracking
- Abandonment detection

---

## Supported Workflows

### 📅 Mentor Booking
1. Search mentors by expertise
2. Select mentor
3. Schedule session
4. Confirm booking

### 📊 Submission Tracking
1. Identify submission type
2. Retrieve status
3. Show timeline
4. Provide next steps

---

## Supported Workflows (cont.)

### 🔍 Resource Discovery
1. Understand requirements
2. Search inventory
3. Show alternatives
4. Guide reservation

### ✅ Approval Status
1. Identify approval type
2. Check current status
3. Estimate timeline
4. Show blockers

---

## Intent Categories

**Workflow Intents**
- Book mentor
- Track submission
- Find resources
- Check approval
- Reserve equipment
- Submit purchase

**Information Intents**
- View inventory
- Get project status
- List mentors
- View tasks

---

## Intent Categories (cont.)

**Action Intents**
- Add task
- Update project
- Add log

**Meta Intents**
- Get help
- Clarify
- Unknown

---

## Processing Pipeline

1. **Load User Context**
   - Projects, pending items, recent actions

2. **Classify Intent**
   - Analyze query with context
   - Extract entities

3. **Check Clarification**
   - Request details if confidence < 0.7

4. **Handle Workflow**
   - Continue active workflow OR start new

---

## Processing Pipeline (cont.)

5. **Generate Response**
   - Direct answer or workflow prompt

6. **Create Suggestions**
   - Context-aware recommendations

7. **Record Interaction**
   - Learning and pattern analysis

---

## User Context Structure

```typescript
{
  userId, role, activeProjectId,
  
  projects: [
    { id, title, status, feasibilityScore,
      missingResources, pendingTasks }
  ],
  
  pendingApprovals, pendingReservations,
  pendingPurchases,
  
  recentActions, conversationHistory,
  activeWorkflows,
  
  frequentIntents, preferredWorkflows,
  interactionPatterns
}
```

---

## Suggestion Generation Strategy

1. **Workflow-based** - Next steps for active workflows
2. **Context-based** - Pending approvals, missing resources
3. **Pattern-based** - User's frequent actions
4. **Intent-based** - Related to current query
5. **RAG-based** - Knowledge base recommendations

**Result**: Top 5 ranked suggestions

---

## Workflow State Management

### State Structure
- workflowId, type
- currentStep, totalSteps
- stepData (accumulated results)
- completed, paused flags

### Operations
- Start, execute step, pause, resume, cancel
- Persistent storage in database
- Redis caching for active workflows

---

## Learning & Patterns

### Tracked Metrics
- Intent frequency by user
- Workflow completion rates
- Time-based activity patterns
- Suggestion selection rates

### Pattern Analysis
- Identify frequent workflows
- Detect abandonment patterns
- Optimize suggestion ranking
- Improve intent classification

---

## Data Models

### New Tables
- **ConciergeInteraction** - Query logs with intent
- **WorkflowState** - Active workflow persistence
- **UserPattern** - Learned behavior patterns

### Existing Extensions
- User model extended with relations

### Redis Structures
- Context cache (5 min TTL)
- Intent cache (1 hour TTL)
- Workflow state (24 hour TTL)

---

## Design Principles

1. **Leverage Existing Infrastructure**
   - Reuse IntentService, MemoryService, RAG

2. **Minimal Schema Changes**
   - Extend, don't rebuild

3. **Backward Compatibility**
   - Existing endpoints unchanged

4. **Stateless Design**
   - All state in database/Redis

5. **Progressive Enhancement**
   - Graceful degradation when AI unavailable

---

## Error Handling

### Categories
- Intent classification errors → Clarification
- Workflow errors → Validation & recovery
- External service errors → Fallback behavior
- Data errors → Suggestions & alternatives

### Resilience
- Circuit breaker for external services
- Retry with exponential backoff
- Graceful degradation to rule-based responses

---

## Performance Targets

- **Intent Classification**: < 2 seconds (95th percentile)
- **Suggestion Generation**: < 1 second
- **Cache Hit Rate**: > 70%
- **Rate Limiting**: 60 requests/minute per user
- **Circuit Breaker**: Open after 5 failures

---

## Cross-Platform Support

### Web Platform
- Text-based interaction
- Rich UI with suggestions
- Inline workflow progress

### Kiosk Platform
- Voice + text input
- Touch-optimized interface
- Voice feedback (TTS)

### State Consistency
- Seamless transition between platforms
- Shared workflow state
- Synchronized context

---

## Voice Integration

### Flow
1. Kiosk captures audio
2. Voice-agent transcribes (STT)
3. Transcription sent to API
4. Smart Concierge processes
5. Response synthesized (TTS)
6. Audio played on kiosk

### Features
- Transcription display for confirmation
- Fallback to text on failure
- Voice feedback for responses

---

## Testing Strategy

### Dual Approach

**Unit Tests** (>80% coverage)
- Specific workflow examples
- Integration points
- Edge cases
- Error conditions

**Property-Based Tests** (37 properties)
- Universal invariants
- State consistency
- Performance characteristics
- Data integrity

---

## Key Properties (Examples)

**Property 1**: Intent Classification Completeness
- Every query returns valid intent with confidence 0-1

**Property 5**: Suggestion Generation Bounds
- Always return 3-5 suggestions, ordered by relevance

**Property 10**: Workflow State Progression
- Steps advance correctly, completion tracked

**Property 20**: Cross-Platform State Consistency
- Workflow state identical across web/kiosk

---

## Security & Privacy

### Privacy
- No PII in pattern storage
- Anonymized interaction logs
- User data isolation

### Security
- Input validation on all endpoints
- Rate limiting per user
- Operation timeouts (30s)
- Error logging without sensitive data

---

## Implementation Phases

### Phase 1: Core Infrastructure
- ConciergeController & Service
- IntentAnalyzer integration
- ContextEngine implementation

### Phase 2: Workflows
- Mentor booking workflow
- Submission tracking workflow
- Resource discovery workflow
- Approval status workflow

---

## Implementation Phases (cont.)

### Phase 3: Intelligence
- SuggestionEngine with RAG
- LearningModule & patterns
- Pattern-based recommendations

### Phase 4: Integration
- Voice integration
- Cross-platform state sync
- Performance optimization

---

## Success Metrics

### User Engagement
- Query volume and frequency
- Workflow completion rate
- Suggestion selection rate

### Performance
- Response time percentiles
- Cache effectiveness
- Error rates

### Learning
- Intent classification accuracy
- Pattern detection quality
- Suggestion relevance scores

---

## Benefits

### For Students
- ✅ Quick access to information
- ✅ Guided workflows for complex tasks
- ✅ Personalized recommendations
- ✅ Voice interaction on kiosks

### For HODs/Admins
- ✅ Faster approval workflows
- ✅ Proactive notifications
- ✅ Reduced support burden
- ✅ Usage insights

---

## Technology Stack

### Backend
- NestJS (TypeScript)
- Prisma ORM
- PostgreSQL + pgvector
- Redis

### AI/ML
- OpenAI / Ollama (LLM)
- FastAPI (Voice Agent)
- RAG with vector embeddings

### Frontend
- Next.js (Web)
- Electron (Kiosk)

---

## Next Steps

1. Review and approve design
2. Set up development environment
3. Implement Phase 1 (Core Infrastructure)
4. Create test suite with property-based tests
5. Integrate with existing agent service
6. Deploy to staging for testing
7. Iterate based on feedback

---

## Questions?

**Smart Concierge**
Intelligent Conversational Interface for Campus Resource Management

Thank you!
