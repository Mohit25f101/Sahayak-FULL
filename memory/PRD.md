# Product Requirements Document: Sahayak Web App

## Executive Summary
Sahayak is an autonomous browser automation assistant that converts natural language commands into executable browser actions using AI-powered planning, vision capabilities, and self-healing element detection.

## Product Vision
Enable users to automate complex web tasks through simple conversational commands, with the system intelligently handling navigation, form filling, clicking, and verification using advanced AI and computer vision.

## Target Users
- Non-technical users needing web automation
- QA testers automating test scenarios
- Data researchers performing repetitive web tasks
- Productivity enthusiasts automating workflows
- Developers prototyping automation scripts

## Core Features

### 1. Natural Language Command Processing
- **User Input**: Free-form text commands (e.g., "Search for AI news on Google")
- **AI Planning**: Gemini 3 Flash converts commands to structured action plans
- **Context Awareness**: Uses user memory and history for smarter planning

### 2. Browser Automation
- **Supported Actions**: Navigate, click, type, wait, screenshot
- **Platform**: Playwright + Chromium
- **Execution Modes**:
  - Guided: Follows predefined plan
  - Autonomous: Dynamically adapts based on page state

### 3. Vision-Based Navigation
- **Screenshot Analysis**: OpenAI GPT Image 1 understands page content
- **Element Detection**: Identifies buttons, inputs, links visually
- **Error Recognition**: Detects error messages and unexpected states
- **OCR Fallback**: Tesseract for text extraction when needed

### 4. Self-Healing Selectors
- **Multi-Strategy Approach**:
  1. CSS selectors
  2. Text-based matching
  3. XPath queries
  4. OCR-based coordinate clicking
- **Auto-Recovery**: Tries alternative methods if primary fails

### 5. Memory System
- **User Preferences**: Stores emails, usernames, common inputs
- **Execution History**: Logs all commands and results
- **Auto-Fill**: Reuses saved data in future sessions
- **Privacy**: User controls what gets saved

### 6. Human-Like Behavior
- **Random Delays**: 0.5-2 seconds between actions
- **Gradual Mouse Movement**: 10-30 steps to target
- **Natural Typing**: Character-by-character with delays
- **Bot Detection Avoidance**: Mimics human interaction patterns

### 7. Privacy Protection
- **Sensitive Data Blurring**: Automatically blurs passwords, OTPs, PINs
- **Pre-Analysis Processing**: Sanitizes screenshots before AI analysis
- **User Control**: Toggle privacy layer on/off

### 8. Real-Time Feedback
- **Execution Log**: Step-by-step action display
- **Screenshot Gallery**: Visual verification of each step
- **Progress Indicators**: Loading states and status updates
- **Error Reporting**: Clear error messages with recovery suggestions

## Technical Architecture

### Frontend
- **Framework**: React 19
- **Styling**: Tailwind CSS + Shadcn UI
- **State Management**: React hooks
- **API Client**: Axios
- **Notifications**: Sonner toast

### Backend
- **Framework**: FastAPI
- **AI Integration**: emergentintegrations library
  - Gemini 3 Flash (planning)
  - OpenAI GPT Image 1 (vision)
- **Browser**: Playwright with Chromium
- **OCR**: Tesseract
- **Database**: SQLite (migrateable to Supabase PostgreSQL)

### Database Schema
```sql
user_memory: user_id, key, value, created_at
execution_history: user_id, command, steps, status, result, created_at
sessions: session_id, user_id, state, created_at, updated_at
```

## User Flows

### Flow 1: Execute Simple Command
1. User types: "Go to google.com"
2. Click "Execute" button
3. Backend creates plan: [navigate to google.com, screenshot]
4. Browser navigates to URL
5. Screenshot displayed to user
6. Success message shown

### Flow 2: Execute Complex Command (Guided Mode)
1. User types: "Search for AI news on Google"
2. AI generates plan:
   - Navigate to google.com
   - Click search box
   - Type "AI news"
   - Click search button
   - Screenshot results
3. Each step executes sequentially
4. Screenshots after each major action
5. Execution log updates in real-time
6. Final success/failure status

### Flow 3: Autonomous Operation
1. User types: "Find cheapest flight to NYC"
2. Mode: Autonomous
3. System:
   - Navigates to flight search site
   - Observes page layout (vision AI)
   - Fills search form intelligently
   - Clicks search
   - Analyzes results page
   - Identifies cheapest option
   - Screenshots final result
4. Goal achievement verified
5. Complete execution log provided

### Flow 4: Memory Usage
1. User types: "Save my email as john@example.com"
2. System saves to user_memory table
3. Later: User types: "Login to example.com with my email"
4. System retrieves saved email
5. Auto-fills email field
6. User confirms/modifies if needed

## Success Metrics
- Command success rate > 85%
- Average execution time < 30 seconds
- User satisfaction score > 4.5/5
- Self-healing success rate > 70%
- Privacy protection: 100% sensitive field detection

## Security & Privacy
- No data collection without user consent
- Sensitive data blurred before AI processing
- Local database (can upgrade to encrypted cloud)
- API keys stored securely in environment variables
- HTTPS-only communication

## Scalability
- Async/await throughout for concurrency
- Database supports millions of records
- Horizontal scaling via load balancer
- Browser instances can run in containers
- Cloud browser option for heavy loads

## Limitations
- Maximum 20 iterations per autonomous session
- Screenshot quality optimized (reduced size)
- Single browser tab per session
- No CAPTCHA solving (future feature)
- Limited to Chromium browser

## Future Roadmap

### Phase 11: Advanced Features
- Multi-tab support
- Session recording and replay
- Voice command input
- Collaborative sessions
- Plugin system for custom actions

### Phase 12: Enterprise Features
- Team workspaces
- Role-based access control
- Audit logs
- Advanced analytics dashboard
- SLA guarantees

### Phase 13: Intelligence Upgrades
- Multi-model ensemble (use multiple AI models)
- Reinforcement learning from successes
- Predictive actions based on patterns
- Natural language result interpretation
- Automated testing suite generation

## Deployment Strategy
1. **Development**: Local SQLite + Localhost testing
2. **Staging**: Vercel (frontend) + Railway (backend) + SQLite
3. **Production**: Vercel + Railway + Supabase PostgreSQL
4. **Monitoring**: Error tracking, performance monitoring
5. **Scaling**: Increase backend instances as needed

## Compliance
- GDPR: User data control and deletion rights
- CCPA: California privacy compliance
- Accessibility: WCAG 2.1 AA standards
- Terms of Service: Clear usage guidelines
- Privacy Policy: Transparent data practices

## Documentation
- README.md: Quick start guide
- IMPLEMENTATION_GUIDE.md: Technical details
- API_DOCS.md: Endpoint documentation
- USER_GUIDE.md: End-user instructions
- TROUBLESHOOTING.md: Common issues and fixes
