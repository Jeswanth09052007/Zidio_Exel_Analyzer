# ExcelAI - AI-Powered Excel Analysis Platform

## Overview

ExcelAI is a full-stack web application that allows users to upload Excel files and receive AI-powered analysis, insights, and recommendations. The application features a modern React frontend with shadcn/ui components, an Express.js backend, PostgreSQL database with Drizzle ORM, and integrates with OpenAI for intelligent data analysis.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture
- **Framework**: React 18 with TypeScript
- **Routing**: Wouter (lightweight client-side routing)
- **State Management**: TanStack Query (React Query) for server state
- **UI Components**: shadcn/ui built on Radix UI primitives
- **Styling**: Tailwind CSS with CSS variables for theming
- **Build Tool**: Vite with hot module replacement

### Backend Architecture
- **Framework**: Express.js with TypeScript
- **Runtime**: Node.js with ES modules
- **API Style**: RESTful endpoints
- **File Processing**: Multer for file uploads, XLSX for Excel parsing
- **AI Integration**: OpenAI API for data analysis

### Database Architecture
- **Database**: PostgreSQL (configured for Neon serverless)
- **ORM**: Drizzle ORM with type-safe queries
- **Migrations**: Drizzle Kit for schema management
- **Connection**: Connection pooling with @neondatabase/serverless

### Authentication & Session Management
- **Provider**: Replit OIDC authentication
- **Session Storage**: PostgreSQL-backed sessions with connect-pg-simple
- **Security**: HTTP-only cookies with secure flag in production

## Key Components

### Core Entities
1. **Users**: Managed through Replit Auth with profile information
2. **Files**: Excel file metadata and upload tracking
3. **Analyses**: AI-generated insights and recommendations for uploaded files

### File Processing Pipeline
1. **Upload**: Multer handles multipart file uploads with size limits (50MB)
2. **Validation**: File type checking for Excel formats (.xlsx, .xls)
3. **Parsing**: XLSX library extracts data from all sheets
4. **AI Analysis**: OpenAI processes data sample and generates insights
5. **Storage**: Analysis results stored in database for retrieval

### Frontend Components
- **Landing Page**: Marketing page with feature highlights and sign-in
- **Dashboard**: Main application interface with file management
- **File Upload Modal**: Drag-and-drop interface with upload progress
- **File Summary Modal**: Displays AI analysis results and insights
- **Responsive Design**: Mobile-first approach with Tailwind breakpoints

## Data Flow

### File Upload Flow
1. User selects Excel file through drag-and-drop or file picker
2. Frontend validates file type and size constraints
3. FormData uploaded via multipart request to `/api/files/upload`
4. Backend validates file, stores temporarily, and creates database record
5. Background process analyzes file with OpenAI and updates analysis table
6. Frontend polls for completion and displays results

### Authentication Flow
1. User clicks sign-in, redirected to Replit OIDC provider
2. Successful authentication creates/updates user record
3. Session established with PostgreSQL-backed storage
4. Protected routes verify authentication status
5. Automatic token refresh maintains session continuity

### Data Analysis Pipeline
1. Excel file parsed to extract structured data from all sheets
2. Data sample (first 100 records) sent to OpenAI with analysis prompt
3. AI returns structured JSON with insights, recommendations, and metrics
4. Results stored in database and made available through API
5. Frontend displays analysis in organized, visual format

## External Dependencies

### Core Libraries
- **React Ecosystem**: React, React DOM, TanStack Query
- **UI Framework**: Radix UI primitives, Lucide React icons
- **Styling**: Tailwind CSS, class-variance-authority, clsx
- **Backend**: Express, Drizzle ORM, Passport for auth
- **File Processing**: Multer, XLSX, OpenAI client
- **Database**: PostgreSQL, Neon serverless driver

### Development Tools
- **Build**: Vite with React plugin and TypeScript
- **Code Quality**: TypeScript strict mode, ESLint configuration
- **Database**: Drizzle Kit for migrations and schema management
- **Environment**: tsx for TypeScript execution in development

### Third-Party Services
- **Database Hosting**: Neon PostgreSQL (DATABASE_URL required)
- **Authentication**: Replit OIDC (REPL_ID, SESSION_SECRET required)
- **AI Processing**: OpenAI API (OPENAI_API_KEY required)
- **Deployment**: Replit infrastructure with custom domain support

## Deployment Strategy

### Environment Configuration
- **Development**: tsx server with Vite dev server proxy
- **Production**: ESBuild bundles server, Vite builds static assets
- **Environment Variables**: Database URL, OpenAI key, session secrets

### Build Process
1. Frontend assets built with Vite to `dist/public`
2. Server bundled with ESBuild to `dist/index.js`
3. Static files served by Express in production
4. Database migrations applied with `npm run db:push`

### File Storage
- **Development**: Local filesystem in `uploads/` directory
- **Production**: Temporary files cleaned after processing
- **Considerations**: Could be enhanced with cloud storage (S3, etc.)

### Security Measures
- File type validation and size limits
- Secure session cookies with HTTP-only flag
- CSRF protection through SameSite cookie policy
- Environment variable protection for API keys
- SQL injection protection through parameterized queries

### Performance Optimizations
- Database connection pooling
- Frontend code splitting with Vite
- Lazy loading of AI analysis results
- Efficient Excel parsing with streaming where possible
- Memoized OIDC configuration fetching