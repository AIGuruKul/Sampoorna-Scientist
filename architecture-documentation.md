# Full Stack Architecture Documentation for LLM Applications

## Table of Contents
1. [Introduction](#introduction)
2. [Overall Architecture](#overall-architecture)
3. [PART 1: Data Science & LLM Engineering](#part-1-data-science--llm-engineering)
4. [PART 2: Backend Engineering](#part-2-backend-engineering)
5. [PART 3: Frontend Engineering](#part-3-frontend-engineering)
6. [PART 4: Database Engineering](#part-4-database-engineering)
7. [Cross-Cutting Concerns](#cross-cutting-concerns)
8. [Conclusion](#conclusion)

## Introduction

This documentation outlines the architecture for developing a full-stack application that incorporates Large Language Models (LLMs), using AWS/GCP cloud services, Supabase for database management, and FastAPI for the backend framework. The architecture is designed to be scalable, secure, and maintainable while optimizing for performance and cost efficiency.

## Overall Architecture

### High-Level Architecture Diagram

```
┌────────────────┐     ┌─────────────────────┐     ┌───────────────────┐
│                │     │                     │     │                   │
│    Frontend    │◄────┤      Backend        │◄────┤   LLM Services    │
│                │     │     (FastAPI)       │     │                   │
└───────┬────────┘     └─────────┬───────────┘     └───────────────────┘
        │                        │
        │                        │
┌───────▼────────┐     ┌─────────▼───────────┐
│                │     │                     │
│   Static Files │     │ Supabase Database   │
│   (S3/GCS)     │     │                     │
└────────────────┘     └─────────────────────┘
```

### Architecture Visualization Tools

For creating and maintaining architecture diagrams, we recommend the following tools:

1. **Excalidraw**
   - Lightweight, browser-based diagramming tool
   - Collaborative features for team diagramming sessions
   - Simple, sketch-like aesthetics that are perfect for ideation
   - Export options to PNG, SVG, and JSON formats
   - Best for: Quick architecture sketches, whiteboarding sessions, and initial design concepts

2. **draw.io (diagrams.net)**
   - Comprehensive diagramming tool with extensive shape libraries
   - Integration with version control systems (GitHub, GitLab)
   - Support for cloud storage platforms (Google Drive, OneDrive)
   - Export to multiple formats (PNG, SVG, PDF, HTML)
   - Best for: Detailed architecture diagrams, network diagrams, and flow charts
   
3. **Best Practices for Architecture Diagramming**
   - Use consistent notation (preferably C4 model or UML)
   - Create diagrams at multiple levels of abstraction (system context, container, component)
   - Include clear labels and descriptions
   - Version control your diagrams alongside code
   - Update diagrams as architecture evolves

---

# PART 1: Data Science & LLM Engineering

## LLM Service Architecture

### Components
1. **Model Management**
   - Model selection and versioning
   - Model deployment strategies (hosted API, self-hosted)
   - Model evaluation and monitoring
   - A/B testing framework

2. **Prompt Engineering**
   - Prompt template management
   - Prompt versioning and optimization
   - System prompts and user prompt handling
   - Prompt validation and sanitization

3. **Inference Pipeline**
   - Request preprocessing
   - Token management and optimization
   - Response post-processing
   - Streaming support

4. **LLM Orchestration**
   - Model routing based on task complexity
   - Fallback mechanisms
   - Chaining and multi-step reasoning
   - Tool usage integration

### Integration Methods
1. **Direct API Integration**
   - Connecting to commercial LLM providers (OpenAI, Anthropic, etc.)
   - API key management and rotation
   - Rate limiting and quota management
   - Vendor-specific optimizations

2. **Self-hosted Models**
   - Model deployment on AWS/GCP resources
   - Containerized model serving (Docker, Kubernetes)
   - Quantization and optimization
   - Hardware acceleration (GPU/TPU)

3. **Serverless Functions**
   - AWS Lambda or GCP Cloud Functions for LLM processing
   - Request batching and throttling
   - Cold start optimization
   - Memory and timeout configuration

### Best Practices

1. **Prompt Engineering**
   ```
   // Example template structure
   {
     "system_prompt": "You are a helpful assistant that...",
     "user_template": "USER: {{user_input}}\nCONTEXT: {{context}}",
     "assistant_template": "ASSISTANT: ",
     "metadata": {
       "version": "1.2",
       "author": "AI Team",
       "use_case": "Customer Support"
     }
   }
   ```

   - Store prompts as versioned templates
   - Implement systematic prompt testing
   - Document prompt patterns and their effectiveness
   - Use templating systems for dynamic prompt generation

2. **Model Management**
   - Track model performance metrics (accuracy, latency, cost)
   - Implement canary deployments for new models
   - Create model registries with metadata
   - Develop model documentation standards

3. **Response Processing**
   - Implement content filtering and safety measures
   - Use structured outputs (JSON mode) when appropriate
   - Process and validate responses before returning to users
   - Handle hallucinations and incorrect outputs

4. **Caching Strategy**
   - Cache common LLM responses to reduce latency and costs
   - Implement intelligent cache invalidation
   - Use vector similarity for approximate matching
   - Store response metadata with cache entries

5. **Cost and Performance Optimization**
   - Implement token counting and budget controls
   - Use appropriate model sizes based on task complexity
   - Batch similar requests when possible
   - Monitor token usage trends and optimize high-usage patterns

## LLM Infrastructure

### AWS Infrastructure for LLM
1. **Compute Options**
   - SageMaker for model hosting
   - EC2 with GPU instances for custom deployments
   - Lambda for serverless inference
   - ECS/EKS for containerized model serving

2. **Supporting Services**
   - ElastiCache for response caching
   - SQS for request queueing
   - CloudWatch for monitoring and logging
   - S3 for model artifact storage

### GCP Infrastructure for LLM
1. **Compute Options**
   - Vertex AI for model hosting and management
   - Compute Engine with GPU/TPU instances
   - Cloud Run for containerized model serving
   - Cloud Functions for serverless inference

2. **Supporting Services**
   - Memorystore for response caching
   - Pub/Sub for request queueing
   - Cloud Monitoring for performance tracking
   - Cloud Storage for model artifact storage

### Vector Database Integration
1. **Options**
   - Pinecone for dedicated vector search
   - PostgreSQL with pgvector extension (via Supabase)
   - Milvus or Weaviate for open-source vector DB
   - Redis with vector search capabilities

2. **Architecture Patterns**
   - Embeddings generation pipeline
   - Vector indexing and retrieval service
   - Hybrid search (keyword + semantic)
   - Document chunking strategies

## LLM-Specific Monitoring
1. **Key Metrics**
   - Token usage by model
   - Response latency
   - Error rates and types
   - User satisfaction metrics
   - Hallucination detection rates

2. **Logging Requirements**
   - Prompt templates used (non-PII)
   - Model versions
   - Token counts (input/output)
   - Processing times
   - Correlation IDs for request tracing

3. **Dashboard Components**
   - Token usage trends
   - Cost projections
   - Model performance comparisons
   - User feedback aggregation
   - Error clustering and analysis

---

# PART 2: Backend Engineering

## Backend Architecture

### Technologies
- **Framework**: FastAPI for high-performance asynchronous API
- **Authentication**: OAuth2 with JWT
- **Validation**: Pydantic for data validation
- **API Documentation**: Automatic Swagger/OpenAPI documentation
- **Background Tasks**: Celery or FastAPI background tasks

### API Design

1. **RESTful Endpoints**
   ```
   /api/v1/conversations                # Collection endpoints
   /api/v1/conversations/{id}           # Resource endpoints
   /api/v1/conversations/{id}/messages  # Sub-resource endpoints
   ```

2. **GraphQL API** (Optional alternative)
   ```graphql
   type Conversation {
     id: ID!
     title: String
     messages: [Message!]!
     createdAt: DateTime!
   }
   
   type Query {
     conversation(id: ID!): Conversation
     conversations: [Conversation!]!
   }
   ```

### Code Organization
```
backend/
├── app/
│   ├── api/
│   │   ├── endpoints/
│   │   │   ├── auth.py
│   │   │   ├── conversations.py
│   │   │   ├── llm.py
│   │   │   └── users.py
│   │   ├── dependencies.py
│   │   └── router.py
│   ├── core/
│   │   ├── config.py
│   │   ├── security.py
│   │   └── logging.py
│   ├── db/
│   │   ├── models.py
│   │   └── repositories.py
│   ├── services/
│   │   ├── llm_service.py
│   │   ├── embedding_service.py
│   │   └── user_service.py
│   ├── schemas/
│   │   ├── conversation.py
│   │   ├── message.py
│   │   └── user.py
│   └── main.py
├── tests/
│   ├── api/
│   ├── services/
│   └── conftest.py
├── alembic/
│   └── versions/
├── Dockerfile
└── requirements.txt
```

### Best Practices
1. **API Design**:
   - Follow RESTful principles for resource management
   - Use versioned endpoints (e.g., `/api/v1/resource`)
   - Implement comprehensive error handling with meaningful status codes
   - Structure responses consistently with standardized formats

2. **Dependency Injection**:
   ```python
   # dependencies.py
   from fastapi import Depends
   from sqlalchemy.orm import Session
   
   from app.db.session import get_db
   from app.services.llm_service import LLMService
   
   def get_llm_service(db: Session = Depends(get_db)) -> LLMService:
       return LLMService(db)
   ```

   - Use FastAPI's dependency injection system
   - Create reusable dependencies for common functionality
   - Implement scoped dependencies for request-level resources

3. **Environment Configuration**:
   ```python
   # config.py
   from pydantic import BaseSettings, PostgresDsn, SecretStr
   
   class Settings(BaseSettings):
       API_V1_STR: str = "/api/v1"
       PROJECT_NAME: str = "LLM Application"
       POSTGRES_SERVER: str
       POSTGRES_USER: str
       POSTGRES_PASSWORD: SecretStr
       POSTGRES_DB: str
       SQLALCHEMY_DATABASE_URI: PostgresDsn = None
       
       # LLM Config
       LLM_API_KEY: SecretStr
       LLM_MODEL_NAME: str = "gpt-4"
       LLM_MAX_TOKENS: int = 2048
       
       class Config:
           env_file = ".env"
   
   settings = Settings()
   ```

   - Use environment variables for configuration
   - Never hardcode sensitive information
   - Implement configuration validation with Pydantic

4. **Logging and Monitoring**:
   ```python
   # logging.py
   import logging
   import json
   from datetime import datetime
   
   class JSONFormatter(logging.Formatter):
       def format(self, record):
           log_record = {
               "timestamp": datetime.utcnow().isoformat(),
               "level": record.levelname,
               "message": record.getMessage(),
               "module": record.module,
               "function": record.funcName,
               "line": record.lineno
           }
           if hasattr(record, "correlation_id"):
               log_record["correlation_id"] = record.correlation_id
           return json.dumps(log_record)
   
   def setup_logging():
       handler = logging.StreamHandler()
       handler.setFormatter(JSONFormatter())
       logging.basicConfig(
           handlers=[handler],
           level=logging.INFO
       )
   ```

   - Implement structured logging
   - Use correlation IDs to track requests across services
   - Log appropriate information for debugging and auditing

5. **Error Handling**:
   ```python
   # errors.py
   from fastapi import HTTPException, Request
   from fastapi.responses import JSONResponse
   
   class LLMServiceError(Exception):
       def __init__(self, message: str, code: str = "llm_error"):
           self.message = message
           self.code = code
           super().__init__(self.message)
   
   async def llm_exception_handler(request: Request, exc: LLMServiceError):
       return JSONResponse(
           status_code=500,
           content={
               "error": {
                   "code": exc.code,
                   "message": exc.message,
                   "request_id": request.state.request_id
               }
           }
       )
   
   # In main.py
   app.add_exception_handler(LLMServiceError, llm_exception_handler)
   ```

   - Create custom exception classes
   - Implement global exception handlers
   - Return consistent error response formats
   - Include request IDs in error responses

## Backend Infrastructure

### AWS Infrastructure
1. **Compute Options**
   - ECS Fargate for containerized services
   - EC2 instances with Auto Scaling Groups
   - Elastic Beanstalk for simplified deployment
   - Lambda for serverless microservices

2. **Networking**
   - VPC with public and private subnets
   - Application Load Balancer for traffic distribution
   - API Gateway for API management
   - WAF for security and rate limiting

### GCP Infrastructure
1. **Compute Options**
   - Cloud Run for containerized services
   - Compute Engine with managed instance groups
   - App Engine for simplified deployment
   - Cloud Functions for serverless components

2. **Networking**
   - VPC network configuration
   - Cloud Load Balancing
   - API Gateway for API management
   - Cloud Armor for security

### Best Practices
1. **Infrastructure as Code (IaC)**:
   ```terraform
   # Example Terraform configuration for FastAPI service on ECS
   resource "aws_ecs_task_definition" "fastapi_task" {
     family                   = "fastapi-service"
     network_mode             = "awsvpc"
     requires_compatibilities = ["FARGATE"]
     cpu                      = 256
     memory                   = 512
     execution_role_arn       = aws_iam_role.ecs_execution_role.arn
     
     container_definitions = jsonencode([{
       name      = "fastapi-container"
       image     = "${aws_ecr_repository.fastapi_repo.repository_url}:latest"
       essential = true
       
       portMappings = [{
         containerPort = 8000
         hostPort      = 8000
         protocol      = "tcp"
       }]
       
       environment = [
         { name = "POSTGRES_SERVER", value = aws_rds_cluster.postgres.endpoint },
         { name = "POSTGRES_DB", value = "app" },
         { name = "POSTGRES_USER", value = "app_user" }
       ]
       
       secrets = [
         { name = "POSTGRES_PASSWORD", valueFrom = aws_secretsmanager_secret.db_password.arn },
         { name = "LLM_API_KEY", valueFrom = aws_secretsmanager_secret.llm_api_key.arn }
       ]
       
       logConfiguration = {
         logDriver = "awslogs"
         options = {
           "awslogs-group"         = aws_cloudwatch_log_group.fastapi_logs.name
           "awslogs-region"        = var.aws_region
           "awslogs-stream-prefix" = "fastapi"
         }
       }
     }])
   }
   ```

   - Define infrastructure as code with Terraform, AWS CDK, or Pulumi
   - Version control infrastructure definitions
   - Use modules for reusable components
   - Implement variable parameterization

---

# PART 3: Frontend Engineering

## Frontend Architecture

### Technologies
- **Framework**: React.js/Next.js/Vue.js for component-based UI development
- **State Management**: Redux, Zustand, or Context API based on application complexity
- **API Integration**: Axios or fetch for API calls with proper error handling
- **UI Components**: Material UI, Tailwind CSS, or custom component library
- **Authentication**: JWT with secure token management

### Application Structure
```
frontend/
├── public/
│   ├── assets/
│   └── index.html
├── src/
│   ├── api/
│   │   ├── axios.ts
│   │   ├── conversations.ts
│   │   └── llm.ts
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button.tsx
│   │   │   ├── Input.tsx
│   │   │   └── Modal.tsx
│   │   ├── conversation/
│   │   │   ├── ConversationList.tsx
│   │   │   ├── MessageBubble.tsx
│   │   │   └── MessageInput.tsx
│   │   └── layout/
│   │       ├── Header.tsx
│   │       ├── Sidebar.tsx
│   │       └── MainLayout.tsx
│   ├── contexts/
│   │   ├── AuthContext.tsx
│   │   └── ThemeContext.tsx
│   ├── hooks/
│   │   ├── useConversation.ts
│   │   ├── useLLM.ts
│   │   └── useDebounce.ts
│   ├── pages/
│   │   ├── Auth/
│   │   ├── Chat/
│   │   ├── Settings/
│   │   └── App.tsx
│   ├── state/
│   │   ├── slices/
│   │   └── store.ts
│   ├── types/
│   │   ├── conversation.ts
│   │   ├── message.ts
│   │   └── user.ts
│   ├── utils/
│   │   ├── formatting.ts
│   │   └── validation.ts
│   └── main.tsx
├── .eslintrc.js
├── tailwind.config.js
├── tsconfig.json
└── package.json
```

### Best Practices
1. **Component Structure**:
   ```tsx
   // Example component using atomic design principles
   // atoms/Button.tsx
   interface ButtonProps {
     variant: 'primary' | 'secondary';
     size: 'sm' | 'md' | 'lg';
     children: React.ReactNode;
     onClick?: () => void;
     disabled?: boolean;
   }
   
   export const Button: React.FC<ButtonProps> = ({
     variant = 'primary',
     size = 'md',
     children,
     onClick,
     disabled = false
   }) => {
     const baseClasses = "rounded font-medium transition-colors";
     const variantClasses = {
       primary: "bg-blue-600 text-white hover:bg-blue-700",
       secondary: "bg-gray-200 text-gray-800 hover:bg-gray-300"
     };
     const sizeClasses = {
       sm: "text-xs px-2 py-1",
       md: "text-sm px-3 py-2",
       lg: "text-base px-4 py-2"
     };
     
     return (
       <button
         className={`${baseClasses} ${variantClasses[variant]} ${sizeClasses[size]}`}
         onClick={onClick}
         disabled={disabled}
       >
         {children}
       </button>
     );
   };
   ```

   - Use atomic design principles (atoms, molecules, organisms, templates, pages)
   - Implement lazy loading for performance optimization
   - Keep components small and focused on single responsibilities
   - Use TypeScript for type safety

2. **State Management**:
   ```tsx
   // Using React Context for global state
   // contexts/ConversationContext.tsx
   interface ConversationContextType {
     conversations: Conversation[];
     activeConversation: Conversation | null;
     isLoading: boolean;
     error: string | null;
     setActiveConversation: (id: string) => void;
     createConversation: () => Promise<void>;
     sendMessage: (content: string) => Promise<void>;
   }
   
   export const ConversationContext = createContext<ConversationContextType | undefined>(undefined);
   
   export const ConversationProvider: React.FC<{children: React.ReactNode}> = ({ children }) => {
     const [conversations, setConversations] = useState<Conversation[]>([]);
     const [activeConversation, setActiveConversation] = useState<Conversation | null>(null);
     const [isLoading, setIsLoading] = useState(false);
     const [error, setError] = useState<string | null>(null);
     
     // Implement context methods...
     
     return (
       <ConversationContext.Provider value={{
         conversations,
         activeConversation,
         isLoading,
         error,
         setActiveConversation: (id) => {/* implementation */},
         createConversation: async () => {/* implementation */},
         sendMessage: async (content) => {/* implementation */}
       }}>
         {children}
       </ConversationContext.Provider>
     );
   };
   ```

   - Centralize application state for easier management
   - Implement proper error and loading states
   - Use local state for component-specific data
   - Consider performance optimizations (context splitting, memoization)

3. **API Integration**:
   ```tsx
   // api/axios.ts
   import axios from 'axios';
   
   const baseURL = process.env.REACT_APP_API_URL || 'http://localhost:8000/api/v1';
   
   const api = axios.create({
     baseURL,
     headers: {
       'Content-Type': 'application/json',
     },
   });
   
   // Request interceptor for auth token
   api.interceptors.request.use(
     (config) => {
       const token = localStorage.getItem('token');
       if (token) {
         config.headers.Authorization = `Bearer ${token}`;
       }
       return config;
     },
     (error) => Promise.reject(error)
   );
   
   // Response interceptor for error handling
   api.interceptors.response.use(
     (response) => response,
     (error) => {
       // Handle token expiration
       if (error.response?.status === 401) {
         localStorage.removeItem('token');
         window.location.href = '/login';
       }
       return Promise.reject(error);
     }
   );
   
   export default api;
   ```

   - Create a centralized API client
   - Implement request/response interceptors
   - Handle authentication and error cases
   - Use environment variables for configuration

4. **Performance**:
   - Implement code splitting with lazy loading
   ```tsx
   // Lazy loading routes
   const Settings = React.lazy(() => import('./pages/Settings'));
   
   function App() {
     return (
       <Suspense fallback={<div>Loading...</div>}>
         <Routes>
           <Route path="/settings" element={<Settings />} />
         </Routes>
       </Suspense>
     );
   }
   ```
   
   - Optimize renders with React.memo and useMemo
   - Implement virtualization for long lists
   - Use web workers for computationally intensive tasks

5. **User Experience**:
   - Design responsive layouts for all device sizes
   - Implement skeleton screens for loading states
   - Provide immediate feedback for user actions
   - Ensure accessibility compliance (WCAG guidelines)

## Frontend Infrastructure

### Static Hosting
1. **AWS Options**
   - S3 for static website hosting
   - CloudFront for CDN and edge caching
   - Route 53 for DNS management
   - Certificate Manager for SSL certificates

2. **GCP Options**
   - Cloud Storage for static website hosting
   - Cloud CDN for content delivery
   - Cloud DNS for domain management
   - Certificate Manager for SSL certificates

### Build and Deployment
1. **Build Process**
   ```yaml
   # Example GitHub Actions workflow for frontend deployment
   name: Deploy Frontend
   
   on:
     push:
       branches: [main]
       paths:
         - 'frontend/**'
   
   jobs:
     build-and-deploy:
       runs-on: ubuntu-latest
       
       steps:
         - uses: actions/checkout@v2
         
         - name: Set up Node.js
           uses: actions/setup-node@v2
           with:
             node-version: '16'
             
         - name: Install dependencies
           run: |
             cd frontend
             npm ci
             
         - name: Build
           run: |
             cd frontend
             npm run build
           env:
             REACT_APP_API_URL: ${{ secrets.API_URL }}
             
         - name: Deploy to S3
           uses: jakejarvis/s3-sync-action@master
           with:
             args: --acl public-read --follow-symlinks --delete
           env:
             AWS_S3_BUCKET: ${{ secrets.AWS_S3_BUCKET }}
             AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
             AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
             AWS_REGION: 'us-east-1'
             SOURCE_DIR: 'frontend/build'
             
         - name: Invalidate CloudFront
           uses: chetan/invalidate-cloudfront-action@v2
           env:
             DISTRIBUTION: ${{ secrets.CLOUDFRONT_DISTRIBUTION_ID }}
             PATHS: '/*'
             AWS_REGION: 'us-east-1'
             AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
             AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
   ```

---

# PART 4: Database Engineering

## Database Design with Supabase

### Technologies
- **Database**: PostgreSQL (provided by Supabase)
- **Authentication**: Supabase Auth
- **Realtime**: Supabase Realtime for live updates
- **Storage**: Supabase Storage for user-generated content

### Database Schema Design

#### Sample Schema for LLM-powered Application

```
// Users and Authentication
TABLE users (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  email TEXT UNIQUE NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  last_sign_in TIMESTAMP WITH TIME ZONE,
  metadata JSONB
);

// LLM Conversations
TABLE conversations (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  user_id UUID REFERENCES users(id),
  title TEXT,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  metadata JSONB
);

// Individual Messages in Conversations
TABLE messages (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  conversation_id UUID REFERENCES conversations(id) ON DELETE CASCADE,
  role TEXT NOT NULL CHECK (role IN ('user', 'assistant', 'system')),
  content TEXT NOT NULL,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  tokens_used INTEGER,
  model_used TEXT
);

// User Preferences
TABLE user_preferences (
  user_id UUID PRIMARY KEY REFERENCES users(id) ON DELETE CASCADE,
  default_model TEXT,
  theme TEXT DEFAULT 'light',
  settings JSONB
);

// LLM Model Performance Metrics
TABLE model_metrics (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  model_name TEXT NOT NULL,
  timestamp TIMESTAMP WITH TIME ZONE DEFAULT NOW(),
  latency_ms INTEGER NOT NULL,
  tokens_input INTEGER NOT NULL,
  tokens_output INTEGER NOT NULL,
  success BOOLEAN NOT NULL,
  error_message TEXT
);

// Vector Embeddings
TABLE documents (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  content TEXT NOT NULL,
  metadata JSONB,
  user_id UUID REFERENCES users(id),
  created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW()
);

TABLE document_embeddings (
  id UUID PRIMARY KEY DEFAULT uuid_generate_v4(),
  document_id UUID REFERENCES documents(id) ON DELETE CASCADE,
  embedding VECTOR(1536), -- Using pgvector extension
  chunk_index INTEGER,
  chunk_content TEXT
);

// Create index for vector similarity search
CREATE INDEX ON document_embeddings USING ivfflat (embedding vector_cosine_ops);
```

#### Schema Design Tools
- **Supabase Schema Editor**: Visual interface for schema management
- **dbdiagram.io**: Create and visualize database schemas
- **DrawSQL**: Collaborative SQL schema visualization tool
- **Schema Migration Files**: Version-controlled SQL scripts
- **PostgreSQL ERD Tools**: pgAdmin, DBeaver with ERD visualization

### Database Access Patterns

1. **Supabase Client Setup**
   ```typescript
   // supabase.ts
   import { createClient } from '@supabase/supabase-js';
   
   const supabaseUrl = process.env.SUPABASE_URL!;
   const supabaseKey = process.env.SUPABASE_ANON_KEY!;
   
   export const supabase = createClient(supabaseUrl, supabaseKey);
   ```

2. **Repository Pattern**
   ```typescript
   // repositories/conversationRepository.ts
   import { supabase } from '../supabase';
   import { Conversation, Message } from '../types';
   
   export const ConversationRepository = {
     async getByUserId(userId: string): Promise<Conversation[]> {
       const { data, error } = await supabase
         .from('conversations')
         .select('*')
         .eq('user_id', userId)
         .order('updated_at', { ascending: false });
       
       if (error) throw error;
       return data || [];
     },
     
     async getWithMessages(conversationId: string): Promise<Conversation & { messages: Message[] }> {
       // Get conversation
       const { data: conversation, error: convError } = await supabase
         .from('conversations')
         .select('*')
         .eq('id', conversationId)
         .single();
       
       if (convError) throw convError;
       
       // Get messages
       const { data: messages, error: msgError } = await supabase
         .from('messages')
         .select('*')
         .eq('conversation_id', conversationId)
         .order('created_at', { ascending: true });
       
       if (msgError) throw msgError;
       
       return {
         ...conversation,
         messages: messages || []
       };
     },
     
     // Additional methods...
   };
   ```

3. **Row-Level Security**
   ```sql
   -- Enable RLS
   ALTER TABLE conversations ENABLE ROW LEVEL SECURITY;
   
   -- Create policies
   CREATE POLICY "Users can view their own conversations"
     ON conversations
     FOR SELECT
     USING (auth.uid() = user_id);
   
   CREATE POLICY "Users can insert their own conversations"
     ON conversations
     FOR INSERT
     WITH CHECK (auth.uid() = user_id);
   
   CREATE POLICY "Users can update their own conversations"
     ON conversations
     FOR UPDATE
     USING (auth.uid() = user_id);
   ```

