# LinkA React Frontend Integration Prompt

## Project Overview

Create a comprehensive React frontend application for **LinkA** - a property and marketplace platform for East Africa, specifically optimized for Uganda. This frontend will integrate with a Spring Boot backend API running on `http://localhost:8081`.

## Backend API Base Configuration

- **Base URL**: `http://localhost:8080/api`
- **Authentication**: JWT Bearer tokens
- **CORS**: Enabled for `http://localhost:5173`
- **Content-Type**: `application/json` for most endpoints, `multipart/form-data` for file uploads

## Required Dependencies

Add these to your existing `package.json`:

```json
{
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.26.2",
    "axios": "^1.7.7",
    "lucide-react": "^0.439.0",
    "@radix-ui/react-alert-dialog": "^1.1.2",
    "@radix-ui/react-avatar": "^1.1.1",
    "@radix-ui/react-button": "^1.1.2",
    "@radix-ui/react-card": "^1.1.2",
    "@radix-ui/react-dialog": "^1.1.2",
    "@radix-ui/react-dropdown-menu": "^2.1.2",
    "@radix-ui/react-form": "^0.1.0",
    "@radix-ui/react-input": "^1.1.2",
    "@radix-ui/react-label": "^2.1.2",
    "@radix-ui/react-navigation-menu": "^1.2.2",
    "@radix-ui/react-select": "^2.1.2",
    "@radix-ui/react-separator": "^1.1.2",
    "@radix-ui/react-slot": "^1.1.2",
    "@radix-ui/react-switch": "^1.1.2",
    "@radix-ui/react-tabs": "^1.1.2",
    "@radix-ui/react-textarea": "^1.1.2",
    "@radix-ui/react-toast": "^1.2.2",
    "class-variance-authority": "^0.7.0",
    "clsx": "^2.1.1",
    "tailwind-merge": "^2.5.2",
    "tailwindcss-animate": "^1.0.7",
    "react-hook-form": "^7.52.1",
    "@hookform/resolvers": "^3.9.0",
    "zod": "^3.23.8",
    "date-fns": "^3.6.0",
    "react-query": "^3.39.3"
  }
}
```

## API Integration Structure

### 1. Authentication APIs (`/api/auth`)

**Register User**
```typescript
POST /api/auth/register
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe", 
  "email": "john@example.com",
  "password": "password123",
  "phoneNumber": "+256700000000",
  "userType": "BUYER", // BUYER | SELLER | BOTH
  "preferredLanguage": "en",
  "country": "Uganda"
}
```

**Login User**
```typescript
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```typescript
{
  "token": "jwt_token_here",
  "user": {
    "id": 1,
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "userType": "BUYER"
  }
}
```

**Get Profile**
```typescript
GET /api/auth/profile
Authorization: Bearer {token}
```

**Update Profile**
```typescript
PUT /api/auth/profile
Authorization: Bearer {token}
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe",
  "phoneNumber": "+256700000000",
  "location": "Kampala",
  "city": "Kampala",
  "district": "Kampala Central"
}
```

### 2. Category APIs (`/api/categories`)

**Get All Categories**
```typescript
GET /api/categories?page=0&size=20&search=electronics
```

**Get Active Categories**
```typescript
GET /api/categories/active
```

**Get Featured Categories**
```typescript
GET /api/categories/featured
```

**Get Subcategories**
```typescript
GET /api/categories/{categoryId}/subcategories
```

### 3. Listing APIs (`/api/listings`)

**Get All Listings**
```typescript
GET /api/listings?page=0&size=20&sortBy=createdAt&sortDir=desc
```

**Search Listings**
```typescript
GET /api/listings/search?search=laptop&page=0&size=20
```

**Get Listings by Category**
```typescript
GET /api/listings/category/1?page=0&size=20
```

**Get Listings by Price Range**
```typescript
GET /api/listings/price-range?minPrice=100000&maxPrice=500000&page=0&size=20
```

**Get Featured Listings**
```typescript
GET /api/listings/featured
```

**Get Latest Listings**
```typescript
GET /api/listings/latest?limit=10
```

**Get Trending Listings**
```typescript
GET /api/listings/trending
```

**Get Single Listing**
```typescript
GET /api/listings/{id}
```

**Create Listing**
```typescript
POST /api/listings
Authorization: Bearer {token}
Content-Type: multipart/form-data

