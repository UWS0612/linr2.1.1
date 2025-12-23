# LINR – CQA Logging App

## Overview
LINR is a multi-tenant web application designed for Construction Quality Assurance (CQA) logging in geomembrane projects. It enables field teams to record daily reports, manage materials, track welds, and document various quality tests. Originally designed as an offline-first app using localStorage, LINR has evolved into a cloud-based platform with user authentication, role-based access control, and project management for commercial deployment.

## User Preferences
Preferred communication style: Simple, everyday language.

## System Architecture

### Multi-Tenant Cloud Architecture
LINR now supports multiple organisations with:
- **User Authentication**: JWT-based login system with bcrypt password hashing
- **Role-Based Access Control**: Four roles (ORG_ADMIN, SUPERVISOR, QA, TECH) with different permissions
- **Project Management**: Organisation-scoped projects with specs and staff assignments
- **Database**: PostgreSQL with Drizzle ORM for type-safe database access

### Frontend Framework
The application is built with **Next.js 14** using the App Router, leveraging **React 18** and **TypeScript** for a modern, type-safe, and performant user interface.

### UI/UX Design
The application features a minimalist, professional header with a compact design and a water droplet logo. It incorporates a stylish dark theme with a dark blue-black base, vibrant teal for primary actions, and an orange accent, complemented by teal-tinted text for readability. Print output automatically switches to white backgrounds. UI components are built using **shadcn/ui** and styled with **Tailwind CSS**. A mobile-friendly CAD interface is implemented with a floating fullscreen toolbar, touch-optimized selection, and gesture support for drawing.

### Database Schema (PostgreSQL)
- **platform_users**: Platform owner accounts (separate from organisation users)
- **organisations**: Company/organisation entities with billing status, plan type, subscription dates
- **users**: User accounts with email/password authentication
- **org_users**: User membership in organisations with roles (ORG_ADMIN, SUPERVISOR, QA, TECH)
- **projects**: CQA projects belonging to organisations
- **project_specs**: Technical specifications per project (liner type, weld temp windows, etc.)
- **project_assignments**: Staff assignments to projects with project-specific roles
- **support_access**: Time-limited support access grants from orgs to platform owner
- **releases**: App version tracking (stable, beta, deprecated)
- **feature_flags**: Feature toggles with per-plan enablement
- **org_feature_overrides**: Per-org feature flag overrides

### Authentication & Authorization
- **JWT Tokens**: Payload includes userId, organisationId, orgUserId, role
- **Password Security**: bcrypt with 12 salt rounds
- **Middleware**: requireAuth for authenticated routes, requireOrgAdmin for admin routes
- **Token Storage**: Client-side localStorage with 7-day expiry

### Data Storage Strategy
- **Cloud Data**: User accounts, organisations, projects, specs, and assignments stored in PostgreSQL
- **Legacy Offline Mode**: Original localStorage-based project system still available at root path (/)
- **Hybrid Approach**: Project-scoped LINR tool at /linr/[projectId] loads specs from API

### Application Routes

**Organisation Routes:**
- **/login**: User authentication (email/password, registration)
- **/home**: Dashboard with "My Jobs" and Admin Console links
- **/admin/projects**: Admin project management (list, create, edit)
- **/admin/projects/[id]**: Project details with Overview, Specs, Staff tabs
- **/admin/staff**: Team member management with role assignment
- **/linr/[projectId]**: Project-scoped LINR field tool
- **/**: Original offline-first LINR tool (legacy)

**UWS Platform Console (Platform Owner Only):**
- **/platform/login**: Platform owner authentication
- **/platform/orgs**: List all organisations with billing status, plan, usage stats
- **/platform/orgs/[id]**: Organisation detail with subscription management, support access
- **/platform/releases**: Version management and feature flags
- **/platform/metrics**: System-wide usage dashboard (aggregated counts only)

