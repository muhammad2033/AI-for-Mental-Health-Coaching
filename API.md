# MindMend AI - API Documentation

## Overview

MindMend AI currently operates as a client-side application with built-in AI response generation and emotion analysis. This document outlines the internal API structure and provides guidance for future backend integration.

## Current Architecture

### Internal APIs

The application uses internal utility functions that simulate API behavior:

#### Emotion Analysis API

```typescript
// Location: src/utils/aiResponses.ts
function analyzeEmotion(text: string): EmotionAnalysis
```

**Purpose**: Analyzes text input for emotional content and sentiment

**Parameters**:
- `text` (string): The text to analyze

**Returns**: `EmotionAnalysis` object
```typescript
interface EmotionAnalysis {
  sentiment: 'positive' | 'negative' | 'neutral';
  confidence: number; // 0-1 scale
  emotions: {
    anxiety: number;    // 0-1 scale
    depression: number; // 0-1 scale
    stress: number;     // 0-1 scale
    happiness: number;  // 0-1 scale
    calm: number;       // 0-1 scale
  };
}
```

**Example Usage**:
```typescript
const analysis = analyzeEmotion("I'm feeling really anxious about tomorrow's presentation");
// Returns:
// {
//   sentiment: 'negative',
//   confidence: 0.8,
//   emotions: {
//     anxiety: 0.9,
//     depression: 0.1,
//     stress: 0.7,
//     happiness: 0.1,
//     calm: 0.1
//   }
// }
```

#### AI Response Generation API

```typescript
// Location: src/utils/aiResponses.ts
function generateAIResponse(userMessage: string, emotion: EmotionAnalysis): string
```

**Purpose**: Generates contextually appropriate therapeutic responses

**Parameters**:
- `userMessage` (string): The user's input message
- `emotion` (EmotionAnalysis): Emotion analysis of the user's message

**Returns**: String containing the AI's response

**Response Categories**:
1. **High Anxiety Responses**: Grounding techniques, breathing exercises
2. **Depression Support**: Validation, small steps encouragement
3. **Positive Reinforcement**: Building on positive emotions
4. **General Support**: Neutral, supportive responses

**Example Usage**:
```typescript
const userMessage = "I'm having trouble sleeping due to work stress";
const emotion = analyzeEmotion(userMessage);
const response = generateAIResponse(userMessage, emotion);
// Returns: "I can sense you're feeling stressed about work, which is affecting your sleep..."
```

#### Therapeutic Techniques APIs

```typescript
// CBT Techniques
function getCBTTechniques(): string[]

// DBT Techniques  
function getDBTTechniques(): string[]
```

**Purpose**: Provides evidence-based therapeutic technique suggestions

**Returns**: Array of technique descriptions and instructions

## Data Models

### Core Interfaces

#### Message Interface
```typescript
interface Message {
  id: string;           // Unique identifier
  text: string;         // Message content
  sender: 'user' | 'ai'; // Message sender
  timestamp: Date;      // When message was sent
  emotion?: EmotionAnalysis; // Optional emotion analysis
}
```

#### MoodEntry Interface
```typescript
interface MoodEntry {
  id: string;           // Unique identifier
  date: Date;           // Entry date
  mood: number;         // 1-10 mood scale
  emotions: string[];   // Selected emotion tags
  notes?: string;       // Optional notes
  triggers?: string[];  // Optional trigger identification
}
```

#### JournalEntry Interface
```typescript
interface JournalEntry {
  id: string;           // Unique identifier
  date: Date;           // Entry date
  title: string;        // Entry title
  content: string;      // Entry content
  mood: number;         // Associated mood (1-10)
  tags: string[];       // Entry tags
  analysis?: EmotionAnalysis; // Optional sentiment analysis
}
```

#### TherapyTechnique Interface
```typescript
interface TherapyTechnique {
  id: string;           // Unique identifier
  name: string;         // Technique name
  type: 'CBT' | 'DBT' | 'mindfulness'; // Technique category
  description: string;  // Technique description
  steps: string[];      // Implementation steps
}
```

## Local Storage API

### Storage Keys
```typescript
const STORAGE_KEYS = {
  MESSAGES: 'mindmend_messages',
  MOOD_ENTRIES: 'mindmend_mood_entries',
  JOURNAL_ENTRIES: 'mindmend_journal_entries',
  USER_PREFERENCES: 'mindmend_preferences'
};
```

### Storage Operations

#### Save Data
```typescript
// Save messages
localStorage.setItem(STORAGE_KEYS.MESSAGES, JSON.stringify(messages));

// Save mood entries
localStorage.setItem(STORAGE_KEYS.MOOD_ENTRIES, JSON.stringify(moodEntries));

// Save journal entries
localStorage.setItem(STORAGE_KEYS.JOURNAL_ENTRIES, JSON.stringify(journalEntries));
```

