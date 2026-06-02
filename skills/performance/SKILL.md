---
name: performance
description: Profiles, analyzes, and optimizes application performance — backend, frontend, database, and build times. Use when investigating slowness, optimizing queries, reducing bundle size, or improving response times.
---

# Performance

Profiles, analyzes, and optimizes application performance across all layers.

## How it works

1. Identify the performance bottleneck layer (frontend, backend, database, build)
2. Profile the application to find actual hotspots (not guesses)
3. Analyze the root cause of each bottleneck
4. Apply targeted optimizations
5. Measure before and after to verify improvement

## Profiling by layer

### Backend
- Response time per endpoint
- Memory usage and leaks
- CPU profiling for hot paths
- Database query analysis (slow query log, EXPLAIN)
- Connection pool utilization

### Frontend
- Bundle size analysis (webpack-bundle-analyzer, etc.)
- Time to First Byte (TTFB)
- First Contentful Paint (FCP)
- Largest Contentful Paint (LCP)
- Cumulative Layout Shift (CLS)
- JavaScript execution time

### Database
- Slow query log analysis
- EXPLAIN plans for complex queries
- Index usage and missing indexes
- Connection pool saturation
- Lock contention

### Build
- Build time per step
- Dependency installation time
- Cache hit rates
- Parallelization opportunities

## Optimization strategies

### Backend
- Add caching (Redis, in-memory, CDN)
- Optimize database queries (indexes, batching, pagination)
- Use connection pooling
- Implement request deduplication
- Compress responses (gzip, brotli)
- Use streaming for large payloads

### Frontend
- Code splitting and lazy loading
- Optimize images (WebP, responsive sizes)
- Minimize re-renders (memo, useMemo, useCallback)
- Debounce/throttle expensive operations
- Use virtual lists for large datasets
- Preload critical resources

### Database
- Add missing indexes
- Optimize query plans
- Use materialized views for complex aggregations
- Partition large tables
- Denormalize read-heavy data
- Use read replicas for scaling

## Rules

- Profile before optimizing — don't guess at bottlenecks
- Measure before and after every optimization
- Optimize the biggest impact first (80/20 rule)
- Don't sacrifice correctness for speed
- Don't add complexity for marginal gains (<5% improvement)
- Document what was optimized and the measured improvement

## Output format

```markdown
## Performance Analysis

### Baseline
| Metric | Before | After | Improvement |
|--------|--------|-------|-------------|
| Response time | 450ms | 120ms | 73% |
| Bundle size | 1.2MB | 380KB | 68% |
| Query time | 2.3s | 45ms | 98% |

### Bottlenecks found
1. **[Bottleneck]**: [description, location]
2. **[Bottleneck]**: [description, location]

### Optimizations applied
1. **[Optimization]**: [what was done, impact]
2. **[Optimization]**: [what was done, impact]

### Files modified
- `path/to/file` — [what changed]
```
