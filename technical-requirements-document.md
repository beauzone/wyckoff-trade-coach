### 11. User Interface Technical Specifications

#### 11.1 Theme Implementation
The application will support both light and dark themes using CSS variables and a theme context:

```javascript
// Theme context
const ThemeContext = createContext({
  theme: 'dark',
  toggleTheme: () => {}
});

// CSS variables for theming
:root {
  /* Light theme (default) */
  --color-bg-primary: #ffffff;
  --color-bg-secondary: #f9fafb;
  --color-text-primary: #111827;
  --color-text-secondary: #4b5563;
  --color-accent: #2563eb;
  --color-border: #e5e7eb;
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-danger: #ef4444;
}

[data-theme='dark'] {
  --color-bg-primary: #111827;
  --color-bg-secondary: #1f2937;
  --color-text-primary: #f9fafb;
  --color-text-secondary: #d1d5db;
  --color-accent: #3b82f6;
  --color-border: #374151;
  --color-success: #10b981;
  --color-warning: #f59e0b;
  --color-danger: #ef4444;
}
```

#### 11.2 Layout Components
The application will use a modular component approach:

```javascript
// Layout structure
<AppShell>
  <Sidebar />
  <MainContent>
    <Header />
    <PageContent />
    <Footer />
  </MainContent>
  <AIAssistantPanel />
</AppShell>
```

#### 11.3 Dashboard Components
The dashboard will be built with a grid-based component system:

```javascript
<Dashboard>
  <DashboardHeader title="Portfolio Overview" />
  <Grid columns={12}>
    <GridItem colSpan={[12, 6, 3]}>
      <MetricCard 
        title="Total Return" 
        value="$17,431.14" 
        trend={+5.2} 
        trendLabel="vs last month" 
      />
    </GridItem>
    <GridItem colSpan={[12, 6, 3]}>
      <MetricCard 
        title="Win Rate" 
        value="65.3%" 
        trend={+2.1} 
        trendLabel="vs last month" 
      />
    </GridItem>
    <GridItem colSpan={[12, 12, 6]}>
      <ChartCard 
        title="Performance History"
        height={300}
        data={performanceData}
        type="line"
      />
    </GridItem>
    <GridItem colSpan={[12, 6, 6]}>
      <ActivePositionsTable data={positionsData} />
    </GridItem>
    <GridItem colSpan={[12, 6, 6]}>
      <WatchlistCard data={watchlistData} />
    </GridItem>
  </Grid>
</Dashboard>
```

#### 11.4 Data Table Implementation
Tables will be implemented using TanStack Table with sorting, filtering, and pagination:

```javascript
function PositionsTable({ data }) {
  const columns = useMemo(() => [
    {
      id: 'status',
      header: 'Status',
      accessorKey: 'status',
      cell: ({ row }) => (
        <StatusBadge status={row.original.status} />
      ),
    },
    {
      id: 'symbol',
      header: 'Symbol',
      accessorKey: 'symbol',
      cell: ({ row }) => (
        <SymbolCell 
          symbol={row.original.symbol} 
          type={row.original.assetType}
        />
      ),
    },
    // Additional columns...
  ], []);

  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
  });

  return (
    <div className="positions-table">
      <div className="table-toolbar">
        <GlobalFilter table={table} />
        <TableActions />
      </div>
      <Table>
        <TableHeader>
          {/* Header implementation */}
        </TableHeader>
        <TableBody>
          {/* Body implementation */}
        </TableBody>
      </Table>
      <TablePagination table={table} />
    </div>
  );
}
```

#### 11.5 Chart Components
Charts will be implemented using Recharts with theme-aware styling:

