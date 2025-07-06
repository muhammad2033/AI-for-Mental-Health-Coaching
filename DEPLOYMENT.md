# MindMend AI - Deployment Guide

## Overview

This guide covers deploying MindMend AI to various hosting platforms. The application is built as a static single-page application (SPA) that can be deployed to any static hosting service.

## Prerequisites

Before deploying, ensure you have:
- Node.js 16+ installed locally
- Project built successfully (`npm run build`)
- Access to your chosen hosting platform
- Domain name (optional, for custom domains)

## Build Process

### 1. Production Build

```bash
# Install dependencies
npm install

# Create production build
npm run build

# Verify build
ls -la dist/
```

The build process creates a `dist/` directory containing:
- `index.html` - Main HTML file
- `assets/` - Optimized CSS, JS, and other assets
- Static files (favicon, etc.)

### 2. Build Optimization

The production build includes:
- **Minification**: JavaScript and CSS are minified
- **Tree Shaking**: Unused code is removed
- **Asset Optimization**: Images and fonts are optimized
- **Code Splitting**: Separate chunks for better caching

## Deployment Platforms

### Netlify (Recommended)

#### Method 1: Drag and Drop
1. Build the project: `npm run build`
2. Go to [Netlify](https://netlify.com)
3. Drag the `dist/` folder to the deployment area
4. Your site will be live with a random URL

#### Method 2: Git Integration
1. Push your code to GitHub/GitLab/Bitbucket
2. Connect your repository to Netlify
3. Configure build settings:
   - **Build command**: `npm run build`
   - **Publish directory**: `dist`
4. Deploy automatically on every push

#### Method 3: Netlify CLI
```bash
# Install Netlify CLI
npm install -g netlify-cli

# Login to Netlify
netlify login

# Deploy
netlify deploy --prod --dir=dist
```

#### Netlify Configuration
Create `netlify.toml` in project root:
```toml
[build]
  publish = "dist"
  command = "npm run build"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[build.environment]
  NODE_VERSION = "18"
```

### Vercel

#### Method 1: Vercel CLI
```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
vercel

# Follow the prompts
```

#### Method 2: Git Integration
1. Push code to GitHub
2. Import project on [Vercel](https://vercel.com)
3. Configure:
   - **Framework Preset**: Vite
   - **Build Command**: `npm run build`
   - **Output Directory**: `dist`

#### Vercel Configuration
Create `vercel.json` in project root:
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

### GitHub Pages

#### Setup
1. Install gh-pages:
   ```bash
   npm install --save-dev gh-pages
   ```

2. Add to `package.json`:
   ```json
   {
     "scripts": {
       "deploy": "gh-pages -d dist"
     },
     "homepage": "https://yourusername.github.io/mindmend-ai"
   }
   ```

3. Deploy:
   ```bash
   npm run build
   npm run deploy
   ```

#### GitHub Actions (Automated)
Create `.github/workflows/deploy.yml`:
```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Build
      run: npm run build
    
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

### Firebase Hosting

#### Setup
1. Install Firebase CLI:
   ```bash
   npm install -g firebase-tools
   ```

2. Login and initialize:
   ```bash
   firebase login
   firebase init hosting
   ```

3. Configure `firebase.json`:
   ```json
   {
     "hosting": {
       "public": "dist",
       "ignore": [
         "firebase.json",
         "**/.*",
         "**/node_modules/**"
       ],
       "rewrites": [
         {
           "source": "**",
           "destination": "/index.html"
         }
       ]
     }
   }
   ```

4. Deploy:
   ```bash
   npm run build
   firebase deploy
   ```

### AWS S3 + CloudFront

#### S3 Setup
1. Create S3 bucket
2. Enable static website hosting
3. Upload `dist/` contents
4. Configure bucket policy:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Sid": "PublicReadGetObject",
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::your-bucket-name/*"
       }
     ]
   }
   ```

#### CloudFront Setup
1. Create CloudFront distribution
2. Set S3 bucket as origin
3. Configure error pages:
   - Error Code: 403, 404
   - Response Page Path: `/index.html`
   - Response Code: 200

#### AWS CLI Deployment
```bash
# Install AWS CLI
pip install awscli

# Configure credentials
aws configure

# Sync files
aws s3 sync dist/ s3://your-bucket-name --delete

# Invalidate CloudFront cache
aws cloudfront create-invalidation --distribution-id YOUR_DISTRIBUTION_ID --paths "/*"
```

## Custom Domain Setup

### DNS Configuration
For most platforms, add these DNS records:

#### Apex Domain (example.com)
```
Type: A
Name: @
Value: [Platform's IP addresses]
```

#### Subdomain (www.example.com)
```
Type: CNAME
Name: www
Value: [Platform's domain]
```

### Platform-Specific Instructions

#### Netlify
1. Go to Site Settings → Domain Management
2. Add custom domain
3. Configure DNS as instructed
4. SSL certificate is automatically provisioned

#### Vercel
1. Go to Project Settings → Domains
2. Add domain
3. Configure DNS records
4. SSL is automatically handled

#### GitHub Pages
1. Go to Repository Settings → Pages
2. Add custom domain in "Custom domain" field
3. Create `CNAME` file in repository root with your domain

## SSL/HTTPS Configuration

### Automatic SSL (Recommended)
Most modern hosting platforms provide automatic SSL:
- **Netlify**: Automatic Let's Encrypt certificates
- **Vercel**: Automatic SSL for all domains
- **Firebase**: Automatic SSL provisioning
- **GitHub Pages**: Automatic HTTPS for custom domains

### Manual SSL Setup
For custom servers:
1. Obtain SSL certificate (Let's Encrypt recommended)
2. Configure web server (Nginx/Apache)
3. Set up automatic renewal

## Environment Variables

### Build-time Variables
Create `.env.production`:
```env
VITE_APP_NAME=MindMend AI
VITE_APP_VERSION=1.0.0
VITE_API_BASE_URL=https://api.mindmend.com
```

### Platform-specific Configuration

#### Netlify
Set in Site Settings → Environment Variables

#### Vercel
Set in Project Settings → Environment Variables

#### GitHub Actions
Set in Repository Settings → Secrets and Variables

## Performance Optimization

### Caching Strategy
Configure cache headers:
```
# Static assets (1 year)
/assets/* Cache-Control: public, max-age=31536000, immutable

# HTML files (no cache)
/*.html Cache-Control: no-cache

# Service worker
/sw.js Cache-Control: no-cache
```

### CDN Configuration
- Enable gzip/brotli compression
- Set appropriate cache headers
- Configure geographic distribution
- Enable HTTP/2

### Monitoring
Set up monitoring for:
- **Uptime**: Service availability
- **Performance**: Core Web Vitals
- **Errors**: JavaScript errors and failed requests
- **Analytics**: User behavior and usage patterns

## Security Configuration

### Content Security Policy (CSP)
Add to HTML head or server headers:
```html
<meta http-equiv="Content-Security-Policy" content="
  default-src 'self';
  script-src 'self' 'unsafe-inline';
  style-src 'self' 'unsafe-inline';
  img-src 'self' data: https:;
  font-src 'self';
  connect-src 'self';
">
```

### Security Headers
Configure these headers:
```
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
X-XSS-Protection: 1; mode=block
Referrer-Policy: strict-origin-when-cross-origin
Permissions-Policy: geolocation=(), microphone=(), camera=()
```

## Deployment Checklist

### Pre-deployment
- [ ] Run `npm run build` successfully
- [ ] Test production build locally (`npm run preview`)
- [ ] Verify all features work in production mode
- [ ] Check console for errors
- [ ] Test on different devices and browsers
- [ ] Validate HTML and accessibility

### Post-deployment
- [ ] Verify site loads correctly
- [ ] Test all navigation and features
- [ ] Check mobile responsiveness
- [ ] Verify SSL certificate
- [ ] Test performance (Lighthouse)
- [ ] Set up monitoring and analytics
- [ ] Configure error tracking

## Troubleshooting

### Common Issues

#### 404 Errors on Refresh
**Problem**: SPA routes return 404 when accessed directly
**Solution**: Configure server to serve `index.html` for all routes

#### Build Failures
**Problem**: Build fails with memory errors
**Solution**: Increase Node.js memory limit:
```bash
NODE_OPTIONS="--max-old-space-size=4096" npm run build
```

#### Slow Loading
**Problem**: Large bundle size
**Solution**: 
- Analyze bundle with `npm run build -- --analyze`
- Implement code splitting
- Optimize images and assets

#### CORS Errors
**Problem**: API calls fail due to CORS
**Solution**: Configure CORS headers on API server or use proxy

### Platform-specific Issues

#### Netlify
- **Build timeouts**: Increase build timeout in site settings
- **Function errors**: Check function logs in Netlify dashboard

#### Vercel
- **Serverless function limits**: Optimize function size and execution time
- **Build errors**: Check build logs in Vercel dashboard

#### GitHub Pages
- **CNAME conflicts**: Ensure CNAME file contains only domain name
- **Build failures**: Check Actions tab for detailed error logs

## Maintenance

### Regular Tasks
- **Dependency Updates**: Monthly security updates
- **Performance Monitoring**: Weekly performance checks
- **Backup**: Regular backup of deployment configurations
- **SSL Renewal**: Monitor certificate expiration (usually automatic)

### Monitoring Setup
```bash
# Example monitoring script
#!/bin/bash
curl -f https://your-domain.com || echo "Site is down!"
```

### Update Process
1. Test changes locally
2. Deploy to staging environment (if available)
3. Run automated tests
4. Deploy to production
5. Monitor for issues
6. Rollback if necessary

This deployment guide provides comprehensive instructions for deploying MindMend AI to various platforms while ensuring optimal performance, security, and reliability.