# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Development Commands

- `npm start` - Start development server on http://localhost:3000
- `npm run build` - Create production build in `build/` folder
- `npm test` - Run test suite with react-scripts
- `npm run lint` - Run ESLint for code quality (if available)

## Architecture Overview

This is a React TypeScript dashboard application with the following key architectural patterns:

### State Management
- **React Context + useReducer**: Authentication state managed via `AuthContext` with reducer pattern
- **React Query (@tanstack/react-query)**: Server state management with 5-minute caching, automatic retries, and optimistic updates
- **LocalStorage**: User sessions persisted across browser restarts

### Authentication System
- Demo-based authentication using hardcoded users in `DEMO_USERS` constant
- Role-based access control (Editor/Viewer roles)
- Protected routes and conditional UI based on user permissions
- Session persistence via localStorage with `STORAGE_KEYS.AUTH_USER`

### Data Management
- **API Service**: Centralized API calls in `src/services/api.ts` using JSONPlaceholder
- **Optimistic Updates**: Immediate UI updates for better UX, with rollback on failure
- **Query Invalidation**: Strategic cache invalidation for data consistency
- **Local Post Handling**: Posts with ID > 100 are treated as locally created

### Component Architecture
- **Context Providers**: Nested providers (QueryClient → Toast → Auth → MainApplication)
- **Custom Hooks**: Business logic encapsulated in hooks (usePosts, useAuth, etc.)
- **Lazy Loading**: Components loaded on demand via `LazyComponents.tsx`
- **SCSS Modules**: Component-specific styling with shared abstracts

### Key Files & Patterns

#### Core Contexts
- `src/contexts/AuthContext.tsx` - Authentication state with reducer pattern
- `src/contexts/ToastContext.tsx` - Global notification system

#### Data Management
- `src/hooks/usePosts.ts` - Complete CRUD operations with React Query
- `src/services/api.ts` - JSONPlaceholder API integration
- `src/utils/queryKeys.ts` - Centralized query key management

#### UI Components
- `src/components/UI/` - Reusable UI components (Modal, Toast, LoadingSpinner)
- `src/components/DataTable/` - Complex sortable table with pagination
- `src/components/PostsTable/` - Posts-specific table implementation

### Styling Architecture
- SCSS with abstracts in `src/styles/abstracts/`
- Variables and mixins for consistent theming
- Component-specific SCSS files co-located with components
- Responsive design with mobile-first approach

### Error Handling
- React Query retry logic for 4xx errors (no retry) vs network errors (2 retries)
- Toast notifications for user feedback on success/error states
- Optimistic updates with rollback on API failures
- Error boundaries for graceful failure handling

### Testing Notes
- Uses react-scripts test runner
- Test files should follow `.test.tsx` or `.spec.tsx` naming convention

### Demo User Credentials
Available test accounts with different permission levels:
- `editor@test.com` / `Editor123!` - Full CRUD permissions
- `viewer@test.com` / `Viewer123!` - Read-only access
- `admin@test.com` / `Admin123!` - Full CRUD permissions