```javascript
function PerformanceChart({ data, height = 300 }) {
  const theme = useTheme();
  
  return (
    <div className="chart-container">
      <ResponsiveContainer width="100%" height={height}>
        <LineChart data={data} margin={{ top: 5, right: 20, bottom: 5, left: 0 }}>
          <Line 
            type="monotone" 
            dataKey="value" 
            stroke={theme === 'dark' ? '#3b82f6' : '#2563eb'} 
            strokeWidth={2} 
          />
          <CartesianGrid stroke={theme === 'dark' ? '#374151' : '#e5e7eb'} />
          <XAxis 
            dataKey="date" 
            stroke={theme === 'dark' ? '#9ca3af' : '#6b7280'} 
          />
          <YAxis stroke={theme === 'dark' ? '#9ca3af' : '#6b7280'} />
          <Tooltip content={<CustomTooltip />} />
        </LineChart>
      </ResponsiveContainer>
    </div>
  );
}
```

#### 11.6 AI Assistant Interface
The AI assistant will be implemented as a floating panel with voice controls:

```javascript
function AIAssistant() {
  const [isOpen, setIsOpen] = useState(false);
  const [isListening, setIsListening] = useState(false);
  const [messages, setMessages] = useState([]);
  
  return (
    <>
      <AIAssistantButton 
        onClick={() => setIsOpen(true)}
        position="bottom-right"
      />
      
      {isOpen && (
        <AIAssistantPanel>
          <AIAssistantHeader 
            onClose={() => setIsOpen(false)}
            title="Trading Assistant"
          />
          <AIMessageList messages={messages} />
          <AIInputBar>
            <VoiceButton 
              isListening={isListening}
              onClick={() => setIsListening(!isListening)}
            />
            <AITextInput />
            <AISendButton />
          </AIInputBar>
        </AIAssistantPanel>
      )}
    </>
  );
}
```

#### 11.7 Responsive Design Strategy
The application will use a mobile-first approach with responsive breakpoints:

```css
/* Base breakpoints */
--breakpoint-sm: 640px;
--breakpoint-md: 768px;
--breakpoint-lg: 1024px;
--breakpoint-xl: 1280px;
--breakpoint-2xl: 1536px;

/* Media query mixins */
@mixin sm {
  @media (min-width: 640px) { @content; }
}

@mixin md {
  @media (min-width: 768px) { @content; }
}

@mixin lg {
  @media (min-width: 1024px) { @content; }
}

@mixin xl {
  @media (min-width: 1280px) { @content; }
}
```

The layout will adapt to different screen sizes:
- Mobile: Stacked, single-column layout
- Tablet: Two-column layout with collapsible sidebar
- Desktop: Full layout with persistent sidebar and multi-column content#### 10.1 Technology Selection Rationale
- React: Chosen for its robust ecosystem and component-based architecture
- Supabase: Selected for its comprehensive backend capabilities including PostgreSQL database, authentication, storage, and real-time features with direct integrations into AI coding platforms
- Node.js/Express: Provides a JavaScript-based backend for custom API endpoints and business logic that may not be handled directly by Supabase# Technical Requirements Document (TRD)
## Trading Plan Management System

### 1. System Architecture

#### 1.1 Architecture Overview
The Trading Plan Management System (TPMS) will be built using a modern client-server architecture:
- **Frontend**: React.js single-page application (SPA)
- **Backend**: Node.js with Express.js RESTful API
- **Database**: Supabase (PostgreSQL with real-time capabilities)
- **State Management**: Redux for client-side state management
- **Authentication**: Supabase Authentication with JWT
- **Storage**: Supabase Storage for file uploads

#### 1.2 Component Diagram
```
┌─────────────────────────────────────────────────────────────┐
│                     Client Application                       │
│  ┌────────────┐  ┌────────────┐  ┌────────────────────────┐ │
│  │  React UI  │  │   Redux    │  │  Service Layer (Axios) │ │
│  └────────────┘  └────────────┘  └────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                      Server Application                      │
│  ┌────────────┐  ┌────────────┐  ┌────────────────────────┐ │
│  │  Express   │  │ Controllers│  │    Service Layer       │ │
│  └────────────┘  └────────────┘  └────────────────────────┘ │
│                                                             │
│  ┌────────────┐  ┌────────────┐  ┌────────────────────────┐ │
│  │   Models   │  │ Middleware │  │      Utilities         │ │
│  └────────────┘  └────────────┘  └────────────────────────┘ │
└─────────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────────┐
│                        Database                             │
│                        Supabase                             │
│       (PostgreSQL + Real-time + Auth + Storage)            │
└─────────────────────────────────────────────────────────────┘
```

