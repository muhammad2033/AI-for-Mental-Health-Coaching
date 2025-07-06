# MindMend AI - Emotion-Aware Mental Health Coach

## Overview

MindMend AI is an intelligent therapeutic chatbot powered by emotion-aware AI, designed to act as a digital mental health coach. It interprets user mood, tone, language patterns, and journaling behavior to adapt its responses and therapy strategies in real time.

## Features

### 🧠 AI-Powered Chat Interface
- Emotion-aware responses using sentiment analysis
- Evidence-based therapy techniques (CBT/DBT)
- Real-time emotional state detection
- Adaptive conversation flow based on user mood

### 📊 Mood Tracking
- Daily mood logging with 1-10 scale
- Emotion tagging system
- Visual analytics and trend analysis
- Progress tracking over time

### 📝 Personal Journal
- Secure journaling with sentiment analysis
- Automatic mood detection from entries
- Tag-based organization
- Emotional insights and patterns

### 📈 Progress Dashboard
- Comprehensive wellness metrics
- Activity tracking and statistics
- Recent activities timeline
- Quick action buttons

### 🎨 Design Features
- Calming therapeutic color palette
- Responsive design for all devices
- Smooth animations and micro-interactions
- Accessibility-focused interface
- Modern card-based layouts

## Technology Stack

- **Frontend**: React 18 with TypeScript
- **Styling**: Tailwind CSS
- **Icons**: Lucide React
- **Build Tool**: Vite
- **Development**: ESLint, TypeScript ESLint

## Project Structure

```
src/
├── components/           # React components
│   ├── Header.tsx       # Application header
│   ├── Navigation.tsx   # Sidebar navigation
│   ├── Dashboard.tsx    # Main dashboard
│   ├── ChatInterface.tsx # AI chat interface
│   ├── MoodTracker.tsx  # Mood logging and analytics
│   └── Journal.tsx      # Personal journaling
├── types/               # TypeScript type definitions
│   └── index.ts        # Core types and interfaces
├── utils/               # Utility functions
│   └── aiResponses.ts  # AI response generation and emotion analysis
├── App.tsx             # Main application component
├── main.tsx            # Application entry point
└── index.css           # Global styles
```

## Installation & Setup

### Prerequisites
- Node.js (version 16 or higher)
- npm or yarn package manager

### Installation Steps

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd mindmend-ai
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm run dev
   ```

4. **Open in browser**
   Navigate to `http://localhost:5173`

### Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run lint` - Run ESLint

## Core Components

### ChatInterface
The main therapeutic chat interface that:
- Analyzes user messages for emotional content
- Generates appropriate AI responses based on detected emotions
- Provides typing indicators and smooth message flow
- Displays emotion analysis confidence levels

### MoodTracker
Comprehensive mood tracking system featuring:
- 1-10 mood scale with visual indicators
- Multi-select emotion tagging
- Optional notes and trigger identification
- Historical mood data visualization

### Journal
Personal journaling system with:
- Rich text entry capabilities
- Automatic sentiment analysis
- Tag-based organization
- Mood correlation tracking

### Dashboard
Central hub displaying:
- Key wellness metrics and statistics
- Recent activity timeline
- Quick action buttons
- Progress visualization

## AI Response System

### Emotion Analysis
The `analyzeEmotion` function performs:
- Keyword-based sentiment analysis
- Confidence scoring
- Multi-dimensional emotion detection (anxiety, depression, stress, happiness, calm)

### Response Generation
The `generateAIResponse` function provides:
- Context-aware therapeutic responses
- Evidence-based intervention suggestions
- Adaptive conversation strategies
- Crisis-sensitive language patterns

## Data Models

### Message Interface
```typescript
interface Message {
  id: string;
  text: string;
  sender: 'user' | 'ai';
  timestamp: Date;
  emotion?: EmotionAnalysis;
}
```

### MoodEntry Interface
```typescript
interface MoodEntry {
  id: string;
  date: Date;
  mood: number; // 1-10 scale
  emotions: string[];
  notes?: string;
  triggers?: string[];
}
```

### JournalEntry Interface
```typescript
interface JournalEntry {
  id: string;
  date: Date;
  title: string;
  content: string;
  mood: number;
  tags: string[];
  analysis?: EmotionAnalysis;
}
```

## Therapeutic Approaches

### Cognitive Behavioral Therapy (CBT)
- Thought challenging techniques
- Behavioral activation strategies
- Cognitive restructuring methods
- Mindfulness integration

### Dialectical Behavior Therapy (DBT)
- TIPP technique implementation
- Distress tolerance skills
- Emotional regulation strategies
- Interpersonal effectiveness tools

## Security & Privacy

- All data stored locally in browser
- No external data transmission
- Secure conversation interface
- Privacy-focused design principles

## Accessibility Features

- WCAG 2.1 compliant color contrast
- Keyboard navigation support
- Screen reader compatibility
- Responsive design for all devices

## Browser Support

- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## License

This project is licensed under the MIT License.

## Support

For support and questions, please open an issue in the repository.

## Disclaimer

MindMend AI is designed to provide supportive guidance and is not a replacement for professional mental health treatment. If you're experiencing a mental health crisis, please contact emergency services or a mental health professional immediately.