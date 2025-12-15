# Qwen Code Development Guide

This document provides comprehensive context for developing and contributing to the Qwen Code project. Qwen Code is an AI-powered command-line workflow tool optimized for Qwen3-Coder models, adapted from Google's Gemini CLI.

## Project Overview

**Project Type:** Monorepo TypeScript/JavaScript project with multiple packages
**Primary Purpose:** AI-powered CLI tool for developers
**Architecture:** Modular design with separate packages for CLI, core functionality, SDK, and VS Code extension

## Technology Stack

- **Language:** TypeScript/JavaScript
- **Runtime:** Node.js 20+
- **CLI Framework:** React with Ink (terminal UI)
- **Build System:** esbuild + custom build scripts
- **Testing:** Vitest with React Testing Library
- **Code Quality:** ESLint + Prettier
- **Package Manager:** npm (with workspace support)

## Package Structure

```
qwen-code/
├── packages/
│   ├── cli/                    # Main CLI interface
│   │   ├── src/
│   │   │   ├── commands/       # CLI command implementations
│   │   │   ├── config/         # Configuration management
│   │   │   ├── core/           # Core CLI logic
│   │   │   ├── services/       # External service integrations
│   │   │   ├── ui/             # React components for terminal UI
│   │   │   └── utils/          # Utility functions
│   │   └── package.json        # CLI dependencies and scripts
│   │
│   ├── core/                   # Core AI and workflow functionality
│   │   ├── src/
│   │   │   ├── ai/             # AI model integrations
│   │   │   ├── code_assist/    # Code assistance features
│   │   │   ├── config/         # Configuration handling
│   │   │   ├── tools/          # Development tools
│   │   │   └── utils/          # Core utilities
│   │   └── package.json        # Core dependencies
│   │
│   ├── sdk-typescript/         # TypeScript SDK for external use
│   ├── test-utils/             # Shared testing utilities
│   └── vscode-ide-companion/   # VS Code extension
│
├── docs/                       # Documentation
├── scripts/                    # Build and maintenance scripts
├── integration-tests/          # End-to-end tests
└── eslint-rules/              # Custom ESLint rules
```

## Development Setup

### Prerequisites

- **Node.js:** Version 20 or higher
- **npm:** Latest version

### Initial Setup

```bash
# Clone the repository
git clone https://github.com/QwenLM/qwen-code.git
cd qwen-code

# Install dependencies
npm install

# Build all packages
npm run build

# Run tests
npm test
```

### Development Commands

#### Root Level Commands

```bash
npm run build          # Build all packages
npm run build:clean    # Clean build all packages
npm test              # Run all tests
npm run lint          # Lint all code
npm run format        # Format all code
npm run typecheck     # Type check all packages
```

#### Package-Specific Commands

```bash
# CLI package
cd packages/cli
npm run build         # Build CLI package
npm run start         # Start CLI in development
npm run debug         # Debug CLI with inspector
npm run test          # Test CLI package
npm run lint          # Lint CLI code
npm run format        # Format CLI code
npm run typecheck     # Type check CLI

# Core package
cd packages/core
npm run build         # Build core package
npm run test          # Test core package
npm run lint          # Lint core code
```

## Architecture Patterns

### CLI Architecture

The CLI uses a React-based UI with Ink framework:

- **Main Entry:** `packages/cli/index.ts`
- **React Components:** Terminal-based UI components
- **Command System:** Modular command structure in `commands/`
- **Configuration:** Centralized config management
- **State Management:** React hooks and context

### Core Architecture

The core package handles:

- **AI Integration:** Qwen model API calls and response processing
- **Code Analysis:** File parsing and code understanding
- **Tool System:** Extensible tool framework for actions
- **Workflow Automation:** Task execution and automation

### Build System

- **esbuild:** Fast TypeScript compilation and bundling
- **Custom Scripts:** `scripts/build_package.js` for package-specific builds
- **Type Declarations:** Automatic generation of `.d.ts` files
- **Tree Shaking:** Unused code elimination

## Key Configuration Files

### Root Configuration

- `package.json` - Workspace configuration and root scripts
- `tsconfig.json` - TypeScript configuration for the entire project
- `vitest.config.ts` - Test configuration
- `eslint.config.js` - ESLint rules and configuration
- `.prettierrc.json` - Code formatting rules
- `Makefile` - Build and development shortcuts

### Package-Specific Configuration

- `packages/cli/tsconfig.json` - CLI-specific TypeScript settings
- `packages/core/tsconfig.json` - Core package TypeScript settings
- Individual `package.json` files for each package

