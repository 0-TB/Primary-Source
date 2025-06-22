# Primary Source: Technical Build Rules & Safety Systems

This document establishes the technical standards, safety mechanisms, and early warning systems for Primary Source development. It includes built-in safeguards to catch issues early and clear escalation procedures when problems arise.

---

## SECTION 1: SAFETY SYSTEMS & EARLY WARNING MECHANISMS

### ? CRITICAL FAILURE TRIGGERS

If ANY of these occur, I must IMMEDIATELY stop all work and escalate:

- Security vulnerability discovered (data exposure, authentication bypass, etc.)
- Privacy protection failure (EXIF data not stripped, user data leaked, etc.)
- Database corruption or data loss of any kind
- Performance degradation >3 seconds load time
- Deployment failure that breaks production
- Legal compliance issue discovered (DMCA, content policy, etc.)

### ?? WARNING LEVEL TRIGGERS

If ANY of these occur, I must pause and seek guidance:

- Uncertainty about architectural decision (applies 95% confidence rule)
- Test failures that can't be quickly resolved
- Accessibility score drops below 90
- Build process takes longer than expected (>50% over estimate)
- Dependencies introduce known security issues
- Technical debt accumulating (code quality degrading)

### ? AUTOMATED MONITORING SYSTEMS

Built into our development process:

- Lighthouse CI: Automatically checks performance, accessibility, SEO on every build
- TypeScript Strict Mode: Catches type errors and potential bugs at compile time
- Security Headers: Automated verification of CORS, CSP, HTTPS enforcement
- File Size Monitoring: Alerts when bundle size grows significantly
- Error Tracking: Real-time monitoring for uncaught exceptions
- Uptime Monitoring: Automated alerts for service disruptions

### ? ESCALATION PROTOCOL

When issues arise:

- **CRITICAL**: Immediately inform co-founder with detailed problem description and impact assessment
- **WARNING**: Pause work, document issue, present options with risk/benefit analysis
- **UNCERTAINTY**: Apply 95% confidence rule - present problem + proposed solutions
- **ROLLBACK**: Always maintain ability to revert to last working state

---

## SECTION 2: CODING STANDARDS & CONVENTIONS

### ? FILE ORGANIZATION

- Follow Next.js 14 App Router structure
- Components: `/src/components/[feature]/ComponentName.tsx`
- Utilities: `/src/lib/[purpose].ts`
- Types: `/src/types/[feature].ts`
- Constants: `/src/constants/[purpose].ts`

### ?? TYPESCRIPT REQUIREMENTS

- Strict mode enabled: No 'any' types allowed
- Explicit return types for all functions
- Interface definitions for all props and API responses
- Zod schemas for runtime validation of user inputs

### ? REACT COMPONENT STANDARDS

- Functional components only (no class components)
- Custom hooks for reusable logic (prefix with 'use')
- Props destructured at component entry
- Conditional rendering using ternary or early returns
- Event handlers prefixed with 'handle' (handleClick, handleSubmit)

### ? SECURITY REQUIREMENTS

- Input validation on both client AND server
- Sanitize all user inputs before processing
- Rate limiting on all API endpoints
- HTTPS only in production (HTTP security headers)
- No sensitive data in client-side code or logs
- Environment variables for all secrets and API keys

### ? PERFORMANCE STANDARDS

- Page load time <3 seconds on 3G connection
- Lighthouse Performance score >90
- Image optimization with Next.js Image component
- Code splitting for route-based components
- Lazy loading for non-critical components

---

## SECTION 3: TESTING FRAMEWORK & REQUIREMENTS

### ? TESTING PYRAMID

- Unit Tests (70%): Individual function/component testing
- Integration Tests (20%): Component interaction testing
- End-to-End Tests (10%): Full user journey testing

### ? MANDATORY TEST COVERAGE

- ALL file upload functionality (privacy-critical)
- ALL EXIF stripping operations
- ALL API endpoints with security validation
- ALL user input validation
- Error boundary components

### ? TESTING TOOLS

- Jest + React Testing Library for unit/integration
- Playwright for end-to-end testing
- MSW (Mock Service Worker) for API mocking

---

## SECTION 4: DEPLOYMENT & CI/CD PIPELINE

### ? DEPLOYMENT STRATEGY

- Staging Environment: Every PR automatically deploys preview
- Production Environment: Only after manual approval
- Database migrations: Backward compatible, tested separately
- Zero-downtime deployments required

### ?? CI/CD PIPELINE CHECKS

Every commit must pass ALL of these before deployment:

- TypeScript compilation (zero errors)
- All tests passing (unit + integration + e2e)
- Lighthouse scores: Performance >90, Accessibility >90
- Security scanning (no critical vulnerabilities)
- Bundle size analysis (no unexpected growth)
- EXIF stripping verification (automated test)