#### 1.3 Deployment Architecture
- Frontend: Hosted on Vercel or Netlify
- Backend: Deployed on Heroku, AWS Elastic Beanstalk, or Supabase Edge Functions
- Database: Supabase (managed PostgreSQL)

### 2. Technology Stack

#### 2.1 Frontend
- **Framework**: React.js
- **State Management**: Redux with Redux Toolkit
- **UI Library**: Tailwind CSS with custom component library
- **Component Framework**: Headless UI or Radix UI for accessible components
- **Form Handling**: Formik with Yup validation
- **HTTP Client**: Axios
- **Charts**: Chart.js, D3.js, or Recharts
- **Date Handling**: date-fns
- **Table Management**: TanStack Table (React Table)
- **Routing**: React Router
- **Speech Recognition**: Web Speech API or react-speech-recognition
- **Voice Synthesis**: Web Speech API or custom solution for LLM voice responses
- **AI Chat Interface**: Custom UI components with streaming response support
- **Theme Management**: Custom theming solution with CSS variables
- **Framework**: React.js
- **State Management**: Redux with Redux Toolkit
- **UI Library**: Material-UI or Tailwind CSS
- **Form Handling**: Formik with Yup validation
- **HTTP Client**: Axios
- **Charts**: Chart.js or D3.js
- **Date Handling**: date-fns
- **Table Management**: react-table
- **Routing**: React Router

#### 2.2 Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Authentication**: Supabase Auth
- **Validation**: Joi or express-validator
- **Database Access**: Supabase JS Client
- **Logging**: Winston
- **API Documentation**: Swagger/OpenAPI
- **Testing**: Jest, Supertest
- **LLM Integration**: OpenAI Node.js SDK, Anthropic SDK, Google AI SDK
- **API Proxy**: Custom middleware for LLM API request handling and caching

#### 2.3 Database
- **Database**: Supabase (PostgreSQL)
- **Real-time Features**: Supabase Realtime
- **File Storage**: Supabase Storage
- **Authentication**: Supabase Auth
- **Backup Solution**: Automated daily backups via Supabase

#### 2.4 DevOps
- **CI/CD**: GitHub Actions
- **Containerization**: Docker (optional)
- **Code Quality**: ESLint, Prettier
- **Version Control**: Git/GitHub

### 3. Database Schema

The following represents the PostgreSQL tables in Supabase:

#### 3.1 users Table
```sql
CREATE TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  first_name TEXT,
  last_name TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  settings JSONB DEFAULT '{
    "theme": "light",
    "notifications": true,
    "defaultCurrency": "USD",
    "aiSettings": {
      "preferredProvider": "openai",
      "useVoiceInteraction": false,
      "voiceSettings": {
        "speed": 1,
        "pitch": 1,
        "voice": "default"
      }
    }
  }'::jsonb
);

-- Separate table for API keys with RLS policies
CREATE TABLE user_api_keys (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  provider TEXT NOT NULL,
  encrypted_key TEXT NOT NULL,
  provider_details JSONB,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3.2 ai_conversations Table
```sql
CREATE TABLE ai_conversations (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  title TEXT,
  provider TEXT,
  model TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  context JSONB DEFAULT '{}'::jsonb,
  tags TEXT[]
);

