# Google Summer of Code 2026 - Meteor Project Ideas

This document outlines proposed projects for Google Summer of Code 2026 for the Meteor framework. Each project aims to improve key areas of the Meteor ecosystem based on analysis of the current codebase.

Projects are organized by difficulty and size, from smallest to largest.

---

## Project 1: Client-Side Type Safety

### Description

Implement end-to-end type safety for Meteor's client-server communication, ensuring that method calls, subscriptions, and collection operations are fully type-checked at compile time. Currently, method arguments are typed as `any[]`, losing type information across the wire.

### Current State

- **Method Calls**: `Meteor.call(name, ...args: any[])` - no type checking on arguments
- **Subscriptions**: `Meteor.subscribe(name, ...args: any[])` - no type correspondence with publish functions
- **Collections**: Generic `Collection<T>` exists but often instantiated as `Collection<any>`
- **DDP Types**: Basic `DDPStatic` interface with untyped message passing
- **Runtime Validation**: Optional via `audit-argument-checks` package with `check()` function

### Goals

1. **Typed Method Definitions**: Create method registry that infers argument and return types
2. **Typed Subscriptions**: Link `subscribe()` calls to `publish()` function signatures
3. **Collection Schema Validation**: Compile-time document type enforcement
4. **Client-Server Type Bridge**: Generate shared type definitions for both environments
5. **Migration Path**: Provide gradual adoption without breaking existing code

### Technical Details

**Key Files to Modify:**
- `packages/ddp/ddp.d.ts` - Enhanced DDP types
- `packages/meteor/meteor.d.ts` - Method/subscription type registry
- `packages/mongo/mongo.d.ts` - Collection type improvements
- `packages/check/check.d.ts` - Runtime validation integration

**Proposed API:**
```typescript
// Typed method definition
declare module 'meteor/meteor' {
  interface MethodRegistry {
    'users.create': {
      args: [{ name: string; email: string }];
      result: { userId: string };
    };
  }

  function callAsync<K extends keyof MethodRegistry>(
    name: K,
    ...args: MethodRegistry[K]['args']
  ): Promise<MethodRegistry[K]['result']>;
}

// Usage - fully typed!
const result = await Meteor.callAsync('users.create', { name: 'John', email: 'john@example.com' });
// result is typed as { userId: string }
```

**Technologies:**
- TypeScript declaration merging
- Generic type inference
- Build-time code generation
- EJSON type constraints

### Expected Outcomes

- Full type safety for Meteor.call() and Meteor.callAsync()
- Type correspondence between subscribe() and publish()
- Compile-time validation of method arguments
- Type-safe collection operations with schema validation
- Generated type definitions from method/publication registrations

### Skills Required

- TypeScript (conditional types, infer, template literal types)
- Knowledge of compile-time vs runtime type checking

### Difficulty Level

**Easy**

### Mentors

_To be assigned_

### Time Estimate

**90 hours** (Small project)

---

## Project 2: Release CI/CD Speed & Reliability

### Description

Modernize and optimize Meteor's release and CI/CD infrastructure to reduce build times, improve reliability, and provide a better developer experience for contributors. The current release process involves multiple manual steps, scattered across different CI systems (GitHub Actions, CircleCI, Jenkins), and relies on institutional knowledge not fully documented.

### Current State

- **Multiple CI Systems**: GitHub Actions for specific workflows (E2E tests, syntax checks, npm package publishing), CircleCI for main test suite (920-line config with 12 parallel test groups), and legacy Jenkins for dev bundle builds
- **Manual Release Process**: Scripts in `scripts/admin/` require SSH access to platform-specific machines for multi-platform publishing
- **Test Infrastructure**: 12 parallel test groups on CircleCI with regex-based filtering, but no unified release trigger
- **Platform Coverage**: Linux (x86_64, aarch64), macOS (x86_64, arm64), Windows (x86_32) - each requires separate build/publish steps
- **Dev Bundle Publishing**: Currently only Meteor Software staff can publish dev bundles, creating bottlenecks for contributors

### Goals

1. **Consolidate CI/CD to GitHub Actions**: Migrate remaining CircleCI workflows to GitHub Actions for unified management
2. **Improve Test Performance**: Implement better caching strategies, parallel execution, and selective test running based on changed files
3. **Create Release Documentation**: Write comprehensive RELEASING.md with step-by-step procedures

