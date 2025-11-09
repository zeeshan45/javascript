# Contest Participation System Backend

A comprehensive backend system for managing contests, questions, user participations, leaderboards, and prizes.

## Features

- **User Roles**: Admin, VIP, Normal, and Guest users with different access levels
- **Contest Management**: Create and manage contests with VIP and Normal access levels
- **Question Types**: Single-select, Multi-select, and True/False questions
- **Participation System**: Users can participate in contests and submit answers
- **Scoring System**: Automatic score calculation based on correct answers
- **Leaderboard**: Real-time leaderboard with rankings
- **Prize System**: Automatic prize awarding to highest-scoring users
- **User History**: Track contest participations, prizes won, and in-progress contests
- **Authentication**: JWT-based authentication
- **Rate Limiting**: API rate limiting to prevent abuse
- **Error Handling**: Comprehensive error handling and validation

## Tech Stack

- **Node.js** with Express.js
- **MongoDB** with Mongoose
- **JWT** for authentication
- **bcryptjs** for password hashing
- **express-validator** for input validation
- **express-rate-limit** for rate limiting

## Installation

1. Install dependencies:
```bash
npm install
```

2. Create a `.env` file in the root directory:
```env
PORT=3000
NODE_ENV=development
MONGODB_URI=mongodb://localhost:27017/contest_db
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
JWT_EXPIRE=7d
RATE_LIMIT_WINDOW_MS=900000
RATE_LIMIT_MAX_REQUESTS=100
```

3. Start MongoDB (if running locally):
```bash
# Make sure MongoDB is running on your system
```

4. Start the server:
```bash
# Development mode
npm run dev

# Production mode
npm start
```

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register a new user
- `POST /api/auth/login` - Login user
- `GET /api/auth/me` - Get current user

### Users
- `GET /api/users/profile` - Get user profile
- `GET /api/users` - Get all users (Admin only)
- `PUT /api/users/:id/role` - Update user role (Admin only)

### Contests
- `GET /api/contests` - Get all contests (with role-based filtering)
- `GET /api/contests/:id` - Get single contest
- `GET /api/contests/:id/questions` - Get contest questions for participation
- `POST /api/contests` - Create contest (Admin only)
- `PUT /api/contests/:id` - Update contest (Admin only)
- `DELETE /api/contests/:id` - Delete contest (Admin only)
- `POST /api/contests/:id/finalize-prizes` - Finalize prizes for ended contest (Admin only)

### Questions
- `GET /api/questions/contest/:contestId` - Get all questions for a contest (Admin only)
- `GET /api/questions/:id` - Get single question (Admin only)
- `POST /api/questions` - Create question (Admin only)
- `PUT /api/questions/:id` - Update question (Admin only)
- `DELETE /api/questions/:id` - Delete question (Admin only)

### Participations
- `POST /api/participations/start/:contestId` - Start contest participation
- `POST /api/participations/:participationId/submit` - Submit answers
- `GET /api/participations/:participationId` - Get participation details

### Leaderboard
- `GET /api/leaderboard/:contestId` - Get leaderboard for a contest
- `GET /api/leaderboard/:contestId/rank` - Get user's rank in a contest

### History
- `GET /api/history/contests` - Get user's contest history
- `GET /api/history/in-progress` - Get user's in-progress contests
- `GET /api/history/completed` - Get user's completed contests
- `GET /api/history/prizes` - Get user's prizes

## User Roles and Access

### Admin
- Can access all contests (normal and VIP)
- Can create, update, and delete contests
- Can manage questions
- Can manage user roles

### VIP Users
- Can access all contests (normal and VIP)
- Can participate in all contests
- Can view leaderboards

### Normal Users
- Can access only normal contests
- Can participate in normal contests
- Can view leaderboards for normal contests

### Guest Users
- Can only view contests (no participation)
- Cannot access VIP contests
- Must sign up and login to participate

## Authentication

All API endpoints (except registration and login) require authentication. Include the JWT token in the Authorization header:

```
Authorization: Bearer <token>
```

## Rate Limiting

- General API: 100 requests per 15 minutes
- Authentication: 5 requests per 15 minutes
- Contest Participation: 10 requests per hour
- Submission: 3 requests per minute

## Scoring System

- Correct answers contribute positively to the score
- Incorrect answers have no effect on the score
- Score is calculated based on question points
- User with highest score wins the prize

## Prize Awarding

Prizes are awarded to the user with the highest score at the end of a contest. To finalize prizes for an ended contest, use the admin endpoint:

```
POST /api/contests/:id/finalize-prizes
```

This endpoint will:
1. Update the leaderboard with final rankings
2. Award the prize to the top-ranked user
3. Record the prize in the user's prize history

## Error Handling

The API returns standardized error responses:

```json
{
  "success": false,
  "error": "Error message"
}
```

## Postman Collection

Import the `Contest_API.postman_collection.json` file into Postman for API documentation and testing.

## Creating an Admin User

You can create an admin user by:

1. Registering a normal user through the API
2. Updating the user role to 'admin' using the admin endpoint (if you have admin access)
3. Or manually updating the database:

```javascript
// In MongoDB shell or using a script
db.users.updateOne(
  { email: "admin@example.com" },
  { $set: { role: "admin" } }
)
```

