# MindMend AI - Setup Guide

## Quick Start

### 1. Prerequisites Check
Before starting, ensure you have:
- Node.js 16+ installed
- npm or yarn package manager
- Git (for version control)
- A modern web browser

### 2. Installation

```bash
# Clone the repository (if from git)
git clone <repository-url>
cd mindmend-ai

# Install dependencies
npm install

# Start development server
npm run dev
```

### 3. Access the Application
Open your browser and navigate to: `http://localhost:5173`

## Detailed Setup Instructions

### Step 1: Environment Setup

#### Install Node.js
1. Visit [nodejs.org](https://nodejs.org/)
2. Download the LTS version
3. Run the installer
4. Verify installation:
   ```bash
   node --version
   npm --version
   ```

#### Install Git (Optional)
1. Visit [git-scm.com](https://git-scm.com/)
2. Download and install
3. Configure:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

### Step 2: Project Setup

#### Option A: From Source Code
If you have the source code directly:
```bash
# Navigate to project directory
cd mindmend-ai

# Install dependencies
npm install
```

#### Option B: From Git Repository
If cloning from a repository:
```bash
# Clone the repository
git clone <repository-url>
cd mindmend-ai

# Install dependencies
npm install
```

### Step 3: Development Server

```bash
# Start the development server
npm run dev

# The application will be available at:
# http://localhost:5173
```

### Step 4: Verify Installation

1. Open `http://localhost:5173` in your browser
2. You should see the MindMend AI welcome screen
3. Test navigation between different sections:
   - Dashboard
   - AI Coach (Chat)
   - Mood Tracker
   - Journal

## Available Scripts

### Development
```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run preview      # Preview production build locally
npm run lint         # Run code linting
```

### Production Build
```bash
# Create production build
npm run build

# The build files will be in the 'dist' directory
# You can serve these files with any static file server
```

## Configuration Options

### Environment Variables
Create a `.env` file in the root directory:

```env
# Application Configuration
VITE_APP_NAME=MindMend AI
VITE_APP_VERSION=1.0.0

# Development Settings
VITE_DEV_PORT=5173
VITE_DEV_HOST=localhost
```

### Customization
You can customize the application by modifying:

- **Colors**: Edit `tailwind.config.js` for theme colors
- **Components**: Modify files in `src/components/`
- **AI Responses**: Update `src/utils/aiResponses.ts`
- **Types**: Extend interfaces in `src/types/index.ts`

## Troubleshooting

### Common Issues

#### Port Already in Use
```bash
# If port 5173 is busy, Vite will automatically try the next available port
# Or specify a different port:
npm run dev -- --port 3000
```

#### Node Version Issues
```bash
# Check your Node.js version
node --version

# If you need to upgrade:
# Visit nodejs.org and download the latest LTS version
```

#### Dependency Installation Errors
```bash
# Clear npm cache
npm cache clean --force

# Delete node_modules and reinstall
rm -rf node_modules package-lock.json
npm install
```

#### Build Errors
```bash
# Check for TypeScript errors
npm run lint

# Clear build cache
rm -rf dist
npm run build
```

### Performance Issues

#### Slow Development Server
- Close unnecessary browser tabs
- Restart the development server
- Check system resources (RAM, CPU)

#### Large Bundle Size
- Run build analysis:
  ```bash
  npm run build
  # Check the dist/ folder size
  ```

## IDE Setup

### Visual Studio Code
Recommended extensions:
1. ES7+ React/Redux/React-Native snippets
2. TypeScript Importer
3. Tailwind CSS IntelliSense
4. ESLint
5. Prettier - Code formatter
6. Auto Rename Tag
7. Bracket Pair Colorizer

### Settings
Create `.vscode/settings.json`:
```json
{
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

## Deployment

### Static Hosting (Recommended)

#### Netlify
1. Build the project: `npm run build`
2. Drag the `dist` folder to Netlify
3. Configure redirects for SPA routing

#### Vercel
1. Install Vercel CLI: `npm i -g vercel`
2. Run: `vercel`
3. Follow the prompts

#### GitHub Pages
1. Install gh-pages: `npm install --save-dev gh-pages`
2. Add to package.json:
   ```json
   "scripts": {
     "deploy": "gh-pages -d dist"
   }
   ```
3. Run: `npm run build && npm run deploy`

### Server Configuration
For SPA routing, configure your server to:
- Serve `index.html` for all routes
- Enable gzip compression
- Set appropriate cache headers

## Security Considerations

### Development
- Never commit sensitive data
- Use environment variables for configuration
- Keep dependencies updated

### Production
- Enable HTTPS
- Implement Content Security Policy
- Regular security audits: `npm audit`

## Getting Help

### Documentation
- Check this README.md
- Review component documentation in source files
- TypeScript definitions provide inline help

### Community Support
- Open an issue in the repository
- Check existing issues for solutions
- Contribute improvements back to the project

### Professional Support
For enterprise deployments or custom modifications, consider:
- Code review services
- Custom feature development
- Performance optimization
- Security auditing

## Next Steps

After successful setup:

1. **Explore the Application**
   - Try the AI chat interface
   - Log some mood entries
   - Write a journal entry
   - Check the dashboard analytics

2. **Customize for Your Needs**
   - Modify AI response patterns
   - Adjust color themes
   - Add new features
   - Integrate with external APIs

3. **Deploy to Production**
   - Choose a hosting platform
   - Configure domain and SSL
   - Set up monitoring
   - Plan for updates and maintenance

4. **Contribute Back**
   - Report bugs
   - Suggest improvements
   - Submit pull requests
   - Share your experience

## License

This project is open source and available under the MIT License.