// Form Data:
// data: JSON string of listing data
// images: File[] (optional)

{
  "title": "MacBook Pro 2023",
  "description": "Excellent condition MacBook Pro",
  "price": 4500000,
  "originalPrice": 5500000,
  "categoryId": 1,
  "listingType": "SELL", // SELL | BUY | RENT | SERVICE | JOB | EVENT
  "conditionType": "GOOD", // NEW | LIKE_NEW | GOOD | FAIR | POOR | REFURBISHED
  "location": "Kampala",
  "city": "Kampala", 
  "district": "Kampala Central",
  "brand": "Apple",
  "model": "MacBook Pro",
  "color": "Space Gray",
  "size": "14 inch",
  "tags": "laptop,apple,macbook",
  "negotiable": true,
  "allowOffers": true,
  "minimumOffer": 4000000,
  "quantityAvailable": 1,
  "featuredImageIndex": 0
}
```

**Toggle Favorite**
```typescript
POST /api/listings/{id}/favorite
Authorization: Bearer {token}
```

**Increment Contact Count**
```typescript
POST /api/listings/{id}/contact
```

### 4. File Upload APIs (`/api/uploads`)

**Serve Listing Images**
```typescript
GET http://localhost:8080/api/uploads/listings/{listingId}/{filename}
// Returns image file
```

### 5. Mobile Money APIs (`/api/mobile-money`)

**Get Providers**
```typescript
GET /api/mobile-money/providers
```

**Initiate Payment**
```typescript
POST /api/mobile-money/payment
Content-Type: application/json

{
  "provider": "mtn", // mtn | airtel | mula
  "phoneNumber": "+256700000000",
  "amount": 50000,
  "reference": "ORDER_123"
}
```

**Validate Phone Number**
```typescript
GET /api/mobile-money/validate-phone/{phoneNumber}
```

**Check Transaction Status**
```typescript
GET /api/mobile-money/transaction/{transactionId}
```

### 6. Health Check APIs

**Health Check**
```typescript
GET /api/health
```

**Service Info**
```typescript
GET /api/info
```

## Frontend Components to Build

### 1. Authentication System
- **LoginForm.tsx** - Email/password login with JWT
- **RegisterForm.tsx** - User registration with validation
- **ProtectedRoute.tsx** - Route guard for authenticated users
- **AuthContext.tsx** - Global authentication state
- **useAuth.tsx** - Authentication hook

### 2. Listing Management
- **ListingCard.tsx** - Individual listing display
- **ListingGrid.tsx** - Grid layout for listings
- **ListingDetail.tsx** - Single listing page with full details
- **CreateListing.tsx** - Multi-step listing creation form
- **ListingFilters.tsx** - Search and filter controls
- **ImageUpload.tsx** - Multiple image upload component

### 3. Category System
- **CategoryTree.tsx** - Hierarchical category display
- **CategoryCard.tsx** - Individual category component
- **CategoryFilter.tsx** - Category selection for filtering

### 4. Search & Discovery
- **SearchBar.tsx** - Main search functionality
- **FilterPanel.tsx** - Advanced filtering options
- **SortOptions.tsx** - Sorting controls
- **LocationSelector.tsx** - Uganda location picker

### 5. Payment Integration
- **MobileMoneyProvider.tsx** - Payment provider selection
- **PaymentForm.tsx** - Payment processing interface
- **TransactionStatus.tsx** - Payment status display

### 6. User Profile
- **ProfileForm.tsx** - User profile management
- **UserListings.tsx** - User's own listings
- **UserStats.tsx** - User statistics display

## State Management

### Using React Query for API State
```typescript
// Example Query Setup
import { useQuery, useMutation, useQueryClient } from 'react-query'
import axios from 'axios'

const API_BASE = 'http://localhost:8080/api'

// Auth queries
export const useAuth = () => useQuery('auth', async () => {
  const { data } = await axios.get(`${API_BASE}/auth/check`, {
    headers: { Authorization: `Bearer ${localStorage.getItem('token')}` }
  })
  return data
})

// Listings queries  
export const useListings = (params: any) => useQuery(['listings', params], async () => {
  const { data } = await axios.get(`${API_BASE}/listings`, { params })
  return data
})

