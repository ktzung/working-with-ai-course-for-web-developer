# Create Agent: Performance Analyzer

## Learning Objectives
- Build an agent that analyzes application performance
- Identify bottlenecks in frontend and backend
- Generate optimization recommendations

## Why Performance Analysis?

Slow applications lose users. A 1-second delay in page load can reduce conversions by 7%. Performance analysis identifies bottlenecks before users notice them.

## Performance Metrics

### Frontend Metrics
- **First Contentful Paint (FCP)**: Time to first visible content
- **Largest Contentful Paint (LCP)**: Time to largest content element
- **Time to Interactive (TTI)**: Time until page is fully interactive
- **Cumulative Layout Shift (CLS)**: Visual stability
- **Bundle Size**: Total JavaScript downloaded

### Backend Metrics
- **Response Time**: Time to process requests
- **Throughput**: Requests per second
- **Database Query Time**: Time for database operations
- **Memory Usage**: RAM consumption
- **CPU Usage**: Processor utilization

## The Agent Definition

Create `.github/copilot/agents/performance-analyzer.md`:

```markdown
# Performance Analyzer Agent

## Description
Analyzes application performance and generates optimization recommendations.

## Trigger
When user asks to check performance, optimize speed, or analyze bottlenecks.

## Workflow

### Phase 1: Bundle Analysis
Analyze frontend bundle:
1. Run bundle analyzer
2. Identify large dependencies
3. Check for code splitting
4. Review lazy loading implementation

### Phase 2: API Performance
Analyze backend endpoints:
1. Check response times for all endpoints
2. Identify slow database queries
3. Review N+1 query problems
4. Check caching implementation

### Phase 3: Image Optimization
Review image handling:
1. Check image formats (WebP, AVIF)
2. Verify image compression
3. Check for lazy loading
4. Review responsive images

### Phase 4: Caching Strategy
Review caching implementation:
1. Check browser caching headers
2. Review CDN configuration
3. Analyze Redis/memory caching
4. Check React Query cache settings

### Phase 5: Generate Report
Create performance report with:
- Overall performance score
- List of issues by impact
- Optimization recommendations
- Expected improvement for each fix

## Output Format

```markdown
# Performance Analysis Report

## Overall Score: 72/100

## Frontend Issues

### [HIGH] Large Bundle Size (2.5MB)
**Impact**: Slow initial load on mobile networks
**Cause**: Moment.js included (290KB gzipped)
**Fix**: Replace with date-fns (20KB gzipped)
**Expected Improvement**: -270KB bundle size

### [MEDIUM] No Code Splitting
**Impact**: Users download all code upfront
**Fix**: Implement React.lazy() for route-based splitting
**Expected Improvement**: -40% initial bundle

## Backend Issues

### [HIGH] N+1 Query in GET /api/tasks
**Impact**: 500ms response time with 100 tasks
**Cause**: Fetching assignee for each task separately
**Fix**: Use .populate('assignee') in single query
**Expected Improvement**: -450ms response time

### [MEDIUM] No Caching on GET /api/products
**Impact**: Database hit on every request
**Fix**: Add Redis cache with 5-minute TTL
**Expected Improvement**: -90% response time
```

## Implementation

```javascript
// performance-analyzer.js
const fs = require('fs');
const path = require('path');
const { execSync } = require('child_process');

class PerformanceAnalyzer {
  constructor(projectPath) {
    this.projectPath = projectPath;
    this.issues = [];
  }

  async analyze() {
    await this.analyzeBundleSize();
    await this.analyzeAPIPerformance();
    await this.analyzeImageOptimization();
    await this.analyzeCaching();
    return this.generateReport();
  }

