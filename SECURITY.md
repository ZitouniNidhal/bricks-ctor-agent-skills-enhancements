# Security Policy

## Reporting Security Vulnerabilities

If you discover a security vulnerability in BRICKS CTOR Agent Skills, please **do not** create a public GitHub issue. Instead, please email the maintainers directly with:

- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if available)

We will acknowledge receipt within 48 hours and provide an expected timeline for a fix.

## Security Best Practices

### For Contributors

- **Never commit secrets**: API keys, tokens, passwords, or credentials should never be included in the repository
- **Use environment variables**: Store sensitive configuration in `.env` files (which should be in `.gitignore`)
- **Review dependencies**: Before adding new npm/Python packages, review their security status
  ```bash
  # Check for vulnerabilities
  npm audit
  bun audit
  ```
- **Sanitize inputs**: Always validate and sanitize user inputs
- **Secure file operations**: Use secure methods for file I/O with proper permissions
- **No hardcoded credentials**: Use config files or environment variables

### For Plugin Authors

- **Input validation**: Validate all parameters before processing
  ```typescript
  // ✓ Good
  if (!filePath.startsWith('/safe/directory')) {
    throw new Error('Invalid path');
  }
  
  // ✗ Bad - allows directory traversal
  readFile(userProvidedPath);
  ```

- **Resource limits**: Set limits on memory and processing time
  ```typescript
  // ✓ Prevent DoS
  const MAX_FILE_SIZE = 100 * 1024 * 1024; // 100MB
  if (fileSize > MAX_FILE_SIZE) {
    throw new Error('File too large');
  }
  ```

- **Error handling**: Don't expose internal details in error messages
  ```typescript
  // ✓ Good
  throw new Error('Failed to process file');
  
  // ✗ Bad - reveals implementation details
  throw new Error(`Database connection failed: ${error.message}`);
  ```

### For Users

- **Keep CTOR updated**: Regular updates include security patches
- **Review plugins**: Only install plugins from trusted sources
- **Verify permissions**: Be aware of what data plugins can access:
  - File system access
  - Device metrics (bricks CLI)
  - Model execution environment
- **Secure credentials**: Store API keys and authentication tokens securely
  - Use CTOR's credential management when available
  - Never share tokens or API keys
  - Rotate credentials regularly

## Dependency Management

### Approved Package Managers

- **npm** for Node.js packages
- **pip** for Python packages
- **Bun** native runtime support

### Adding Dependencies

```bash
# Review package first
npm view package-name

# Check for known vulnerabilities
npm audit package-name

# Install with lock file
bun add package-name
# OR
npm install package-name
```

### Keeping Dependencies Updated

```bash
# Check for updates
bun outdated

# Update patches (safe)
bun update --patch

# Review major version updates before upgrading
bun update --major
```

## Code Review Security Checklist

When reviewing PRs, verify:

- [ ] No secrets, API keys, or credentials in code
- [ ] Input validation on all user inputs
- [ ] Resource limits (file size, memory, execution time)
- [ ] Proper error handling without exposing internals
- [ ] Dependencies are legitimate and maintained
- [ ] No hardcoded URLs (use configuration)
- [ ] File permissions are restrictive
- [ ] No use of insecure algorithms or deprecated functions

## Vulnerability Response Timeline

- **Immediate** (T+0h): Acknowledge receipt
- **Investigation** (T+24h): Assess impact and severity
- **Development** (T+3-7 days): Develop and test fix
- **Release** (T+7 days): Release patch/update
- **Disclosure** (T+14 days): Public disclosure (coordinated)

### Severity Levels

1. **Critical**: Affects multiple users, easy to exploit, high impact
2. **High**: Significant impact, requires specific conditions
3. **Medium**: Limited impact, difficult to exploit
4. **Low**: Minimal impact, not easily exploitable

## Incident Response

If a security issue is discovered in published code:

1. **Create private security advisory** (GitHub feature)
2. **Develop fix** in private branch
3. **Notify maintainers** through security channel
4. **Test thoroughly** before release
5. **Release patch** with CVE if applicable
6. **Publish advisory** after patch release
7. **Document in CHANGELOG** with security notice

## Compliance

This project:

- ✅ Uses MIT License (permissive, well-vetted)
- ✅ Maintains dependency inventories
- ✅ Supports security issue reporting
- ✅ Follows secure coding practices
- ✅ Reviews dependencies for vulnerabilities
- ✅ Documents security policies

## Additional Resources

- [OWASP Secure Coding Practices](https://owasp.org/www-project-secure-coding-practices-quick-reference-guide/)
- [Node.js Security Best Practices](https://nodejs.org/en/docs/guides/security/)
- [Python Security](https://python.readthedocs.io/en/latest/library/security_warnings.html)
- [npm Security Advisories](https://www.npmjs.com/advisories)

## Security Contact

For security vulnerabilities, please email maintainers through GitHub's private security vulnerability reporting feature.

---

Thank you for helping keep BRICKS CTOR Agent Skills secure! 🔒
