# Task Manager - Full Stack Web Application

A modern, scalable, and secure task management application built with **Next.js** (frontend) and **Node.js/Express** (backend) featuring JWT-based authentication, CRUD operations, and responsive design.

## Features

### Frontend
- Built with **Next.js 15** and **TypeScript**
- **TailwindCSS** for responsive, modern UI
- Client-side and server-side form validation
- Protected routes with authentication
- Real-time search and filtering
- User profile management
- Task management dashboard

### Backend
- **Node.js** with **Express** framework
- **MongoDB** database with Mongoose ODM
- **JWT-based authentication**
- **bcrypt** password hashing
- Input validation with express-validator
- RESTful API architecture
- Error handling middleware
- CORS enabled for frontend integration

### Security Features
- Password hashing with bcrypt (salt rounds: 10)
- JWT token authentication with 30-day expiration
- Protected API routes with middleware
- Input validation on both client and server
- Secure HTTP headers
- Authorization checks for resource access

## Project Structure

```
.
├── frontend/                 # Next.js frontend application
│   ├── app/                 # Next.js app directory
│   │   ├── dashboard/       # Dashboard page (protected)
│   │   ├── login/           # Login page
│   │   ├── signup/          # Signup page
│   │   ├── globals.css      # Global styles
│   │   ├── layout.tsx       # Root layout with AuthProvider
│   │   └── page.tsx         # Home page
│   ├── components/          # Reusable React components
│   │   ├── Navbar.tsx       # Navigation bar
│   │   └── ProtectedRoute.tsx  # Route protection wrapper
│   ├── lib/                 # Utility functions
│   │   ├── api.ts           # Axios instance with interceptors
│   │   └── AuthContext.tsx  # Authentication context
│   ├── types/               # TypeScript type definitions
│   │   └── index.ts
│   └── package.json
│
├── backend/                 # Express backend application
│   ├── config/              # Configuration files
│   │   └── db.js            # MongoDB connection
│   ├── middleware/          # Express middleware
│   │   ├── auth.js          # JWT authentication middleware
│   │   └── errorHandler.js  # Error handling middleware
│   ├── models/              # Mongoose models
│   │   ├── User.js          # User model with password hashing
│   │   └── Task.js          # Task model
│   ├── routes/              # API routes
│   │   ├── auth.js          # Authentication routes
│   │   └── tasks.js         # Task CRUD routes
│   ├── utils/               # Utility functions
│   │   └── generateToken.js # JWT token generation
│   ├── .env                 # Environment variables
│   ├── server.js            # Express server entry point
│   └── package.json
│
├── API_DOCUMENTATION.md     # Complete API documentation
├── Task_Manager_API.postman_collection.json  # Postman collection
└── README.md                # This file
```

## Prerequisites

- **Node.js** (v18 or higher)
- **MongoDB** (v6 or higher)
- **npm** or **yarn**

## Installation & Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd task-manager
```

### 2. Backend Setup

```bash
cd backend
npm install
```

Create a `.env` file in the backend directory:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/taskmanager
JWT_SECRET=your_jwt_secret_key_here_change_in_production
NODE_ENV=development
```

**Important:** Change the `JWT_SECRET` to a strong, random string in production.

### 3. Frontend Setup

```bash
cd frontend
npm install
```

Create a `.env.local` file in the frontend directory:

```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
```

### 4. Start MongoDB

Make sure MongoDB is running on your system:

```bash
# For Windows (if installed as service)
net start MongoDB

# For macOS/Linux
mongod

# Or using Docker
docker run -d -p 27017:27017 --name mongodb mongo:latest
```

### 5. Run the Application

**Backend (Terminal 1):**
```bash
cd backend
npm run dev
```

The backend server will start on `http://localhost:5000`

**Frontend (Terminal 2):**
```bash
cd frontend
npm run dev
```

The frontend application will start on `http://localhost:3000`

### 6. Access the Application

Open your browser and navigate to `http://localhost:3000`