### Technical Details

**Key Files to Modify:**
- `.github/workflows/` - New and updated workflow files
- `.circleci/config.yml` - Migration source (920 lines)
- `scripts/admin/publish-meteor-tool-on-all-platforms.sh` - Automation target
- `scripts/admin/bump-all-version-numbers.js` - Version automation
- `scripts/make-release-tarballs.sh` - Release tarball generation

**Technologies:**
- GitHub Actions (matrix builds, reusable workflows, composite actions)
- Docker for consistent build environments
- AWS S3 for artifact storage
- Shell scripting (Bash, PowerShell)

### Expected Outcomes

- Reduction in total CI/CD pipeline time
- Automated version management and changelog generation

### Skills Required

- Strong experience with GitHub Actions and CI/CD pipelines
- Shell scripting (Bash, PowerShell)
- Docker containerization

### Difficulty Level

**Medium**

### Mentors

_To be assigned_

### Time Estimate

**175 hours** (Medium project)

---

## Project 3: TypeScript Improvements

### Description

Enhance Meteor's TypeScript support with better type definitions, improved build system integration, and modern TypeScript feature support. While Meteor has basic TypeScript support via Babel/SWC transpilation, the type definitions for core packages are incomplete and lack advanced TypeScript features.

### Current State

- **Build System**: TypeScript transpilation via `babel-compiler` package using SWC (`@meteorjs/swc-core`) or Babel fallback
- **Type Definitions**: 25+ packages have `.d.ts` files, but many core packages lack definitions (e.g., minimongo)
- **Configuration**: Root `tsconfig.json` targeting ES2020, tools using ES2018 with strict mode
- **Community Types**: Integration with `zodern:types` and `@types/meteor` for package type resolution
- **Modern Features**: Limited support for TypeScript 5.x features (const type parameters, NoInfer, etc.)

### Goals

1. **Complete Core Package Types**: Add missing type definitions for minimongo, templating, and other core packages
2. **Type-Safe Methods/Publications**: Implement registry for method and publication type inference
3. **Declaration File Generation**: Enable packages to export their own `.d.ts` files automatically
4. **Modern TypeScript Support**: Add support for TypeScript 5.x features
5. **Build System Improvements**: Better error reporting, incremental compilation support
6. **Rspack integration**: Explore Rspack as an alternative bundler with TypeScript support

### Technical Details

**Key Files to Modify:**
- `packages/*/package.js` - Add type definition exports
- `packages/babel-compiler/babel-compiler.js` - Build system integration
- `packages/minimongo/*.d.ts` - New type definitions
- `tools/static-assets/skel-typescript/` - Skeleton template improvements
- `tsconfig.json` - Modern compiler options

**New Type Definitions Needed:**
```typescript
// Example: minimongo types
declare module 'meteor/minimongo' {
  interface LocalCollection<T> {
    find(selector?: Selector<T>, options?: Options): Cursor<T>;
    findOne(selector?: Selector<T>): T | undefined;
    insert(doc: OptionalId<T>): string;
    update(selector: Selector<T>, modifier: Modifier<T>): number;
    remove(selector: Selector<T>): number;
  }
}
```

**Technologies:**
- TypeScript 5.x compiler API
- Babel/SWC integration
- Declaration file generation
- JSON Schema for type inference

### Expected Outcomes

- Complete type definitions for all core Meteor packages
- Type-safe Meteor.methods() and Meteor.publish() with argument inference
- Automatic declaration file generation for Meteor packages
- Support for TypeScript 5.x language features
- Improved developer experience with better IDE integration

### Skills Required

- TypeScript knowledge (generics, conditional types, mapped types)
- Understanding of TypeScript compiler API
- Node.js build system knowledge

### Difficulty Level

**Medium**

### Mentors

_To be assigned_

### Time Estimate

**175 hours** (Medium project)

---

## Project 4: OpenTelemetry Support

### Description

Implement comprehensive OpenTelemetry instrumentation for Meteor applications, providing automatic tracing, metrics, and logging for DDP, MongoDB, HTTP, and other core operations. The foundation exists with `meteor-otel` package dependencies, but active instrumentation is missing.

### Current State