## Development Conventions

### Code Style

- **Formatting:** Prettier with project-specific configuration
- **Linting:** ESLint with TypeScript and React rules
- **Type Safety:** Strict TypeScript configuration
- **Naming:** camelCase for variables/functions, PascalCase for components

### File Organization

- **Source Code:** `src/` directories in each package
- **Tests:** Co-located with source files or in `test/` directories
- **Generated Files:** `dist/` for build outputs, `generated/` for code generation

### Testing Strategy

- **Unit Tests:** Vitest with React Testing Library
- **Integration Tests:** `integration-tests/` directory
- **Test Utilities:** Shared in `packages/test-utils/`
- **Mocking:** Jest-style mocking with Vitest

### Git Workflow

- **Branching:** Feature branches from main
- **Commits:** Conventional commit format
- **Hooks:** Pre-commit hooks for linting and formatting
- **PR Process:** Automated CI checks required

## Environment Setup

### Development Environment Variables

For development and testing:

```bash
# Optional: Custom API endpoints for testing
export OPENAI_API_KEY="your_test_key"
export OPENAI_BASE_URL="your_test_endpoint"
export OPENAI_MODEL="your_test_model"
```

### Local Configuration

User-specific settings are stored in `~/.qwen/settings.json`:

```json
{
  "sessionTokenLimit": 32000,
  "experimental": {
    "vlmSwitchMode": "once"
  }
}
```

## Debugging and Development Tools

### Debugging CLI

```bash
# Start CLI with Node.js debugger
cd packages/cli
npm run debug

# Debug specific commands
node --inspect-brk dist/index.js [command] [args]
```

### Testing

```bash
# Run tests with coverage
npm test -- --coverage

# Watch mode for development
npm test -- --watch

# Run specific test files
npm test gemini.test.tsx
```

### Code Quality

```bash
# Lint with auto-fix
npm run lint -- --fix

# Check formatting
npm run format -- --check

# Type check
npm run typecheck
```

## Common Development Tasks

### Adding New Commands

1. Create command file in `packages/cli/src/commands/`
2. Implement command logic and React UI components
3. Add command registration in main CLI entry
4. Add tests for the new command
5. Update documentation

### Adding New Features to Core

1. Implement feature in appropriate `packages/core/src/` directory
2. Export functionality in `packages/core/src/index.ts`
3. Create tests in corresponding test file
4. Update package exports if needed

### Building and Publishing

```bash
# Build all packages
npm run build

# Build specific package
cd packages/[package-name]
npm run build

# Publish (requires authentication)
npm publish
```

## Troubleshooting

### Common Issues

**Build Failures:**

- Ensure Node.js 20+ is installed
- Clear `node_modules` and reinstall: `rm -rf node_modules && npm install`
- Check for TypeScript errors: `npm run typecheck`

**Test Failures:**

- Run tests in watch mode to identify failing tests: `npm test -- --watch`
- Check test setup files in each package
- Ensure all dependencies are installed

**Development Server Issues:**

- Check port availability if running multiple instances
- Clear any cached build artifacts
- Restart the development server

### Getting Help

- **Documentation:** Check `docs/` directory for detailed guides
- **Issues:** GitHub Issues for bug reports and feature requests
- **Contributing:** See `CONTRIBUTING.md` for contribution guidelines

## Performance Considerations

### Build Performance

- esbuild provides fast compilation
- Incremental builds supported via watch mode
- Parallel building of packages when possible

### Runtime Performance

- Tree shaking eliminates unused code
- Lazy loading for large dependencies
- Efficient React rendering in terminal environment

### Memory Usage

- Monitor memory usage during development
- Use Node.js flags for memory profiling if needed
- Clean up resources in React components

## Security Considerations

### API Key Management

- Never commit API keys to version control
- Use environment variables for sensitive data
- Follow the project's authentication guidelines

### Dependency Security

- Regular security audits: `npm audit`
- Keep dependencies updated
- Review new dependencies before adding

### Code Security

- Follow secure coding practices
- Validate user inputs
- Use TypeScript for type safety

## Future Development

### Planned Features

- Enhanced AI model support
- Additional IDE integrations
- Expanded workflow automation
- Performance optimizations

### Contributing

See `CONTRIBUTING.md` for detailed contribution guidelines, coding standards, and the pull request process.

---

**Note:** This guide should be updated as the project evolves. Last updated: 2025-12-15