CREATE TABLE ai_messages (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  conversation_id UUID REFERENCES ai_conversations(id) ON DELETE CASCADE,
  role TEXT NOT NULL,
  content TEXT NOT NULL,
  timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  is_voice BOOLEAN DEFAULT FALSE
);
```

#### 3.3 ai_prompt_templates Table
```sql
CREATE TABLE ai_prompt_templates (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  title TEXT NOT NULL,
  description TEXT,
  system_prompt TEXT,
  user_prompt TEXT,
  context_type TEXT,
  is_default BOOLEAN DEFAULT FALSE,
  category TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3.1 User Collection
```json
{
  "_id": "ObjectId",
  "email": "String (unique, required)",
  "passwordHash": "String (required)",
  "firstName": "String",
  "lastName": "String",
  "createdAt": "Date",
  "updatedAt": "Date",
  "settings": {
    "theme": "String (light/dark)",
    "notifications": "Boolean",
    "defaultCurrency": "String",
    "aiSettings": {
      "preferredProvider": "String (openai/anthropic/google/deepseek)",
      "useVoiceInteraction": "Boolean",
      "voiceSettings": {
        "speed": "Number",
        "pitch": "Number",
        "voice": "String"
      }
    }
  },
  "apiKeys": {
    "openai": {
      "key": "String (encrypted)",
      "organization": "String (optional)"
    },
    "anthropic": {
      "key": "String (encrypted)"
    },
    "google": {
      "key": "String (encrypted)"
    },
    "deepseek": {
      "key": "String (encrypted)"
    },
    "mistral": {
      "key": "String (encrypted)"
    },
    "custom": [{
      "name": "String",
      "endpoint": "String",
      "key": "String (encrypted)",
      "headers": "Object"
    }]
  }
}
```

#### 3.4 trading_plans Table
```sql
CREATE TABLE trading_plans (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  version INTEGER DEFAULT 1,
  is_active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  questionnaire JSONB,
  concepts JSONB,
  rules JSONB,
  processes JSONB
);
```

#### 3.5 market_analyses Table
```sql
CREATE TABLE market_analyses (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  date DATE DEFAULT CURRENT_DATE,
  long_term_trend JSONB,
  intermediate_term_trend JSONB,
  short_term_trend JSONB,
  chart_images TEXT[],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3.6 watchlists Table
```sql
CREATE TABLE watchlists (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  name TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE watchlist_items (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  watchlist_id UUID REFERENCES watchlists(id) ON DELETE CASCADE,
  symbol TEXT NOT NULL,
  asset_type TEXT,
  added_date TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  notes TEXT,
  potential_entry DECIMAL,
  potential_stop_loss DECIMAL,
  potential_target DECIMAL,
  risk_reward_ratio DECIMAL,
  bias TEXT,
  time_frame TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3.7 positions Table
```sql
CREATE TABLE positions (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  watchlist_item_id UUID REFERENCES watchlist_items(id) ON DELETE SET NULL,
  symbol TEXT NOT NULL,
  asset_type TEXT,
  direction TEXT CHECK (direction IN ('long', 'short')),
  status TEXT CHECK (status IN ('planned', 'open', 'closed')),
  entry_date TIMESTAMP WITH TIME ZONE,
  entry_price DECIMAL,
  quantity DECIMAL,
  initial_stop_loss DECIMAL,
  current_stop_loss DECIMAL,
  exit_date TIMESTAMP WITH TIME ZONE,
  exit_price DECIMAL,
  profit_loss DECIMAL,
  profit_loss_percentage DECIMAL,
  notes TEXT,
  tags TEXT[],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE position_targets (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  position_id UUID REFERENCES positions(id) ON DELETE CASCADE,
  price DECIMAL NOT NULL,
  percentage DECIMAL,
  hit_date TIMESTAMP WITH TIME ZONE,
  status TEXT CHECK (status IN ('pending', 'hit', 'missed')),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE position_attachments (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  position_id UUID REFERENCES positions(id) ON DELETE CASCADE,
  file_path TEXT NOT NULL,
  file_type TEXT,
  description TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3.8 journals Table
```sql
CREATE TABLE journals (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  date DATE DEFAULT CURRENT_DATE,
  title TEXT,
  content TEXT,
  mood TEXT,
  market_conditions TEXT,
  lessons TEXT[],
  images TEXT[],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE journal_position_links (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  journal_id UUID REFERENCES journals(id) ON DELETE CASCADE,
  position_id UUID REFERENCES positions(id) ON DELETE CASCADE,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3.9 process_variances Table
```sql
CREATE TABLE process_variances (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  date DATE DEFAULT CURRENT_DATE,
  position_id UUID REFERENCES positions(id) ON DELETE SET NULL,
  planned_process TEXT,
  actual_process TEXT,
  reason TEXT,
  outcome TEXT,
  lesson_learned TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

#### 3.10 backtests Table
```sql
CREATE TABLE backtests (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id) ON DELETE CASCADE,
  strategy_name TEXT,
  time_frame TEXT,
  asset_universe TEXT[],
  start_date DATE,
  end_date DATE,
  market_conditions TEXT,
  summary JSONB,
  chart_images TEXT[],
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

CREATE TABLE backtest_results (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  backtest_id UUID REFERENCES backtests(id) ON DELETE CASCADE,
  symbol TEXT,
  entry_date DATE,
  entry_price DECIMAL,
  entry_reason TEXT,
  exit_date DATE,
  exit_price DECIMAL,
  exit_reason TEXT,
  profit_loss DECIMAL,
  profit_loss_percentage DECIMAL,
  successful BOOLEAN,
  notes TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);
```

### 4. API and Data Access

Supabase provides multiple ways to interact with the data:

#### 4.1 Supabase JavaScript Client
The primary method of interacting with the database will be through the Supabase JavaScript client:

```javascript
// Authentication
const { user, error } = await supabase.auth.signUp({
  email: 'user@example.com',
  password: 'password123'
});

// Database operations
const { data, error } = await supabase
  .from('trading_plans')
  .select('*')
  .eq('user_id', user.id);
```

#### 4.2 REST API
Supabase automatically generates REST endpoints for all tables, which can be used directly:

```
POST /rest/v1/trading_plans
GET /rest/v1/trading_plans?user_id=eq.{user_id}
PATCH /rest/v1/trading_plans?id=eq.{id}
DELETE /rest/v1/trading_plans?id=eq.{id}
```

#### 4.3 Real-time Subscriptions
The application will use Supabase's real-time capabilities for live updates:

```javascript
// Subscribe to changes in the positions table
const positionsSubscription = supabase
  .from('positions')
  .on('*', payload => {
    console.log('Change received!', payload);
    // Update UI
  })
  .subscribe();
```

#### 4.4 Custom API Endpoints
For complex operations that require additional business logic, the application will use Express.js API endpoints:

```
POST /api/ai/chat                      - Send message to AI and get response
POST /api/ai/speech-to-text            - Convert voice to text
POST /api/ai/text-to-speech            - Convert text to voice audio
POST /api/positions/{id}/analyze       - Run AI analysis on a position
POST /api/trading-plans/generate       - Generate trading plan with AI assistance
GET /api/performance/summary           - Get performance analytics
```
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user
- `POST /api/auth/logout` - Logout user
- `GET /api/auth/me` - Get current user
- `PUT /api/auth/password` - Update password

#### 4.3 Trading Plan API
- `GET /api/trading-plans` - Get all trading plans
- `GET /api/trading-plans/:id` - Get specific trading plan
- `POST /api/trading-plans` - Create new trading plan
- `PUT /api/trading-plans/:id` - Update trading plan
- `DELETE /api/trading-plans/:id` - Delete trading plan
- `POST /api/trading-plans/:id/duplicate` - Duplicate trading plan

#### 4.4 Market Analysis API
- `GET /api/market-analysis` - Get all market analyses
- `GET /api/market-analysis/:id` - Get specific market analysis
- `POST /api/market-analysis` - Create new market analysis
- `PUT /api/market-analysis/:id` - Update market analysis
- `DELETE /api/market-analysis/:id` - Delete market analysis

#### 4.5 Watchlist API
- `GET /api/watchlists` - Get all watchlists
- `GET /api/watchlists/:id` - Get specific watchlist
- `POST /api/watchlists` - Create new watchlist
- `PUT /api/watchlists/:id` - Update watchlist
- `DELETE /api/watchlists/:id` - Delete watchlist
- `POST /api/watchlists/:id/items` - Add item to watchlist
- `PUT /api/watchlists/:id/items/:itemId` - Update watchlist item
- `DELETE /api/watchlists/:id/items/:itemId` - Remove item from watchlist

#### 4.6 Position API
- `GET /api/positions` - Get all positions
- `GET /api/positions/:id` - Get specific position
- `POST /api/positions` - Create new position
- `PUT /api/positions/:id` - Update position
- `DELETE /api/positions/:id` - Delete position
- `PUT /api/positions/:id/close` - Close position
- `POST /api/positions/:id/attach` - Attach file to position

#### 4.7 Journal API
- `GET /api/journals` - Get all journal entries
- `GET /api/journals/:id` - Get specific journal entry
- `POST /api/journals` - Create new journal entry
- `PUT /api/journals/:id` - Update journal entry
- `DELETE /api/journals/:id` - Delete journal entry

#### 4.8 Process Variance API
- `GET /api/process-variances` - Get all process variances
- `GET /api/process-variances/:id` - Get specific process variance
- `POST /api/process-variances` - Create new process variance
- `PUT /api/process-variances/:id` - Update process variance
- `DELETE /api/process-variances/:id` - Delete process variance

#### 4.9 Backtesting API
- `GET /api/backtests` - Get all backtests
- `GET /api/backtests/:id` - Get specific backtest
- `POST /api/backtests` - Create new backtest
- `PUT /api/backtests/:id` - Update backtest
- `DELETE /api/backtests/:id` - Delete backtest

#### 4.10 Upload API
- `POST /api/upload` - Upload file and get URL
- `DELETE /api/upload/:id` - Delete uploaded file

### 5. Security Requirements

#### 5.1 Authentication
- Supabase Auth with JWT tokens
- Social authentication options (Google, GitHub)
- Email verification
- Password reset functionality
- Rate limiting for login attempts
- Session management

#### 5.2 Authorization
- Row-Level Security (RLS) policies in Supabase
- Example RLS policy for trading plans:
  ```sql
  CREATE POLICY "Users can only view their own trading plans"
  ON trading_plans
  FOR SELECT
  USING (auth.uid() = user_id);
  ```
- Role-based access for multi-user environments
- Middleware validation for custom API endpoints

#### 5.3 Data Protection
- Encrypted API key storage using Supabase's pgcrypto extension
- API key encryption example:
  ```sql
  CREATE EXTENSION IF NOT EXISTS pgcrypto;
  
  -- Function to encrypt API keys
  CREATE OR REPLACE FUNCTION encrypt_api_key(key_text TEXT, secret TEXT)
  RETURNS TEXT AS $
  BEGIN
    RETURN encode(encrypt(key_text::bytea, secret::bytea, 'aes')::text, 'base64');
  END;
  $ LANGUAGE plpgsql SECURITY DEFINER;
  ```
- XSS protection in frontend
- CSRF protection for custom endpoints
- Secure headers implementation

#### 5.4 Auditing
- Activity logging using Supabase's built-in logging
- Custom audit log table:
  ```sql
  CREATE TABLE audit_logs (
    id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
    user_id UUID REFERENCES users(id),
    action TEXT NOT NULL,
    table_name TEXT,
    record_id UUID,
    details JSONB,
    ip_address TEXT,
    user_agent TEXT,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
  );
  ```
- Database function triggers for critical actions

### 6. Performance Requirements

#### 6.1 Response Times
- API response time < 200ms for standard operations
- Dashboard loading time < 1 second
- Chart rendering < 500ms

#### 6.2 Scalability
- Support for up to 1000 concurrent users
- Database designed to efficiently handle large datasets
- Pagination for all list endpoints
- Caching strategy for frequently accessed data

#### 6.3 Resource Utilization
- Optimized bundle size with code splitting
- Lazy loading for non-critical components
- Image optimization
- Database indexing for common queries

### 7. Test Requirements

#### 7.1 Unit Testing
- Jest for React components and utilities
- Mocha/Chai for backend services
- 80% code coverage minimum

#### 7.2 Integration Testing
- API endpoint testing with Supertest
- Database operation testing

#### 7.3 E2E Testing
- Cypress for critical user flows
- Testing across supported browsers

#### 7.4 Performance Testing
- Load testing with Artillery or k6
- Stress testing for specific high-load scenarios

### 8. Deployment and DevOps

#### 8.1 CI/CD Pipeline
- GitHub Actions workflow for automated testing
- Automatic deployment to staging on PR merge
- Manual promotion to production

#### 8.2 Environment Configuration
- Environment variable management
- Secrets management
- Configuration validation on startup

#### 8.3 Monitoring
- Error tracking with Sentry
- Performance monitoring
- Server health checks
- Uptime monitoring

#### 8.4 Backup and Recovery
- Daily database backups
- Point-in-time recovery capability
- Disaster recovery plan

### 9. Implementation Plan

#### 9.1 Phase 1: Core Infrastructure (Weeks 1-2)
- Project setup and repository creation
- Authentication system implementation
- Database schema creation
- Core API endpoints

#### 9.2 Phase 2: Trading Plan Module (Weeks 3-4)
- Trading plan questionnaire UI
- Trading plan CRUD operations
- Trading plan versioning

#### 9.3 Phase 3: Market Analysis & Watchlist (Weeks 5-6)
- Market analysis form and visualization
- Watchlist management
- Basic charting functionality

#### 9.4 Phase 4: Position Management (Weeks 7-8)
- Position entry/exit forms
- Portfolio dashboard
- Risk management calculations

#### 9.5 Phase 5: Analysis Tools (Weeks 9-10)
- Journal implementation
- Process variance tracking
- Post-trade analysis

#### 9.6 Phase 6: Backtesting & Simulation (Weeks 11-12)
- Visual backtesting module
- Trade simulation tools
- Performance reporting

#### 9.7 Phase 7: AI Integration (Weeks 13-14)
- LLM provider integrations
- Voice interaction capabilities
- AI prompt templates
- Context-aware AI assistant functionality
- Conversation storage and management

#### 9.8 Phase 8: Testing & Polish (Weeks 15-16)
- Comprehensive testing
- UI/UX refinement
- Performance optimization
- AI feature usability testing

### 10. Appendix

#### 10.1 Technology Selection Rationale
- React: Chosen for its robust ecosystem and component-based architecture
- MongoDB: Selected for its flexibility with document-based structures that match the varying templates
- Node.js/Express: Provides a JavaScript-based full-stack solution for easier development

#### 10.2 AI Integration Architecture

The AI integration will follow a client-server architecture with these components:

1. **Frontend Components**:
   - AI Chat Widget (present throughout the application)
   - Voice Recording and Playback Interface
   - Provider Selection and Configuration UI
   - Context Injection System (to provide LLMs with relevant app state)

2. **Backend Services**:
   - LLM Provider Proxy Service (handles API key security and request routing)
   - Context Builder Service (constructs context-aware prompts)
   - Conversation Management Service
   - Voice Processing Service

3. **Security Considerations**:
   - Encrypted API key storage
   - Rate limiting for API requests
   - Usage tracking and quota management

4. **Performance Optimization**:
   - Streaming responses for real-time feedback
   - Client-side caching of conversations
   - Optimized prompt construction for context efficiency

#### 10.3 Future Considerations
- Mobile application development
- Brokerage API integrations
- Advanced analytics and machine learning
- Community features for sharing trading plans and analyses
- Custom fine-tuned LLMs specifically for trading analysis