1. **Sign Up**: Create a new account
2. **Login**: Sign in with your credentials
3. **Dashboard**: Manage your tasks

## API Endpoints

### Authentication
- `POST /api/auth/signup` - Register new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/profile` - Get user profile (protected)
- `PUT /api/auth/profile` - Update user profile (protected)

### Tasks
- `GET /api/tasks` - Get all tasks with filters (protected)
- `GET /api/tasks/:id` - Get single task (protected)
- `POST /api/tasks` - Create task (protected)
- `PUT /api/tasks/:id` - Update task (protected)
- `DELETE /api/tasks/:id` - Delete task (protected)

For detailed API documentation, see [API_DOCUMENTATION.md](./API_DOCUMENTATION.md)

## Testing with Postman

1. Import `Task_Manager_API.postman_collection.json` into Postman
2. The collection includes all API endpoints with example requests
3. After login/signup, the auth token is automatically saved to variables
4. All protected routes use the saved token automatically

## Features Demonstration

### 1. Authentication Flow
- User signup with validation
- Login with email and password
- JWT token storage in localStorage
- Automatic token attachment to API requests
- Token expiration handling

### 2. Task Management
- Create new tasks with title, description, status, priority, and due date
- View all tasks in a clean, organized dashboard
- Edit existing tasks
- Delete tasks with confirmation
- Real-time updates

### 3. Search and Filtering
- Search tasks by title or description
- Filter by status (pending, in-progress, completed)
- Filter by priority (low, medium, high)
- Combine multiple filters

### 4. User Profile
- View user information
- Edit profile (name, email, bio)
- Profile updates sync with navigation

## Scalability Notes

### Current Architecture
The application is built with scalability in mind using modular architecture and best practices.

### Scaling for Production

#### 1. **Frontend Scaling**

**Current Setup:**
- Next.js with client-side rendering for dynamic pages
- TailwindCSS for optimized CSS bundle

**Production Improvements:**
- Deploy to **Vercel** or **Netlify** for automatic CDN distribution
- Enable Next.js **Image Optimization** for faster loading
- Implement **Code Splitting** for larger applications
- Add **Redis** for client-side caching
- Use **React Query** or **SWR** for data fetching and caching
- Implement **Service Workers** for offline functionality

**Example:**
```typescript
// Add React Query for caching
import { QueryClient, QueryClientProvider } from '@tanstack/react-query';

const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 5 * 60 * 1000, // 5 minutes
      cacheTime: 10 * 60 * 1000, // 10 minutes
    },
  },
});
```

#### 2. **Backend Scaling**

**Current Setup:**
- Single Node.js server
- MongoDB for data storage

**Production Improvements:**

**Horizontal Scaling:**
- Deploy multiple instances behind a **load balancer** (nginx, AWS ALB)
- Use **PM2** for cluster mode on single server:
```bash
pm2 start server.js -i max
```

**Database Scaling:**
- Implement **MongoDB replica sets** for high availability
- Add **database indexing** for faster queries (already implemented for tasks)
- Use **MongoDB sharding** for very large datasets
- Implement **read replicas** for read-heavy workloads

**Caching Layer:**
- Add **Redis** for session management and caching:
```javascript
const redis = require('redis');
const client = redis.createClient();

// Cache frequent queries
app.get('/api/tasks', async (req, res) => {
  const cacheKey = `tasks:${req.user._id}`;
  const cached = await client.get(cacheKey);

  if (cached) {
    return res.json(JSON.parse(cached));
  }

  const tasks = await Task.find({ user: req.user._id });
  await client.setEx(cacheKey, 300, JSON.stringify(tasks)); // 5 min cache
  res.json(tasks);
});
```

**API Gateway:**
- Implement **rate limiting** to prevent abuse
- Add **API versioning** for backward compatibility
- Use **GraphQL** for flexible data fetching (future enhancement)

#### 3. **Database Optimization**

**Indexing Strategy:**
```javascript
// Already implemented in Task model
taskSchema.index({ user: 1, status: 1 });
taskSchema.index({ user: 1, createdAt: -1 });