  async analyzeBundleSize() {
    // Check for large dependencies
    const packageJson = JSON.parse(
      fs.readFileSync(path.join(this.projectPath, 'package.json'), 'utf-8')
    );

    const largeDependencies = {
      'moment': { size: '290KB', alternative: 'date-fns', altSize: '20KB' },
      'lodash': { size: '72KB', alternative: 'lodash-es', altSize: '25KB' },
      'jquery': { size: '87KB', alternative: 'vanilla JS', altSize: '0KB' }
    };

    const deps = { ...packageJson.dependencies, ...packageJson.devDependencies };

    Object.keys(deps).forEach(dep => {
      if (largeDependencies[dep]) {
        this.issues.push({
          severity: 'HIGH',
          category: 'Bundle Size',
          title: `Large dependency: ${dep}`,
          impact: `Adds ${largeDependencies[dep].size} to bundle`,
          fix: `Replace with ${largeDependencies[dep].alternative} (${largeDependencies[dep].altSize})`
        });
      }
    });

    // Check for code splitting
    const srcFiles = this.getFiles('src', '.tsx');
    const hasLazyLoading = srcFiles.some(file => {
      const content = fs.readFileSync(file, 'utf-8');
      return content.includes('React.lazy') || content.includes('dynamic(');
    });

    if (!hasLazyLoading && srcFiles.length > 10) {
      this.issues.push({
        severity: 'MEDIUM',
        category: 'Bundle Size',
        title: 'No code splitting detected',
        impact: 'Users download all code upfront',
        fix: 'Implement route-based code splitting with React.lazy()'
      });
    }
  }

  async analyzeAPIPerformance() {
    // Check for N+1 queries
    const controllerFiles = this.getFiles('controllers', '.js');

    controllerFiles.forEach(file => {
      const content = fs.readFileSync(file, 'utf-8');

      // Check for loops with database calls
      const loopPattern = /for\s*\(.*\)\s*\{[\s\S]*?(?:find|findOne|findById)/g;
      if (loopPattern.test(content)) {
        this.issues.push({
          severity: 'HIGH',
          category: 'API Performance',
          title: `Potential N+1 query in ${path.basename(file)}`,
          impact: 'Slow response times with many records',
          fix: 'Use populate() or batch queries'
        });
      }
    });

    // Check for missing indexes
    const modelFiles = this.getFiles('models', '.js');
    modelFiles.forEach(file => {
      const content = fs.readFileSync(file, 'utf-8');

      if (!content.includes('.index(')) {
        this.issues.push({
          severity: 'MEDIUM',
          category: 'API Performance',
          title: `No indexes defined in ${path.basename(file)}`,
          impact: 'Slow queries on large collections',
          fix: 'Add indexes for frequently queried fields'
        });
      }
    });
  }

  async analyzeImageOptimization() {
    // Check for image optimization
    const nextConfig = path.join(this.projectPath, 'next.config.js');
    if (fs.existsSync(nextConfig)) {
      const content = fs.readFileSync(nextConfig, 'utf-8');
      if (!content.includes('images')) {
        this.issues.push({
          severity: 'MEDIUM',
          category: 'Image Optimization',
          title: 'Next.js image optimization not configured',
          impact: 'Larger image sizes, slower loads',
          fix: 'Configure next/image with proper domains and formats'
        });
      }
    }
  }

  async analyzeCaching() {
    // Check for caching headers
    const appFile = path.join(this.projectPath, 'app.js');
    if (fs.existsSync(appFile)) {
      const content = fs.readFileSync(appFile, 'utf-8');
      if (!content.includes('Cache-Control')) {
        this.issues.push({
          severity: 'MEDIUM',
          category: 'Caching',
          title: 'No cache headers configured',
          impact: 'Repeated downloads of static assets',
          fix: 'Add Cache-Control headers for static files'
        });
      }
    }
  }

  generateReport() {
    const high = this.issues.filter(i => i.severity === 'HIGH').length;
    const medium = this.issues.filter(i => i.severity === 'MEDIUM').length;
    const low = this.issues.filter(i => i.severity === 'LOW').length;

    const score = Math.max(0, 100 - (high * 20) - (medium * 10) - (low * 5));

    return {
      score,
      summary: { high, medium, low },
      issues: this.issues
    };
  }
}

module.exports = PerformanceAnalyzer;
```

## AI Prompt for Performance Agent

```
Create a performance analyzer agent that:
1. Analyzes frontend bundle size
2. Identifies slow API endpoints
3. Checks for N+1 query problems
4. Reviews caching implementation
5. Generates optimization recommendations

Include the agent definition and implementation code.
```

## Practice Exercise

Build and run the performance analyzer:
1. Create the agent definition file
2. Implement the analyzer logic
3. Run it on your Task Management app
4. Review the findings
5. Implement the top 3 optimizations

## Key Takeaways

- Performance agents identify bottlenecks automatically
- Check bundle size, API performance, and caching
- Generate actionable optimization recommendations
- Run regularly to maintain performance standards
