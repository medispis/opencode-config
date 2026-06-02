---
name: dep-audit
description: Checks for outdated, vulnerable, or problematic dependencies and suggests upgrades. Use when auditing dependencies, planning upgrades, or checking for security vulnerabilities.
---

# Dependency Audit

Checks for known vulnerabilities, outdated packages, dependency health, and suggests safe upgrade paths.

## How it works

1. Detect the package manager and lock file
2. Run vulnerability audit (npm audit, pip-audit, cargo audit, etc.)
3. Check for outdated packages
4. For each outdated/vulnerable package: current version vs latest, severity, breaking changes, maintenance status
5. Generate a prioritized report

## Audit commands by ecosystem

| Ecosystem | Vulnerability check | Outdated check |
|-----------|-------------------|----------------|
| npm | `npm audit` | `npm outdated` |
| pnpm | `pnpm audit` | `pnpm outdated` |
| yarn | `yarn audit` | `yarn outdated` |
| Python | `pip-audit` or `safety check` | `pip list --outdated` |
| Rust | `cargo audit` | `cargo outdated` |
| Go | `govulncheck ./...` | `go list -m -u all` |
| Ruby | `bundle audit` | `bundle outdated` |

## Risk classification

### Critical (fix immediately)
Known exploited vulnerabilities, remote code execution, authentication bypass.

### High (fix soon)
SQL injection, XSS, CSRF, data exposure, denial of service.

### Medium (plan to fix)
Outdated with known issues, deprecated packages, missing security patches.

### Low (nice to fix)
Minor version behind, alternative packages available, maintenance concerns.

## Upgrade strategy

- **Patch upgrade**: Always safe, do it
- **Minor upgrade**: Usually safe, check changelog
- **Major upgrade**: Check breaking changes, may need code changes

## Rules

- Run actual audit commands, don't just read package.json
- Distinguish between vulnerabilities and just being outdated
- Check for breaking changes before recommending major upgrades
- Don't suggest removing dependencies without checking for dynamic imports
- If no lock file exists, note that audits are less reliable

## Output format

```markdown
## Dependency Audit

**Ecosystem:** Node.js (npm)
**Total dependencies:** 45

### Vulnerabilities
| Package | Severity | Issue | Fix |
|---------|----------|-------|-----|
| lodash | high | Prototype pollution | upgrade to 4.17.21 |

### Outdated packages
| Package | Current | Latest | Type | Breaking? |
|---------|---------|--------|------|-----------|
| express | 4.18.0 | 4.18.2 | patch | no |
| react | 17.0.2 | 18.2.0 | major | yes |

### Recommendations
1. **Immediate:** Fix 2 vulnerabilities (lodash, axios)
2. **Safe upgrades:** 8 patch updates, no breaking changes
3. **Plan ahead:** 3 major upgrades need testing
```