// Additional indexes for production
taskSchema.index({ user: 1, priority: 1 });
taskSchema.index({ title: 'text', description: 'text' }); // Full-text search
```

**Connection Pooling:**
```javascript
mongoose.connect(process.env.MONGODB_URI, {
  maxPoolSize: 50,
  minPoolSize: 10,
  serverSelectionTimeoutMS: 5000,
});
```

#### 4. **Security Enhancements**

**Current:** Basic JWT authentication and bcrypt hashing

**Production Additions:**
- Implement **helmet.js** for security headers
- Add **rate limiting** with express-rate-limit
- Enable **HTTPS** with SSL certificates
- Implement **refresh tokens** for better security
- Add **OAuth2** providers (Google, GitHub)
- Implement **2FA** for sensitive accounts
- Add **CSRF protection**

**Example:**
```javascript
const helmet = require('helmet');
const rateLimit = require('express-rate-limit');

app.use(helmet());

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100 // limit each IP to 100 requests per windowMs
});

app.use('/api/', limiter);
```

#### 5. **Monitoring and Logging**

**Production Requirements:**
- **Application Monitoring**: New Relic, DataDog, or AWS CloudWatch
- **Error Tracking**: Sentry for error reporting
- **Logging**: Winston or Bunyan for structured logging
- **Performance Monitoring**: Track API response times
- **Uptime Monitoring**: UptimeRobot or Pingdom

**Example:**
```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});
```

#### 6. **CI/CD Pipeline**

**Recommended Setup:**
- **GitHub Actions** or **GitLab CI** for automated testing and deployment
- **Docker** containers for consistent environments
- **Kubernetes** for orchestration at scale
- **Automated testing** with Jest and Cypress

**Example GitHub Actions:**
```yaml
name: CI/CD

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Run tests
        run: npm test
      - name: Deploy
        run: npm run deploy
```

#### 7. **Microservices Architecture (Future)**

For very large scale:
- Split into **microservices** (Auth Service, Task Service, Notification Service)
- Use **message queues** (RabbitMQ, Apache Kafka) for inter-service communication
- Implement **API Gateway** pattern
- Use **Docker** and **Kubernetes** for container orchestration

#### 8. **Performance Optimization**

- **Compression**: Use gzip compression
- **CDN**: Serve static assets via CDN
- **Lazy Loading**: Implement lazy loading for frontend components
- **Database Query Optimization**: Use aggregation pipelines
- **WebSocket**: For real-time updates (Socket.io)

### Estimated Capacity

**Current Setup:**
- ~100 concurrent users
- ~1000 requests per minute

**With Optimizations:**
- ~10,000+ concurrent users
- ~100,000+ requests per minute
- Horizontal scaling capability

## Development Scripts

### Backend
```bash
npm start       # Start production server
npm run dev     # Start development server with nodemon
```

### Frontend
```bash
npm run dev     # Start development server
npm run build   # Build for production
npm start       # Start production server
npm run lint    # Run ESLint
```

## Environment Variables

### Backend (.env)
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/taskmanager
JWT_SECRET=your_secret_key
NODE_ENV=development
```

### Frontend (.env.local)
```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
```

## Technologies Used

### Frontend
- Next.js 15
- React 19
- TypeScript
- TailwindCSS
- Axios

### Backend
- Node.js
- Express
- MongoDB
- Mongoose
- bcryptjs
- jsonwebtoken
- express-validator
- cors
- dotenv

## License

MIT License

## Author

Created for Frontend Developer Intern Assignment

---

## Quick Start Commands

```bash
# Install all dependencies
cd backend && npm install && cd ../frontend && npm install && cd ..

# Start MongoDB (if not running)
mongod

# Start backend (in one terminal)
cd backend && npm run dev

# Start frontend (in another terminal)
cd frontend && npm run dev
```

Visit `http://localhost:3000` to see the application!