// Categories queries
export const useCategories = () => useQuery('categories', async () => {
  const { data } = await axios.get(`${API_BASE}/categories/active`)
  return data
})
```

### Authentication Context
```typescript
interface AuthContextType {
  user: User | null
  token: string | null
  login: (email: string, password: string) => Promise<void>
  register: (userData: RegisterData) => Promise<void>
  logout: () => void
  isAuthenticated: boolean
  isLoading: boolean
}
```

## Environment Configuration

Create `.env.local`:
```env
VITE_API_BASE_URL=http://localhost:8080/api
VITE_UPLOAD_BASE_URL=http://localhost:8080/api/uploads
VITE_APP_NAME=LinkA
VITE_DEFAULT_LANGUAGE=en
VITE_DEFAULT_COUNTRY=Uganda
```

## Uganda-Specific Features

### 1. Location Support
- Support for Uganda districts and cities
- Local address formats
- Regional mobile money integration

### 2. Mobile Money Integration
- MTN Mobile Money (most popular)
- Airtel Money 
- Mula by Stanbic
- Phone number validation for Uganda format

### 3. Currency
- Uganda Shillings (UGX) as default
- Price formatting for local market
- Mobile money amounts in UGX

### 4. Language Support
- English as primary language
- Ready for Luganda and other local languages
- RTL text support preparation

## Key User Flows

### 1. User Registration & Authentication
1. User visits homepage
2. Clicks "Sign Up" → RegisterForm
3. Fills registration data → Submits to `/api/auth/register`
4. Automatic login or redirect to login
5. AuthContext stores JWT token
6. Protected routes become accessible

### 2. Creating a Listing
1. Authenticated user clicks "Post Ad"
2. CreateListing component with multi-step form
3. Category selection from `/api/categories/active`
4. Image upload with preview
5. Submit to `/api/listings` with multipart/form-data
6. Success message and redirect to listing detail

### 3. Browsing & Searching
1. Homepage shows featured categories and latest listings
2. SearchBar component queries `/api/listings/search`
3. FilterPanel allows price, location, category filtering
4. Results display in ListingGrid with pagination

### 4. Making a Purchase
1. User views listing detail
2. Clicks "Contact Seller" → increments contact count
3. User can initiate mobile money payment
4. MobileMoneyProvider component shows available options
5. Payment flow through `/api/mobile-money/payment`

## Responsive Design Requirements

- **Mobile-first** approach for East African market
- Optimized for 2G/3G networks
- Progressive Web App (PWA) capabilities
- Offline-ready with service workers
- Touch-friendly interfaces

## Testing Strategy

### Unit Tests
- Component testing with React Testing Library
- API service testing with Jest and MSW
- Authentication flow testing

### Integration Tests  
- Full user journey testing
- Payment flow testing
- File upload testing

### Performance Testing
- Image optimization and lazy loading
- API response time monitoring
- Bundle size optimization

## Deployment Considerations

### Development
- Vite dev server on port 5173
- Hot module replacement for fast development
- Proxy API calls to http://localhost:8080

### Production
- Static file hosting (Vercel, Netlify, or similar)
- Environment variable configuration
- Image optimization and CDN integration
- Service worker for caching

## Security Considerations

### Client-Side Security
- JWT token storage (httpOnly cookies preferred)
- XSS protection with content security policy
- Input validation and sanitization
- Secure file upload handling

### API Security
- CORS configuration for production domains
- Rate limiting implementation
- File upload restrictions
- User input validation

## Test Users

The backend creates these test accounts:
- **Admin**: admin@linka.com / admin123
- **Buyer**: john@example.com / password123  
- **Seller**: jane@example.com / password123

## Sample Categories

The backend includes these test categories:
1. **Electronics** (ID: 1)
2. **Fashion** (ID: 2) 
3. **Home & Garden** (ID: 3)

## Next Steps

1. Set up the project structure with the above components
2. Implement authentication system with JWT
3. Create listing management interfaces
4. Build search and filtering functionality
5. Integrate mobile money payment flows
6. Add responsive design and testing
7. Deploy and configure production environment

This comprehensive prompt provides everything needed to build a full-featured React frontend for the LinkA marketplace platform, optimized for the East African market with particular focus on Uganda's mobile money ecosystem.