- **OTEL Dependencies**: `meteor-otel` package includes full OpenTelemetry SDK (api, sdk-trace-node, sdk-metrics, exporters)
- **Hook System**: `callback-hook` package with `Hook` class for registration/iteration
- **DDP Hooks**: `onConnectionHook` and `onMessageHook` available in livedata_server
- **Async Context**: `EnvironmentVariableAsync` using Node.js `AsyncLocalStorage`
- **Logging**: Structured `Log` package with JSON output support
- **No Active Instrumentation**: OTEL packages installed but not utilized

### Goals

1. **Automatic DDP Tracing**: Trace all method calls, subscriptions, and DDP messages
2. **MongoDB Instrumentation**: Capture database operations with query details
3. **HTTP Request Tracing**: Instrument webapp's Express middleware
4. **Custom Metrics**: Method latency, subscription counts, connection metrics
5. **Context Propagation**: Full trace context across async boundaries

### Technical Details

**Key Files to Create/Modify:**
- `packages/meteor-otel/instrumentation.js` - New instrumentation code
- `packages/ddp-server/livedata_server.js` - Hook integration
- `packages/mongo/mongo_driver.js` - MongoDB tracing
- `packages/webapp/webapp_server.js` - HTTP middleware

**Instrumentation Architecture:**
```javascript
// Auto-instrumentation for Meteor methods
const methodSpan = tracer.startSpan('meteor.method', {
  attributes: {
    'meteor.method.name': methodName,
    'meteor.user.id': this.userId,
    'meteor.connection.id': this.connection?.id,
  }
});

// MongoDB operation tracing
const dbSpan = tracer.startSpan('mongodb.operation', {
  attributes: {
    'db.system': 'mongodb',
    'db.operation': 'find',
    'db.collection': collectionName,
  }
});
```

**Semantic Conventions:**
- `meteor.method.name` - Method being called
- `meteor.subscription.name` - Publication name
- `meteor.user.id` - Current user ID
- `ddp.message.type` - DDP message type (method, sub, unsub)
- `ddp.connection.id` - DDP connection identifier

**Technologies:**
- OpenTelemetry SDK for Node.js
- OTLP exporters (HTTP, gRPC)
- Prometheus metrics format

### Expected Outcomes

- Zero-config automatic instrumentation for Meteor apps
- Full distributed tracing across DDP, MongoDB, and HTTP
- Custom Meteor-specific metrics dashboard
- Performance insights for method calls, subscriptions, and database operations
- Comprehensive documentation and examples

### Skills Required

- OpenTelemetry SDK experience
- Understanding of distributed tracing concepts
- Node.js async context management
- Meteor's DDP protocol knowledge
- MongoDB driver internals

### Difficulty Level

**Hard**

### Mentors

_To be assigned_

### Time Estimate

**175 hours** (Medium project)

---

## Project 5: .env Support

### Description

Implement native `.env` file support in Meteor, allowing developers to manage environment variables through standard dotenv files with environment-specific overrides, validation, and secure secret handling.

### Current State

- **No Native .env Support**: Meteor doesn't automatically load `.env` files
- **Settings.json**: Primary configuration via `--settings file.json` with `Meteor.settings`
- **METEOR_SETTINGS**: Environment variable for JSON settings (not reactive in production)
- **Environment Variables**: Manual setup required (`MONGO_URL`, `ROOT_URL`, `MAIL_URL`, etc.)
- **Current .envrc**: Only for Meteor development, not application use

### Goals

1. **Automatic .env Loading**: Load `.env` files from project root on startup
2. **Environment-Specific Files**: Support `.env.development`, `.env.production`, `.env.local`
3. **Variable Validation**: Schema-based validation of required environment variables
4. **Settings Integration**: Bridge between .env and Meteor.settings
5. **CLI Tools**: Commands for env management, template generation, validation

### Technical Details

**Key Files to Modify/Create:**
- `packages/meteor/env-loader.js` - New .env loading module
- `tools/runners/run-app.js` - Integration with app startup
- `tools/cli/commands.js` - New env commands
- `packages/meteor/server_environment.js` - Settings bridge

**Loading Priority:**
```
1. .env.local (highest priority, gitignored)
2. .env.[NODE_ENV] (.env.development, .env.production)
3. .env (base defaults)
4. process.env (existing environment)
5. --settings file.json (Meteor settings)
```