#### Load Data
```typescript
// Load messages
const savedMessages = localStorage.getItem(STORAGE_KEYS.MESSAGES);
const messages = savedMessages ? JSON.parse(savedMessages) : [];

// Load mood entries
const savedMoodEntries = localStorage.getItem(STORAGE_KEYS.MOOD_ENTRIES);
const moodEntries = savedMoodEntries ? JSON.parse(savedMoodEntries) : [];
```

## Future Backend API Specification

### Authentication Endpoints

#### POST /api/auth/register
Register a new user account

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "firstName": "John",
  "lastName": "Doe"
}
```

**Response**:
```json
{
  "success": true,
  "user": {
    "id": "user_123",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe"
  },
  "token": "jwt_token_here"
}
```

#### POST /api/auth/login
Authenticate user login

**Request Body**:
```json
{
  "email": "user@example.com",
  "password": "securePassword123"
}
```

**Response**:
```json
{
  "success": true,
  "user": {
    "id": "user_123",
    "email": "user@example.com",
    "firstName": "John",
    "lastName": "Doe"
  },
  "token": "jwt_token_here"
}
```

### Chat Endpoints

#### POST /api/chat/message
Send a message and receive AI response

**Headers**:
```
Authorization: Bearer jwt_token_here
Content-Type: application/json
```

**Request Body**:
```json
{
  "message": "I'm feeling anxious about my presentation tomorrow",
  "sessionId": "session_123"
}
```

**Response**:
```json
{
  "success": true,
  "userMessage": {
    "id": "msg_456",
    "text": "I'm feeling anxious about my presentation tomorrow",
    "sender": "user",
    "timestamp": "2024-01-15T10:30:00Z",
    "emotion": {
      "sentiment": "negative",
      "confidence": 0.8,
      "emotions": {
        "anxiety": 0.9,
        "depression": 0.1,
        "stress": 0.7,
        "happiness": 0.1,
        "calm": 0.1
      }
    }
  },
  "aiResponse": {
    "id": "msg_457",
    "text": "I can sense you're feeling anxious about your presentation...",
    "sender": "ai",
    "timestamp": "2024-01-15T10:30:05Z",
    "techniques": ["breathing_exercise", "cognitive_restructuring"]
  }
}
```

#### GET /api/chat/history
Retrieve chat history

**Headers**:
```
Authorization: Bearer jwt_token_here
```

**Query Parameters**:
- `limit` (optional): Number of messages to retrieve (default: 50)
- `offset` (optional): Pagination offset (default: 0)
- `sessionId` (optional): Specific session ID

**Response**:
```json
{
  "success": true,
  "messages": [
    {
      "id": "msg_456",
      "text": "I'm feeling anxious about my presentation tomorrow",
      "sender": "user",
      "timestamp": "2024-01-15T10:30:00Z",
      "emotion": { /* emotion analysis */ }
    }
  ],
  "pagination": {
    "total": 150,
    "limit": 50,
    "offset": 0,
    "hasMore": true
  }
}
```

### Mood Tracking Endpoints

#### POST /api/mood/entry
Create a new mood entry

**Headers**:
```
Authorization: Bearer jwt_token_here
Content-Type: application/json
```

**Request Body**:
```json
{
  "mood": 7,
  "emotions": ["happy", "grateful", "calm"],
  "notes": "Had a great day with family",
  "triggers": []
}
```

**Response**:
```json
{
  "success": true,
  "entry": {
    "id": "mood_789",
    "date": "2024-01-15T10:30:00Z",
    "mood": 7,
    "emotions": ["happy", "grateful", "calm"],
    "notes": "Had a great day with family",
    "triggers": []
  }
}
```

#### GET /api/mood/entries
Retrieve mood entries

**Headers**:
```
Authorization: Bearer jwt_token_here
```

**Query Parameters**:
- `startDate` (optional): Start date for filtering (ISO 8601)
- `endDate` (optional): End date for filtering (ISO 8601)
- `limit` (optional): Number of entries to retrieve

**Response**:
```json
{
  "success": true,
  "entries": [
    {
      "id": "mood_789",
      "date": "2024-01-15T10:30:00Z",
      "mood": 7,
      "emotions": ["happy", "grateful", "calm"],
      "notes": "Had a great day with family"
    }
  ],
  "statistics": {
    "averageMood": 6.8,
    "totalEntries": 45,
    "moodTrend": "improving"
  }
}
```

### Journal Endpoints

#### POST /api/journal/entry
Create a new journal entry

**Headers**:
```
Authorization: Bearer jwt_token_here
Content-Type: application/json
```

**Request Body**:
```json
{
  "title": "Reflection on Progress",
  "content": "Today I feel like I'm making real progress...",
  "mood": 8,
  "tags": ["progress", "anxiety", "breathing"]
}
```

**Response**:
```json
{
  "success": true,
  "entry": {
    "id": "journal_101",
    "date": "2024-01-15T10:30:00Z",
    "title": "Reflection on Progress",
    "content": "Today I feel like I'm making real progress...",
    "mood": 8,
    "tags": ["progress", "anxiety", "breathing"],
    "analysis": {
      "sentiment": "positive",
      "confidence": 0.8,
      "emotions": { /* emotion breakdown */ }
    }
  }
}
```

#### GET /api/journal/entries
Retrieve journal entries

**Headers**:
```
Authorization: Bearer jwt_token_here
```

**Query Parameters**:
- `limit` (optional): Number of entries to retrieve
- `offset` (optional): Pagination offset
- `tags` (optional): Filter by tags (comma-separated)
- `search` (optional): Search in title and content

**Response**:
```json
{
  "success": true,
  "entries": [
    {
      "id": "journal_101",
      "date": "2024-01-15T10:30:00Z",
      "title": "Reflection on Progress",
      "content": "Today I feel like I'm making real progress...",
      "mood": 8,
      "tags": ["progress", "anxiety", "breathing"]
    }
  ],
  "pagination": {
    "total": 25,
    "limit": 10,
    "offset": 0,
    "hasMore": true
  }
}
```

### Analytics Endpoints

#### GET /api/analytics/dashboard
Get dashboard analytics

**Headers**:
```
Authorization: Bearer jwt_token_here
```

**Response**:
```json
{
  "success": true,
  "analytics": {
    "daysActive": 28,
    "averageMood": 7.2,
    "totalSessions": 45,
    "progressScore": 85,
    "moodTrend": "improving",
    "recentActivities": [
      {
        "type": "mood",
        "message": "Logged mood: 8/10 - Feeling optimistic",
        "timestamp": "2024-01-15T08:00:00Z"
      }
    ]
  }
}
```

#### GET /api/analytics/mood-trends
Get detailed mood trend analysis

**Headers**:
```
Authorization: Bearer jwt_token_here
```

**Query Parameters**:
- `period` (optional): 'week', 'month', 'quarter', 'year' (default: 'month')

**Response**:
```json
{
  "success": true,
  "trends": {
    "period": "month",
    "averageMood": 6.8,
    "moodData": [
      {
        "date": "2024-01-01",
        "mood": 6.5,
        "emotions": ["calm", "neutral"]
      }
    ],
    "insights": [
      "Your mood has improved by 15% this month",
      "You tend to feel better on weekends"
    ]
  }
}
```

## Error Handling

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input data",
    "details": {
      "field": "mood",
      "reason": "Must be between 1 and 10"
    }
  }
}
```

