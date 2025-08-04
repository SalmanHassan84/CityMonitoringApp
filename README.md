# CityMonitoringApp
City Issues Reporting Web Application
# CityBeats - AI-Powered Smart City Monitoring Platform
## Deployment Guide for AI/ML Engineers

### System Architecture Overview

CityBeats is a full-stack TypeScript application with the following components:
- **Frontend**: React 18 + TypeScript + Vite + Tailwind CSS
- **Backend**: Node.js + Express + TypeScript
- **Database**: MongoDB (with Mongoose ODM)
- **AI Integration**: OpenAI GPT-4o Vision API
- **Maps**: Mapbox API for location services
- **Email**: Gmail SMTP for notifications
- **Authentication**: MongoDB-based user system with sessions

### Prerequisites

1. **Node.js** (v18 or higher)
2. **MongoDB** (local or cloud instance)
3. **API Keys**:
   - OpenAI API Key (for image analysis)
   - Mapbox API Key (for maps)
   - Gmail App Password (for email notifications)

### Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# Database
MONGODB_URI=mongodb://localhost:27017/citybeats
# OR for MongoDB Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/citybeats

# OpenAI API
OPENAI_API_KEY=your_openai_api_key_here

# Mapbox API (for frontend)
VITE_MAPBOX_API_KEY=your_mapbox_api_key_here

# Gmail SMTP Configuration
GMAIL_USER=your-email@gmail.com
GMAIL_PASSWORD=your_app_password_here

# Session Secret (generate a random string)
SESSION_SECRET=your_random_session_secret_here

# Development Environment
NODE_ENV=development
PORT=5000
```

### Project Structure

```
citybeats/
├── client/                     # React frontend
│   ├── src/
│   │   ├── components/         # React components
│   │   │   ├── ui/            # shadcn/ui components
│   │   │   ├── AdminAnalytics.tsx
│   │   │   ├── AdminDashboard.tsx
│   │   │   ├── AdminIssueManagement.tsx
│   │   │   ├── AdminUserManagement.tsx
│   │   │   ├── CitizenDashboard.tsx
│   │   │   ├── Dashboard.tsx
│   │   │   ├── HomePage.tsx
│   │   │   ├── IssueHeatMap.tsx
│   │   │   ├── LoginForm.tsx
│   │   │   ├── MapboxLocationPicker.tsx
│   │   │   ├── Navigation.tsx
│   │   │   ├── ReportIssue.tsx
│   │   │   ├── ReportIssueForm.tsx
│   │   │   ├── TrackIssues.tsx
│   │   │   ├── UserAdministration.tsx
│   │   │   ├── UserPoints.tsx
│   │   │   └── UserRegistration.tsx
│   │   ├── hooks/              # Custom React hooks
│   │   ├── lib/               # Utility libraries
│   │   ├── pages/             # Page components
│   │   ├── App.tsx            # Main app component
│   │   ├── index.css          # Global styles
│   │   └── main.tsx           # React entry point
│   └── index.html
├── server/                     # Node.js backend
│   ├── models/                # MongoDB models
│   │   └── Issue.ts
│   ├── routes/                # API route modules
│   │   └── mapbox.ts
│   ├── aiService.ts           # OpenAI integration
│   ├── db.ts                  # PostgreSQL setup (unused)
│   ├── emailService.ts        # Gmail SMTP service
│   ├── index.ts               # Server entry point
│   ├── mongoAuth.ts           # MongoDB authentication
│   ├── mongodb.ts             # MongoDB connection
│   ├── routes.ts              # Main API routes
│   ├── statusUpdateHandler.ts # Issue status updates
│   ├── storage.ts             # Storage interface
│   ├── validation.ts          # Input validation
│   └── vite.ts                # Vite development server
├── shared/                     # Shared TypeScript types
│   └── schema.ts
├── uploads/                    # File upload directory
├── package.json               # Dependencies
├── tsconfig.json              # TypeScript config
├── vite.config.ts             # Vite configuration
├── tailwind.config.ts         # Tailwind CSS config
└── components.json            # shadcn/ui config
```

### Installation Instructions

1. **Clone/Extract the project files**
2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Set up environment variables**:
   - Copy the environment variables template above
   - Fill in your actual API keys and database connection string

4. **Set up MongoDB**:
   - Install MongoDB locally OR use MongoDB Atlas (cloud)
   - Create a database named `citybeats`
   - The application will automatically create the required collections

5. **Run the application**:
   ```bash
   # Development mode (runs both frontend and backend)
   npm run dev
   ```

6. **Access the application**:
   - Frontend: `http://localhost:5000`
   - Backend API: `http://localhost:5000/api`