**Proposed API:**
```javascript
// .env file
MONGO_URL=mongodb://localhost:27017/myapp
ROOT_URL=http://localhost:3000
MAIL_URL=smtp://user:pass@smtp.example.com

# Public variables (sent to client)
PUBLIC_APP_NAME=My Meteor App
PUBLIC_API_URL=https://api.example.com

// Access in code
process.env.MONGO_URL
Meteor.settings.public.APP_NAME  // from PUBLIC_* vars
```

**Validation Schema:**
```javascript
// meteor.config.js or package.json
{
  "meteor": {
    "envSchema": {
      "MONGO_URL": { "required": true, "format": "uri" },
      "PORT": { "type": "number", "default": 3000 },
      "NODE_ENV": { "enum": ["development", "production", "test"] }
    }
  }
}
```

**Technologies:**
- dotenv package for parsing
- JSON Schema for validation
- CLI integration
- Secure variable handling

### Expected Outcomes

- Automatic `.env` file loading in Meteor applications
- Environment-specific configuration (development/production/test)
- Schema validation with helpful error messages
- Seamless integration with `Meteor.settings`
- CLI tools: `meteor env:check`, `meteor env:template`
- Comprehensive documentation and migration guide
- Security best practices for secret management

### Skills Required

- Node.js environment handling
- dotenv and configuration management
- CLI tool development
- Security best practices

### Difficulty Level

**Hard**

### Mentors

_To be assigned_

### Time Estimate

**175 hours** (Medium project)

---

## Project 6: Core Code Quality / Linting / Standards

### Description

Establish comprehensive code quality standards and tooling for the Meteor codebase, including ESLint migration for legacy files, pre-commit hooks, automated formatting, and continuous code quality monitoring.

### Current State

- **ESLint**: Configured with `vazco` extends, but `.eslintignore` excludes 93+ legacy files
- **Prettier**: Basic config in `package.json` (semi: true, singleQuote: false)
- **No Pre-Commit Hooks**: Missing Husky/lint-staged configuration
- **Legacy Files**: Many tools, isobuild, and CLI files not yet linted
- **TypeScript Checking**: `tsconfig.json` exists but no CI validation
- **Documentation Reference**: Non-existent `scripts/admin/eslint/eslint.sh` referenced in DEVELOPMENT.md

### Goals
1. **ESLint Migration**: Gradually migrate all files to ESLint compliance
2. **Pre-Commit Hooks**: Implement Husky + lint-staged for automated checks
3. **TypeScript Validation**: Add `tsc --noEmit` to CI pipeline
4. **CSS/SCSS Linting**: Implement StyleLint for any style files
5. **Commit Standards**: Conventional commits with automated validation
6. **Code Quality Dashboard**: Track and visualize quality metrics over time

### Technical Details

**Key Files to Modify/Create:**
- `.eslintignore` - Remove migrated files
- `package.json` - Add husky, lint-staged, commitlint
- `.husky/pre-commit` - Pre-commit hook script
- `.github/workflows/code-quality.yml` - CI quality checks
- `DEVELOPMENT.md` - Update documentation

**Migration Strategy:**
```bash
# Phase 1: Auto-fixable issues
npx eslint --fix tools/isobuild/*.js

# Phase 2: Manual review required
npx eslint --max-warnings=0 tools/isobuild/*.js

# Phase 3: Full enforcement
# Remove from .eslintignore
```

**Pre-Commit Configuration:**
```json
{
  "lint-staged": {
    "*.{js,ts}": ["eslint --fix", "prettier --write"],
    "*.{json,md}": ["prettier --write"]
  },
  "commitlint": {
    "extends": ["@commitlint/config-conventional"]
  }
}
```

**Technologies:**
- ESLint 8.x with TypeScript support
- Prettier for formatting
- Husky for git hooks
- lint-staged for staged file linting
- Commitlint for commit message validation

### Expected Outcomes

- 100% ESLint coverage across all JavaScript/TypeScript files
- Automated pre-commit linting and formatting
- Conventional commit enforcement
- TypeScript type checking in CI
- Updated and accurate documentation
- Code quality metrics tracking

### Skills Required

- ESLint configuration and rule development
- Git hooks and pre-commit tooling
- JavaScript/TypeScript code standards
- CI/CD pipeline development

### Difficulty Level