### Common Error Codes
- `AUTHENTICATION_REQUIRED`: User not authenticated
- `INVALID_TOKEN`: JWT token is invalid or expired
- `VALIDATION_ERROR`: Request data validation failed
- `NOT_FOUND`: Requested resource not found
- `RATE_LIMIT_EXCEEDED`: Too many requests
- `SERVER_ERROR`: Internal server error

## Rate Limiting

### Limits
- Authentication endpoints: 5 requests per minute
- Chat endpoints: 30 requests per minute
- Data endpoints: 100 requests per minute
- Analytics endpoints: 20 requests per minute

### Headers
```
X-RateLimit-Limit: 30
X-RateLimit-Remaining: 25
X-RateLimit-Reset: 1642694400
```

## Security

### Authentication
- JWT tokens with 24-hour expiration
- Refresh tokens for extended sessions
- Secure password hashing (bcrypt)

### Data Protection
- HTTPS required for all endpoints
- Input validation and sanitization
- SQL injection prevention
- XSS protection headers

### Privacy
- Data encryption at rest
- Minimal data collection
- User data deletion on request
- GDPR compliance

## WebSocket API (Future)

### Real-time Features

#### Connection
```javascript
const ws = new WebSocket('wss://api.mindmend.com/ws');
ws.onopen = () => {
  ws.send(JSON.stringify({
    type: 'authenticate',
    token: 'jwt_token_here'
  }));
};
```

#### Message Types
- `typing_indicator`: Show/hide typing status
- `mood_update`: Real-time mood changes
- `session_start`: Begin therapy session
- `session_end`: End therapy session

This API documentation provides a comprehensive guide for both current internal APIs and future backend integration possibilities.