### API Endpoints
- **POST /api/auth/login**: User authentication
- **POST /api/auth/register**: New organisation/user registration
- **GET /api/my-projects**: Projects assigned to current user
- **GET /api/org-projects**: All organisation projects (admin only)
- **POST/GET /api/projects**: Create/list projects
- **GET/PUT/DELETE /api/projects/[id]**: Project CRUD
- **GET/PUT /api/projects/[id]/specs**: Project specifications
- **GET/POST /api/projects/[id]/assignments**: Staff assignments
- **DELETE /api/projects/[id]/assignments/[assignmentId]**: Remove assignment
- **GET/POST /api/staff**: Team member management

### Role Permissions
- **ORG_ADMIN**: Full access to Admin Console, create/edit projects, assign staff, edit specs
- **SUPERVISOR**: View assigned jobs, full LINR field tool access
- **QA**: View assigned jobs, full LINR field tool access
- **TECH**: View assigned jobs, log defects/seams/tests only

### State Management
**React's `useState`** and `useEffect` hooks manage local component state. Authentication state is managed via React Context (AuthProvider).

### Data Export Architecture
The application uses the **JSZip library** to generate client-side ZIP archives containing CSV exports of all logged data, JSON exports of as-built drawings, and Markdown summary reports.

### Responsive Design
A **mobile-first approach** with Tailwind CSS ensures the application is fully responsive, featuring custom breakpoints, touch-optimized controls, mobile navigation, adaptive tables, and flexible forms suitable for field use. The application also functions as a **Progressive Web App (PWA)** with offline caching via a service worker and an installable manifest.

### Key Features
- **User Authentication**: Secure JWT-based login with bcrypt password hashing
- **Multi-Tenant Architecture**: Organisation-scoped data isolation
- **Role-Based Access Control**: Four roles with different permission levels
- **Project Management**: Create projects, set specs, assign staff
- **CQA Logging Modules**: Supports daily reports, material tracking, weld management, and various quality tests
- **As-Built Drawings**: Comprehensive drawing suite with tools (Select, Line, Curve, Rectangle, Structure, Text, Measure, GPS Marker), zoom & pan, undo/redo, smart erasers, layer system, and keyboard shortcuts
- **Role-Based Submission**: "Final Joint Portal" provides a role-based end-of-day submission system with audit trails
- **Print Functionality**: Generates professional, branded reports with company logos and page numbers

## External Dependencies

### Backend/Database
- `drizzle-orm` and `drizzle-kit` for PostgreSQL ORM
- `pg` PostgreSQL client
- `bcryptjs` for password hashing
- `jsonwebtoken` for JWT authentication
- `dotenv` for environment configuration

### UI Component Libraries
- `@radix-ui/react-dropdown-menu`, `@radix-ui/react-label`, `@radix-ui/react-separator`
- `class-variance-authority`, `tailwind-merge`, `clsx`, `tailwindcss-animate`

### Data Processing
- `JSZip` (v3.10.1) for client-side ZIP file generation

### Icons & Assets
- `Lucide React` (v0.462.0) for UI icons

### Build Tools
- `Next.js` (v14.2.13)
- `Tailwind CSS` (v3.4.1)
- `PostCSS`, `Autoprefixer`
- `ESLint`

### Development Environment
- `TypeScript` (v5) and `@types` packages

### Runtime Environment
- Node.js 20+

## Database Commands
- `npm run db:push` - Push schema changes to database
- `npm run db:studio` - Open Drizzle Studio for database management

## Deployment Configuration
The application is configured for **Autoscale** deployment on Replit:
- **Build Command**: `npm run build --prefix linr-cqa`
- **Run Command**: `npm run start --prefix linr-cqa`
- **Deployment Type**: Autoscale (serverless)
- **Database**: PostgreSQL (Neon-backed, provisioned via Replit)
- **Port Configuration**: Port 5000 for both dev and production
- **Directory Structure**: App is located in `linr-cqa` subdirectory

## Security Considerations
- All API routes validate JWT tokens before processing
- Organisation-scoped data access prevents cross-tenant data leaks
- Admin routes require ORG_ADMIN role
- Passwords are hashed with bcrypt (12 salt rounds)
- Session secret stored in environment variable (SESSION_SECRET)