**Easy**

### Mentors

_To be assigned_

### Time Estimate

**350 hours** (Large project)

---

## Project 7: Test Support Improvements

### Description

Modernize Meteor's testing infrastructure by improving the TinyTest framework, adding modern testing features, and providing better integration with popular test runners. The current testing system, while functional, lacks features expected in modern JavaScript testing.

### Current State

- **TinyTest**: Primary framework (v1.3.2) - basic assertions, no snapshot testing, limited async patterns
- **Test Drivers**: `test-in-browser` (interactive), `test-in-console` (CI), dated Bootstrap 4.3.1 UI
- **Modern Tests**: Separate Jest setup in `tools/modern-tests/` with Playwright
- **Self-Test Framework**: CLI testing with Sandbox class and Run utilities
- **Coverage**: No built-in code coverage, 150+ test files across packages

### Goals

1. **Modernize TinyTest**: Add snapshot testing, better async support, improved assertions
2. **Unified Test Runner**: Create single test command supporting multiple frameworks
3. **Code Coverage Integration**: Built-in coverage reporting with LCOV output
4. **Better Reporting**: Modern test UI, JUnit XML output, CI integration
5. **Test Discovery**: Automatic test file detection without manual registration

### Technical Details

**Key Files to Modify:**
- `packages/tinytest/tinytest.js` - Core framework enhancements
- `packages/tinytest/model.js` - Test case models
- `packages/test-in-browser/driver.js` - Modern UI
- `packages/test-in-console/driver.js` - CI improvements
- `tools/cli/commands.js` - New test commands

**New Assertion Methods:**
```javascript
// Proposed additions to TinyTest
Tinytest.add('example', async (test) => {
  // Snapshot testing
  test.matchSnapshot(complexObject, 'my-snapshot');

  // Better async
  await test.rejects(asyncFn(), /expected error/);

  // Type assertions
  test.typeOf(value, 'object');

  // Collection assertions
  test.containsAll(array, [1, 2, 3]);
  test.hasProperties(obj, ['name', 'email']);
});
```

**Technologies:**
- Jest-compatible assertion library
- Istanbul/NYC for coverage
- React or Svelte for modern test UI
- Playwright for E2E testing

### Expected Outcomes

- Modern TinyTest with snapshot testing and improved assertions
- Unified `meteor test` command with multiple framework support
- Built-in code coverage with HTML and LCOV reports
- Modern, responsive test runner UI
- Automatic test discovery and parallel execution
- Better CI/CD integration with standardized output formats

### Skills Required

- JavaScript testing framework experience (Jest, Mocha, Vitest)
- Code coverage tools (Istanbul, NYC, c8)
- Frontend development (React/Svelte for test UI)
- CI/CD integration knowledge

### Difficulty Level

**Hard**

### Mentors

_To be assigned_

### Time Estimate

**350 hours** (Large project)

---

## Summary

| # | Project | Difficulty | Hours | Size |
|---|---------|------------|-------|------|
| 1 | Client-Side Type Safety | Easy | 90 | Small |
| 2 | Release CI/CD Speed & Reliability | Medium | 175 | Medium |
| 3 | TypeScript Improvements | Medium | 175 | Medium |
| 4 | OpenTelemetry Support | Hard | 175 | Medium |
| 5 | .env Support | Hard | 175 | Medium |
| 6 | Core Code Quality / Linting | Easy | 350 | Large |
| 7 | Test Support Improvements | Hard | 350 | Large |

---

## How to Apply

1. **Choose a project** that matches your skills and interests
2. **Study the codebase** - Clone the Meteor repository and explore the relevant packages
3. **Engage with the community** - Join the Meteor forums and Discord
4. **Prepare your proposal** - Include timeline, milestones, and technical approach
5. **Submit early** - Get feedback on your proposal before the deadline

## Resources

- [Meteor GitHub Repository](https://github.com/meteor/meteor)
- [Meteor Documentation](https://docs.meteor.com)
- [Meteor Forums](https://forums.meteor.com)
- [Contributing Guide](https://github.com/meteor/meteor/blob/devel/CONTRIBUTING.md)
- [Development Guide](https://github.com/meteor/meteor/blob/devel/DEVELOPMENT.md)

---

_This document was generated based on analysis of the Meteor codebase as of January 2025._