### Key Features Implemented

1. **User Authentication System**:
   - Registration and login with MongoDB
   - Role-based access (Admin/Citizen)
   - Session management

2. **Issue Reporting System**:
   - Multi-step form with photo upload
   - AI-powered image analysis using OpenAI GPT-4o Vision
   - Location selection with Mapbox integration
   - Automatic categorization and priority assessment

3. **Points System**:
   - Users earn 10 points per reported issue
   - Points displayed on user dashboard
   - Admin can view user ratings based on points

4. **Admin Panel**:
   - User management (CRUD operations)
   - Issue management with status updates
   - Analytics dashboard with charts
   - Interactive heatmap showing issue density

5. **Email Notifications**:
   - Welcome emails for new users
   - Status update notifications
   - Admin alerts for critical issues

6. **Data Visualization**:
   - Interactive Mapbox heatmap
   - Charts and analytics using Recharts
   - Real-time data updates

### API Endpoints

#### Authentication
- `POST /api/register` - User registration
- `POST /api/login` - User login
- `POST /api/logout` - User logout
- `GET /api/mongo-user` - Get current user

#### Issues
- `POST /api/issues/report` - Submit new issue
- `GET /api/issues` - Get user's issues
- `PATCH /api/issues/:id/status` - Update issue status (admin)

#### Admin
- `GET /api/admin/users` - Get all users
- `POST /api/admin/users` - Create user
- `PATCH /api/admin/users/:id` - Update user
- `DELETE /api/admin/users/:id` - Delete user
- `GET /api/admin/all-issues` - Get all issues
- `GET /api/admin/analytics` - Get analytics data
- `GET /api/admin/system-stats` - Get system statistics

#### Analytics
- `GET /api/admin/dashboard-stats` - Dashboard statistics
- `GET /api/admin/recent-users` - Recent users list

### Database Schema

#### Users Collection
```javascript
{
  _id: ObjectId,
  fullName: String,
  email: String (unique),
  password: String (hashed),
  role: String ('admin' | 'citizen'),
  city: String,
  country: String,
  points: Number (default: 0),
  createdAt: Date,
  updatedAt: Date
}
```

#### Issues Collection
```javascript
{
  _id: ObjectId,
  title: String,
  description: String,
  category: String,
  priority: String,
  status: String,
  location: {
    lat: Number,
    lng: Number,
    address: String
  },
  images: [String], // File paths
  reportedBy: ObjectId (User reference),
  reportedAt: Date,
  aiAnalysis: {
    analysis: String,
    recommendedActions: [String],
    estimatedTimeframe: String,
    severity: String
  },
  statusHistory: [{
    status: String,
    timestamp: Date,
    updatedBy: ObjectId
  }]
}
```

### Production Deployment Considerations

1. **Environment Setup**:
   - Set `NODE_ENV=production`
   - Use production MongoDB instance
   - Configure proper CORS settings
   - Set up SSL/HTTPS

2. **File Storage**:
   - Current implementation uses local file storage
   - Consider moving to cloud storage (AWS S3, Google Cloud Storage)

3. **Email Service**:
   - Currently uses Gmail SMTP
   - Consider using dedicated email service (SendGrid, AWS SES)

4. **Security**:
   - Implement rate limiting
   - Add input sanitization
   - Set up proper authentication middleware
   - Use HTTPS in production

5. **Performance**:
   - Implement caching for analytics data
   - Optimize database queries
   - Use CDN for static assets

### Alternative Platform Deployment

#### For Jupyter Notebook Environment:
1. Focus on the AI analysis components (`server/aiService.ts`)
2. Extract the MongoDB models for data analysis
3. Use the analytics endpoints for data visualization

#### For Python/Flask Migration:
1. Convert TypeScript models to Python/SQLAlchemy
2. Migrate API endpoints to Flask routes
3. Use Python libraries for image analysis (PIL, OpenCV)
4. Implement similar email service with Flask-Mail

#### For Docker Deployment:
```dockerfile
# Dockerfile example
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build
EXPOSE 5000
CMD ["npm", "start"]
```

### Support and Maintenance

- **Logs**: Check console logs for debugging
- **Database**: Monitor MongoDB performance
- **API Keys**: Rotate keys regularly for security
- **Updates**: Keep dependencies updated

### Contact Information

For technical questions about the codebase, refer to the inline comments and this documentation. The system is designed to be modular and scalable for future enhancements.
