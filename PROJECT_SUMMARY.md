# Contest Participation System - Project Summary

## Overview
A comprehensive backend system for managing contests, questions, user participations, leaderboards, and prizes. Built with Express.js and MongoDB.

## Features Implemented

### ✅ User Management
- User registration and authentication (JWT)
- Role-based access control (Admin, VIP, Normal, Guest)
- User profile management
- Admin user creation script

### ✅ Contest Management
- Create, read, update, delete contests
- Contest access levels (Normal, VIP)
- Contest status tracking (upcoming, active, ended)
- Contest filtering by status and access level
- Prize finalization for ended contests

### ✅ Question Management
- Support for three question types:
  - Single-select (one correct answer)
  - Multi-select (multiple correct answers)
  - True/False (two options)
- Question CRUD operations
- Point-based scoring system
- Question ordering

### ✅ Participation System
- Start contest participation
- Submit answers with validation
- Automatic score calculation
- Answer evaluation based on question type
- Participation status tracking (in-progress, completed)

### ✅ Scoring System
- Correct answers contribute positively to score
- Incorrect answers have no effect (no negative scoring)
- Points based on question difficulty
- Real-time score calculation

### ✅ Leaderboard
- Real-time leaderboard updates
- Ranking based on score (highest first)
- Tie-breaking by submission time (earlier submission ranks higher)
- Pagination support
- User rank lookup

### ✅ Prize System
- Automatic prize awarding to highest scorer
- Prize finalization endpoint for ended contests
- Prize history tracking for users
- Duplicate prize prevention

### ✅ User History
- Contest participation history
- In-progress contests tracking
- Completed contests with scores and ranks
- Prize history

### ✅ Security & Performance
- JWT authentication
- Password hashing with bcrypt
- Role-based access control
- Rate limiting on all endpoints
- Input validation with express-validator
- Error handling middleware
- Security headers with Helmet
- CORS configuration

### ✅ API Documentation
- Complete Postman collection

## Technical Stack

- **Runtime**: Node.js
- **Framework**: Express.js
- **Database**: MongoDB with Mongoose
- **Authentication**: JWT (jsonwebtoken)
- **Password Hashing**: bcryptjs
- **Validation**: express-validator
- **Rate Limiting**: express-rate-limit
- **Security**: Helmet, CORS
- **Logging**: Morgan

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user

### Users
- `GET /api/users/profile` - Get user profile
- `GET /api/users` - Get all users (Admin)
- `PUT /api/users/:id/role` - Update user role (Admin)

### Contests
- `GET /api/contests` - Get all contests
- `GET /api/contests/:id` - Get single contest
- `GET /api/contests/:id/questions` - Get contest questions
- `POST /api/contests` - Create contest (Admin)
- `PUT /api/contests/:id` - Update contest (Admin)
- `DELETE /api/contests/:id` - Delete contest (Admin)
- `POST /api/contests/:id/finalize-prizes` - Finalize prizes (Admin)

### Questions
- `GET /api/questions/contest/:contestId` - Get questions (Admin)
- `GET /api/questions/:id` - Get question (Admin)
- `POST /api/questions` - Create question (Admin)
- `PUT /api/questions/:id` - Update question (Admin)
- `DELETE /api/questions/:id` - Delete question (Admin)

### Participations
- `POST /api/participations/start/:contestId` - Start contest
- `POST /api/participations/:participationId/submit` - Submit answers
- `GET /api/participations/:participationId` - Get participation details

### Leaderboard
- `GET /api/leaderboard/:contestId` - Get leaderboard
- `GET /api/leaderboard/:contestId/rank` - Get user rank

### History
- `GET /api/history/contests` - Get contest history
- `GET /api/history/in-progress` - Get in-progress contests
- `GET /api/history/completed` - Get completed contests
- `GET /api/history/prizes` - Get prizes won

## User Roles & Access

### Admin
- Full access to all features
- Can create, update, delete contests
- Can manage questions
- Can manage user roles
- Can finalize prizes

### VIP Users
- Access to all contests (normal + VIP)
- Can participate in all contests
- Can view all leaderboards

### Normal Users
- Access to normal contests only
- Can participate in normal contests
- Can view leaderboards for normal contests

### Guest Users
- View-only access (no participation)
- Cannot access VIP contests
- Must sign up to participate

## Key Implementation Details

### Scoring Logic
- Single-select: User must select exactly one option that matches the correct answer
- Multi-select: User must select all correct options and no incorrect options
- True/False: User must select the correct boolean option
- Points are awarded only for correct answers
- Incorrect answers don't reduce score

### Prize Awarding
- Prizes are awarded to the user with the highest score
- In case of ties, the user who submitted first wins
- Prizes are finalized when contest ends via admin endpoint
- Prize information is stored in user's prize history

### Rate Limiting
- General API: 100 requests per 15 minutes
- Authentication: 5 requests per 15 minutes
- Contest Participation: 10 requests per hour
- Submission: 3 requests per minute

### Error Handling
- Standardized error responses
- Validation errors with detailed messages
- Authentication/authorization errors
- Database errors handling
- Development mode error stack traces

## Database Models

### User
- Basic user information
- Role (admin, vip, normal, guest)
- Password (hashed)
- Prize history

### Contest
- Contest details (name, description)
- Access level (normal, vip)
- Start/end times
- Prize information
- Status (upcoming, active, ended)
- Questions references

### Question
- Question text and type
- Options with correctness flags
- Points value
- Order/position
- Contest reference

### Participation
- User and contest references
- Answers with evaluation
- Total score
- Status (in-progress, completed)
- Rank and prize information

### Leaderboard
- Contest reference
- Rankings with scores
- Last updated timestamp

