# ADR-003: AI Integration Platform

## Status
Accepted

## Date
2025-01-01

## Context
Brand Central AI requires a powerful AI platform for multiple use cases:
- Content idea generation based on brand context
- Brand positioning statement creation (Macro Fantasy Generator)  
- Content validation against brand guidelines (Brand Guardian)
- Prompt refinement with brand context
- L-M-A-I brand alignment scoring
- Community engagement response suggestions

Requirements:
- High-quality text generation with brand context awareness
- JSON-structured responses for programmatic use
- Reasonable pricing for MVP stage
- Strong context understanding (long prompts)
- Reliable API with good uptime
- TypeScript/JavaScript SDK support

## Decision
We will use **Google Gemini API (gemini-2.5-flash-preview-04-17 model)** as our primary AI integration platform.

## Rationale

### Considered Options
1. **Google Gemini API** - Google's latest multimodal AI model
2. **OpenAI GPT-4/GPT-3.5** - Industry-leading language models
3. **Anthropic Claude** - Constitutional AI with strong reasoning
4. **Multiple Providers** - Abstract layer with fallbacks
5. **Local AI Models** - Self-hosted solutions (Ollama, etc.)

### Decision Factors

**Google Gemini Advantages:**
- **Cost Effectiveness**: Competitive pricing vs OpenAI GPT-4
- **JSON Mode**: Native structured output support for our data needs
- **Context Length**: Large context window for brand guidelines + prompts
- **Multimodal Ready**: Future image/visual identity analysis capabilities
- **Enterprise Backing**: Google's infrastructure and reliability
- **Official SDK**: Well-maintained @google/genai TypeScript package
- **Performance**: Fast response times for user experience

**Comparison Analysis:**
| Feature | Gemini | OpenAI | Claude | Multiple |
|---------|--------|--------|---------|----------|
| Cost | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| JSON Support | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| Context Length | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| SDK Quality | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| Reliability | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |
| Implementation | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ |

**Why Not Others:**
- **OpenAI**: Higher costs, potential IP concerns for brand data
- **Claude**: Limited JSON support, fewer integration examples
- **Multiple Providers**: Complexity overhead, inconsistent outputs
- **Local Models**: Resource intensive, quality concerns, maintenance overhead

## Implementation Architecture

### Service Layer Design
```typescript
// services/GeminiService.ts
const API_KEY = process.env.API_KEY;
const ai = new GoogleGenAI({ apiKey: API_KEY });
const model = 'gemini-2.5-flash-preview-04-17';
```

### Core Functions
1. **getLMAIScore()** - Brand alignment analysis with structured scoring
2. **generateMacroFantasyStatement()** - Brand positioning creation  
3. **checkBrandAlignment()** - Content validation against guidelines
4. **generateContentIdeas()** - Topic-based content brainstorming
5. **refineUserPrompt()** - Context-aware prompt enhancement
6. **generateContentFromTemplate()** - Template-based content creation
7. **suggestCommentReply()** - Brand-voice community responses

### Error Handling Strategy
```typescript
// Fallback to mock responses when API unavailable
if (!API_KEY) {
  isMockClient = true;
  // Return mock data for development/demo
}
```

## Consequences

### Positive
- **Cost Control**: Predictable costs during MVP phase
- **Developer Experience**: Excellent TypeScript SDK and documentation
- **JSON Responses**: Native structured output reduces parsing complexity
- **Context Awareness**: Large context window enables rich brand integration
- **Future Expansion**: Multimodal capabilities for visual brand analysis
- **Reliability**: Google's infrastructure provides consistent uptime
- **Mock Fallback**: Graceful degradation when API unavailable

### Negative
- **Vendor Lock-in**: Tight coupling to Google's platform
- **API Dependencies**: Application functionality tied to external service
- **Quality Variance**: AI outputs may vary in quality/consistency
- **Rate Limiting**: Potential throttling during high usage
- **Cost Scaling**: Expenses grow with user adoption
- **Privacy Considerations**: Brand data sent to Google servers

### Risk Mitigation
- **API Key Management**: Secure environment-based configuration
- **Mock Implementation**: Full fallback system for development/demos
- **Rate Limiting**: Implement client-side request throttling
- **Error Boundaries**: Graceful handling of API failures
- **Cost Monitoring**: Track usage and implement budgets
- **Data Minimization**: Send only necessary context to API
- **Response Caching**: Cache common responses to reduce API calls

## Configuration

### Environment Setup
```typescript
// vite.config.ts
define: {
  'process.env.API_KEY': JSON.stringify(env.GEMINI_API_KEY),
}
```

### API Parameters
- **Model**: gemini-2.5-flash-preview-04-17
- **Temperature**: 0.5-0.7 (balanced creativity/consistency)
- **Max Tokens**: 150-500 (based on use case)
- **Response Format**: JSON for structured data, text for content

## Future Considerations

### Potential Migrations
- **Multi-Provider**: Abstract service layer for provider flexibility
- **Custom Models**: Fine-tuned models for brand-specific use cases
- **Local Inference**: Self-hosted models for enterprise privacy
- **Hybrid Approach**: Critical operations local, creative tasks cloud

### Enhancement Opportunities
- **Image Analysis**: Visual brand consistency checking
- **Voice/Tone Training**: Brand-specific language model fine-tuning
- **Real-time Collaboration**: Multi-user brand development sessions
- **Advanced Analytics**: AI-powered brand performance insights

## Monitoring & Success Metrics
- **Response Times**: Target <5 seconds for all AI interactions
- **Error Rates**: <2% API failures with graceful degradation
- **Cost Tracking**: Monthly API usage and budget adherence
- **Quality Assessment**: User satisfaction with AI-generated content
- **Usage Patterns**: Most/least used AI features for optimization 