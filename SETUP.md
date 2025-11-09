# Setup Guide

## Quick Start

1. **Install Dependencies**
   ```bash
   npm install
   ```

2. **Configure Environment**
   - Update MongoDB URI if needed
   - Set a strong JWT_SECRET

3. **Start MongoDB**
   - Make sure MongoDB is running on your system
   - Default connection: `mongodb://localhost:27017/contest_db`

4. **Create Admin User**
   ```bash
   npm run create-admin
   ```
   This will create an admin user with:
   - Email: `admin@contest.com`
   - Password: `admin123`
   - Role: `admin`

5. **Start Server**
   ```bash
   # Development mode (with nodemon)
   npm run dev

   # Production mode
   npm start
   ```

6. **Test API**
   - Server should be running on `http://localhost:3000`
   - Health check: `GET http://localhost:3000/health`
   - Import `Contest_API.postman_collection.json` into Postman for API testing

## Testing the API

### 1. Register a User
```bash
POST http://localhost:3000/api/auth/register
Content-Type: application/json

{
  "name": "testuser",
  "email": "test@example.com",
  "password": "password123"
}
```

### 2. Login
```bash
POST http://localhost:3000/api/auth/login
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "password123"
}
```

Save the token from the response for authenticated requests.

### 3. Create a Contest (Admin)
```bash
POST http://localhost:3000/api/contests
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "name": "Summer Quiz",
  "description": "Test your knowledge",
  "accessLevel": "normal",
  "startTime": "2024-01-01T00:00:00Z",
  "endTime": "2024-12-31T23:59:59Z",
  "prize": "$1000 Cash Prize"
}
```

### 4. Create Questions (Admin)
```bash
POST http://localhost:3000/api/questions
Authorization: Bearer <admin_token>
Content-Type: application/json

{
  "contestId": "<contest_id>",
  "questionText": "What is 2+2?",
  "questionType": "single-select",
  "options": [
    { "text": "3", "isCorrect": false },
    { "text": "4", "isCorrect": true },
    { "text": "5", "isCorrect": false }
  ],
  "points": 10,
  "order": 1
}
```

### 5. Start Contest Participation
```bash
POST http://localhost:3000/api/participations/start/<contest_id>
Authorization: Bearer <user_token>
```

### 6. Submit Answers
```bash
POST http://localhost:3000/api/participations/<participation_id>/submit
Authorization: Bearer <user_token>
Content-Type: application/json

{
  "answers": [
    {
      "questionId": "<question_id>",
      "selectedOptions": [1]
    }
  ]
}
```

### 7. View Leaderboard
```bash
GET http://localhost:3000/api/leaderboard/<contest_id>
Authorization: Bearer <user_token>
```

### 8. Finalize Prizes (Admin - after contest ends)
```bash
POST http://localhost:3000/api/contests/<contest_id>/finalize-prizes
Authorization: Bearer <admin_token>
```

## User Roles

- **Admin**: Full access to all features
- **VIP**: Access to all contests (normal + VIP)
- **Normal**: Access to normal contests only
- **Guest**: View-only access (no participation)

## Common Issues

### MongoDB Connection Error
- Ensure MongoDB is running
- Check MongoDB URI in `.env` file
- Verify network connectivity

### Authentication Error
- Check if token is valid and not expired
- Ensure token is included in Authorization header: `Bearer <token>`
- Verify user exists and is active

### Contest Access Denied
- Normal users cannot access VIP contests
- Guests cannot participate in contests
- Check user role and contest access level

### Rate Limiting
- If you hit rate limits, wait for the window to reset
- Adjust rate limits in `.env` if needed for development

## Next Steps

1. Set up a cron job to automatically finalize prizes for ended contests
2. Add email notifications for prize winners
3. Implement contest analytics and reporting
4. Add contest categories and tags
5. Implement contest search and filtering
6. Add user profile management
7. Implement contest sharing and social features