### ? AUTOMATIC ROLLBACK TRIGGERS

- Error rate >1% for 5 minutes
- Response time >5 seconds for 2 minutes
- Memory usage >80% for 3 minutes
- Any uncaught security exception

---

## SECTION 5: PRIMARY SOURCE SPECIFIC SAFEGUARDS

### ? PRIVACY PROTECTION PROTOCOLS

- **MANDATORY**: Every uploaded file MUST have EXIF data stripped within 30 seconds
- **MANDATORY**: No user tracking cookies or analytics
- **MANDATORY**: Anonymous uploads only (no user accounts initially)
- **MANDATORY**: All user-facing error messages must not leak system information

### ? VERIFICATION & METADATA HANDLING

- **MANDATORY**: Extract and preserve timestamp data before EXIF stripping
- **MANDATORY**: GPS coordinates must be processed and stored separately from original files
- **MANDATORY**: File integrity verification (checksums/hashes for authenticity)
- **MANDATORY**: Device metadata extraction (camera model, settings) stored separately

### ? DATABASE & STORAGE REQUIREMENTS

- **MANDATORY**: Automated backups every 6 hours with geographic distribution
- **MANDATORY**: Immutable storage for uploaded content (no deletion/modification)
- **MANDATORY**: Database encryption at rest and in transit
- **MANDATORY**: Content addressing for decentralized distribution

---

## SECTION 6: ERROR HANDLING & RESILIENCE

### ?? ERROR RECOVERY PROTOCOLS

- Graceful degradation: Core upload functionality must work even if auxiliary services fail
- Retry mechanisms: Failed uploads automatically retry with exponential backoff
- Queue persistence: Upload queue survives server restarts and failures
- Circuit breakers: Prevent cascade failures by isolating failed services

### ? INCIDENT RESPONSE FRAMEWORK

- Severity Classification: P0 (service down), P1 (degraded), P2 (minor issues)
- Response Times: P0 = immediate, P1 = 30 minutes, P2 = 24 hours
- Communication Plan: Status page updates, co-founder notification protocols
- Post-incident reviews: Document root cause, prevention measures

---

## SECTION 7: ACCESSIBILITY & USABILITY STANDARDS

### ? ACCESSIBILITY COMPLIANCE

- WCAG 2.1 AA compliance minimum (target AAA where possible)
- Screen reader compatibility for all core functions
- Keyboard navigation for entire application
- Color contrast ratio >4.5:1 for normal text, >3:1 for large text

### ? INTERNATIONALIZATION

- UTF-8 support for all user inputs and content
- RTL (right-to-left) language support architecture
- Timezone handling for global uploads
- Cultural sensitivity in UI design and content handling

---

## SECTION 8: DOCUMENTATION & KNOWLEDGE TRANSFER

### ? MANDATORY DOCUMENTATION

- API documentation with real examples for every endpoint
- Component library with usage examples and props documentation
- Database schema documentation with relationship diagrams
- Deployment guides and environment setup instructions

### ? DECISION DOCUMENTATION

- Architecture Decision Records (ADRs) for all major technical choices
- Privacy protection decision log (why specific measures were chosen)
- Performance optimization decisions and trade-offs
- Security implementation choices and threat model considerations

---

## SECTION 9: DAILY WORKFLOW & DEVELOPMENT PROTOCOLS

### ? DEVELOPMENT VELOCITY RULES

- Feature branches: All development in branches, no direct commits to main
- Commit standards: Conventional commits with clear scope and breaking changes
- Code reviews: Required for all changes, focus on security and privacy
- Deploy frequency: Small, frequent deployments over large releases

### ? CONTINUOUS IMPROVEMENT

- Weekly performance reviews: Check Lighthouse scores, bundle size, load times
- Security audits: Monthly vulnerability scans and dependency updates
- Privacy compliance checks: Verify EXIF stripping, no tracking, data handling
- Technical debt assessment: Identify and prioritize code quality improvements

---

## ?? FINAL REMINDER: CRITICAL SUCCESS FACTORS

Primary Source's mission depends on absolute trust. Every technical decision must prioritize:

1. **User Privacy Protection** (EXIF stripping, no tracking, anonymous uploads)
2. **Content Integrity** (checksums, immutable storage, verification)
3. **Global Accessibility** (fast uploads, international support, resilience)
4. **Long-term Preservation** (decentralized storage, backup strategies)

These build rules are not suggestions?they are mandatory safeguards that ensure Primary Source can serve as a trusted historical archive. When in doubt, always choose the more secure, more private, more accessible option.
