# Create Agent: Security Scanner

## Learning Objectives
- Build an agent that scans for security vulnerabilities
- Check for common web security issues
- Generate actionable security reports

## Why a Security Scanner?

Security vulnerabilities can lead to data breaches, financial loss, and damaged reputation. An automated security scanner catches issues before they reach production.

## Common Web Security Issues

1. **SQL Injection**: Unsanitized user input in database queries
2. **XSS (Cross-Site Scripting)**: Unescaped user content in HTML
3. **CSRF (Cross-Site Request Forgery)**: Missing CSRF tokens
4. **Insecure Authentication**: Weak passwords, missing rate limiting
5. **Sensitive Data Exposure**: Logging passwords, missing encryption
6. **Broken Access Control**: Missing authorization checks
7. **Vulnerable Dependencies**: Outdated packages with known CVEs

## The Agent Definition

Create `.github/copilot/agents/security-scanner.md`:

```markdown
# Security Scanner Agent

## Description
Scans the codebase for security vulnerabilities and generates actionable reports.

## Trigger
When user asks to check security, scan for vulnerabilities, or audit the codebase.

## Workflow

### Phase 1: Static Analysis
Scan all source files for dangerous patterns:

#### SQL Injection Risks
- Search for string concatenation in queries
- Check for raw SQL without parameterization
- Flag: `query("SELECT * FROM users WHERE id = " + userId)`

#### XSS Risks
- Search for `dangerouslySetInnerHTML`
- Check for unescaped user input in JSX
- Flag: `<div dangerouslySetInnerHTML={{__html: userContent}} />`

#### Sensitive Data Exposure
- Search for hardcoded secrets (API keys, passwords)
- Check for passwords in logs
- Flag: `console.log('Password:', password)`

#### Insecure Dependencies
- Run `npm audit` and parse results
- Check for outdated packages
- Flag packages with known CVEs

### Phase 2: Authentication Review
Check authentication implementation:

1. **Password Hashing**: Verify bcrypt or similar is used
2. **JWT Security**: Check for proper token validation
3. **Rate Limiting**: Verify login endpoints have rate limiting
4. **Session Management**: Check for secure cookie settings

### Phase 3: Authorization Review
Check access control:

1. **Route Protection**: Verify all sensitive routes have auth middleware
2. **Role-Based Access**: Check for proper role validation
3. **Resource Ownership**: Verify users can only access their own data

### Phase 4: Input Validation
Check input handling:

1. **Request Validation**: Verify all inputs are validated
2. **File Upload Security**: Check file type and size validation
3. **SQL Parameterization**: Verify queries use parameters

### Phase 5: Generate Report
Create security report with:
- Summary of findings
- Detailed list of vulnerabilities
- Severity levels (Critical, High, Medium, Low)
- Suggested fixes for each issue
- References to OWASP guidelines

## Output Format

```markdown
# Security Scan Report

## Summary
- Critical: X issues
- High: X issues
- Medium: X issues
- Low: X issues

## Findings

### [CRITICAL] SQL Injection in routes/users.js:45
**Description**: User input directly concatenated in SQL query
**Code**: `query("SELECT * FROM users WHERE id = " + req.params.id)`
**Fix**: Use parameterized queries
**Reference**: https://owasp.org/www-community/attacks/SQL_Injection

### [HIGH] Missing Rate Limiting on /api/auth/login
**Description**: Login endpoint has no rate limiting
**Fix**: Add express-rate-limit middleware
```

## Implementation

```javascript
// security-scanner.js
const fs = require('fs');
const path = require('path');
const { execSync } = require('child_process');

class SecurityScanner {
  constructor(projectPath) {
    this.projectPath = projectPath;
    this.findings = [];
  }

  async scan() {
    await this.scanForSQLInjection();
    await this.scanForXSS();
    await this.scanForHardcodedSecrets();
    await this.checkDependencies();
    await this.reviewAuthentication();
    return this.generateReport();
  }

  async scanForSQLInjection() {
    const files = this.getJavaScriptFiles();
    for (const file of files) {
      const content = fs.readFileSync(file, 'utf-8');
      const lines = content.split('\n');

      lines.forEach((line, index) => {
        // Check for string concatenation in queries
        if (line.match(/query\s*\(\s*['"`].*\+/)) {
          this.findings.push({
            severity: 'CRITICAL',
            type: 'SQL Injection',
            file: file,
            line: index + 1,
            code: line.trim(),
            fix: 'Use parameterized queries'
          });
        }
      });
    }
  }

  async scanForXSS() {
    const files = this.getJavaScriptFiles();
    for (const file of files) {
      const content = fs.readFileSync(file, 'utf-8');

      if (content.includes('dangerouslySetInnerHTML')) {
        this.findings.push({
          severity: 'HIGH',
          type: 'XSS Risk',
          file: file,
          fix: 'Sanitize HTML content before rendering'
        });
      }
    }
  }

  async checkDependencies() {
    try {
      const audit = execSync('npm audit --json', { cwd: this.projectPath });
      const results = JSON.parse(audit);

      if (results.vulnerabilities) {
        Object.entries(results.vulnerabilities).forEach(([pkg, vuln]) => {
          this.findings.push({
            severity: vuln.severity.toUpperCase(),
            type: 'Vulnerable Dependency',
            file: `package.json (${pkg})`,
            fix: `Update to version ${vuln.fixAvailable || 'latest'}`
          });
        });
      }
    } catch (error) {
      // npm audit returns non-zero exit code when vulnerabilities found
    }
  }

  generateReport() {
    const critical = this.findings.filter(f => f.severity === 'CRITICAL').length;
    const high = this.findings.filter(f => f.severity === 'HIGH').length;
    const medium = this.findings.filter(f => f.severity === 'MEDIUM').length;
    const low = this.findings.filter(f => f.severity === 'LOW').length;

    return {
      summary: { critical, high, medium, low, total: this.findings.length },
      findings: this.findings
    };
  }
}

module.exports = SecurityScanner;
```

## AI Prompt for Security Agent

```
Create a security scanner agent that:
1. Scans Express.js code for SQL injection risks
2. Checks React components for XSS vulnerabilities
3. Reviews authentication implementation
4. Checks for hardcoded secrets
5. Audits npm dependencies
6. Generates a security report with severity levels

Include the agent definition and implementation code.
```

## Practice Exercise

Build and run the security scanner on your project:
1. Create the agent definition file
2. Implement the scanner logic
3. Run it on your Task Management API
4. Review the findings
5. Fix the critical and high severity issues

## Key Takeaways

- Security scanners automate vulnerability detection
- Check for SQL injection, XSS, authentication issues, and vulnerable dependencies
- Generate reports with severity levels and suggested fixes
- Run regularly to catch new vulnerabilities
