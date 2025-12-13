# Qwen Code Project Context

This document provides comprehensive context for the Qwen Code project to guide future development and interaction.

## Project Overview

**Qwen Code** is an AI-powered command-line workflow tool for developers, adapted from Google's Gemini CLI and specifically optimized for Qwen3-Coder models. It enhances development workflows with advanced code understanding, automated tasks, and intelligent assistance.

- **Version**: 0.4.1
- **License**: Apache-2.0
- **Repository**: https://github.com/QwenLM/qwen-code
- **Primary Language**: TypeScript/JavaScript (Node.js)
- **Node.js Requirement**: >=20.0.0

## Architecture & Structure

### Monorepo Layout

This project uses a **monorepo structure** with multiple packages:

```
packages/
├── cli/                    # Main CLI application (@qwen-code/qwen-code)
├── core/                   # Core functionality (qwen-code-core)
├── sdk-typescript/         # TypeScript SDK
├── test-utils/             # Shared testing utilities
└── vscode-ide-companion/   # VS Code extension
```

### Key Technologies

- **Runtime**: Node.js 20+
- **Type System**: TypeScript with strict configuration
- **CLI UI Framework**: Ink (React-based terminal UI)
- **Build System**: ESBuild + custom build scripts
- **Testing**: Vitest + Testing Library
- **Code Quality**: ESLint + Prettier + Husky (pre-commit hooks)

## Development Workflow

### Building & Running

#### Primary Commands

```bash
# Install dependencies
npm install

# Build all packages
npm run build

# Start CLI application
npm run start

# Debug mode (with inspector)
npm run debug

# Run all tests
npm run test

# Run tests in CI mode
npm run test:ci

# Run integration tests
npm run test:integration:all

# Lint and fix code
npm run lint:fix

# Format code
npm run format

# Type checking
npm run typecheck
```

#### Pre-commit Checks

```bash
# Complete pre-flight checks (lint, build, test, typecheck)
npm run preflight
```

#### Package-Specific Commands

```bash
# Build specific package
npm run build --workspace=packages/cli

# Test specific package
npm run test --workspace=packages/cli
```

### Development Conventions

#### Code Style

- **Linting**: ESLint with TypeScript, React, and import rules
- **Formatting**: Prettier with experimental CLI support
- **Pre-commit Hooks**: Husky + lint-staged for automated code quality checks
- **License Headers**: Required for all source files (Apache-2.0)

#### File Patterns

```javascript
// Lint-staged configuration
{
  "*.{js,jsx,ts,tsx}": [
    "prettier --write",
    "eslint --fix --max-warnings 0"
  ],
  "*.{json,md}": [
    "prettier --write"
  ]
}
```

#### TypeScript Configuration

- **Strict Mode**: Enabled across all packages
- **Module System**: ES modules (type: "module")
- **Target**: Modern ES features
- **Build Output**: Separate `dist/` directories for each package

#### Package Structure

Each package follows consistent patterns:

```
packages/{package-name}/
├── src/                   # Source code
├── dist/                  # Built output (git-ignored)
├── package.json           # Package-specific dependencies
├── tsconfig.json          # TypeScript configuration
├── vitest.config.ts       # Test configuration
└── test-setup.ts          # Test environment setup
```

## Key Dependencies

### Core CLI Dependencies

- **@google/genai**: Google AI SDK integration
- **@qwen-code/qwen-code-core**: Internal core package
- **ink**: React-based terminal UI framework
- **@modelcontextprotocol/sdk**: MCP protocol support
- **react**: UI framework for terminal interface
- **yargs**: Command-line argument parsing
- **simple-git**: Git operations
- **zod**: Runtime type validation

### Development Dependencies

- **esbuild**: Fast bundler and minifier
- **vitest**: Fast unit testing framework
- **@testing-library/react**: React component testing
- **typescript**: Static type checking
- **eslint**: Code linting and style enforcement
- **prettier**: Code formatting
- **husky**: Git hooks management

## Testing Strategy

### Test Types

1. **Unit Tests**: Vitest + Testing Library for React components
2. **Integration Tests**: End-to-end testing in `integration-tests/` directory
3. **Terminal Benchmarks**: Performance testing for CLI interactions
4. **Sandbox Testing**: Docker/Podman containerized testing

### Test Commands

```bash
# Standard unit tests
npm run test

# CI-ready tests (parallel, optimized)
npm run test:ci

# Integration tests (multiple sandbox modes)
npm run test:integration:all
npm run test:integration:sandbox:docker
npm run test:integration:sandbox:podman

# Terminal benchmark tests
npm run test:terminal-bench

# Specific test suites
npm run test:integration:cli:sandbox:none
npm run test:integration:sdk:sandbox:docker
```

### Test Configuration

- **Vitest**: Primary test runner across all packages
- **Coverage**: v8 coverage provider
- **Parallel Execution**: Enabled for CI performance
- **Mock File System**: memfs for isolated testing
- **JSdom**: Browser environment simulation for React testing

## Build System

### Build Process

1. **Package Build**: Custom `build_package.js` script for each package
2. **Bundle Creation**: ESBuild bundling with `esbuild.config.js`
3. **Asset Copying**: Custom script for copying bundle assets
4. **Version Generation**: Git commit info generation
5. **Type Checking**: TypeScript compilation validation

### Build Commands

```bash
# Build all packages
npm run build

# Build with bundle creation
npm run bundle

# Build specific components
npm run build:vscode
npm run build:sandbox
npm run build:all

# Generate commit info
npm run generate
```

### Output Structure

```
dist/
├── cli.js                 # Main CLI entry point
├── index.js               # Package entry point
└── index.d.ts             # TypeScript definitions
```

## Package Details

### CLI Package (`packages/cli`)

**Main Features:**

- Command-line interface using Ink (React for terminal)
- Interactive terminal UI with gradients, links, and spinners
- QR code generation for authentication
- File system operations with diff visualization
- Git integration for repository analysis
- Support for multiple AI providers (OpenAI-compatible APIs)

**Key Components:**

- Terminal-based React UI components
- Command parsing and execution
- Authentication flow management
- File system abstraction layer
- Real-time progress indicators

### Core Package (`packages/core`)

**Purpose:** Shared functionality used across all packages
**Contents:**

- Core business logic
- Shared utilities and helpers
- Common type definitions
- Platform-agnostic functionality

### SDK TypeScript Package (`packages/sdk-typescript`)

**Purpose:** Programmatic API for integrating Qwen Code functionality
**Use Cases:**

- Embedding Qwen Code in other applications
- Custom CLI implementations
- Programmatic access to AI workflows

### VS Code Extension (`packages/vscode-ide-companion`)

**Purpose:** IDE integration bringing AI assistance directly into VS Code
**Status:** In development
**Features:** (Based on README reference)

- File system operations
- Native diff visualization
- Interactive chat interface
- Direct IDE integration

## Configuration Files

### Root Configuration

- **package.json**: Main workspace configuration and scripts
- **eslint.config.js**: ESLint configuration with TypeScript, React, and Prettier integration
- **tsconfig.json**: Root TypeScript configuration
- **esbuild.config.js**: Build bundling configuration
- **vitest.config.ts**: Root test configuration
- **prettier.config.json**: Code formatting rules

### Package Configuration

Each package has:

- **package.json**: Dependencies and package-specific scripts
- **tsconfig.json**: Package-specific TypeScript settings
- **vitest.config.ts**: Package-specific test configuration
- **test-setup.ts**: Test environment initialization

## Authentication & API Integration

### Supported Authentication Methods

1. **Qwen OAuth** (Recommended)
   - 2,000 requests/day free tier
   - Browser-based authentication
   - Automatic credential management

2. **OpenAI-Compatible APIs**
   - Environment variable configuration
   - Support for multiple providers (OpenRouter, ModelScope, etc.)
   - Configurable base URLs and models

### Configuration Methods

#### Environment Variables

```bash
export OPENAI_API_KEY="your_api_key"
export OPENAI_BASE_URL="your_endpoint"
export OPENAI_MODEL="your_model"
```

#### Project Configuration

```env
# .env file in project root
OPENAI_API_KEY=your_key
OPENAI_MODEL=your_model
```

#### User Configuration

```json
# ~/.qwen/settings.json
{
  "sessionTokenLimit": 32000,
  "experimental": {
    "vlmSwitchMode": "once"
  }
}
```

## Common Development Tasks

### Adding New Features

1. **Create feature branch**: `git checkout -b feature/new-feature`
2. **Implement changes** following existing patterns
3. **Add tests** for new functionality
4. **Run quality checks**: `npm run preflight`
5. **Submit PR** with proper issue linkage

### Debugging Issues

1. **Use debug mode**: `npm run debug`
2. **Check test output**: `npm run test:ci`
3. **Review lint errors**: `npm run lint`
4. **Validate TypeScript**: `npm run typecheck`

### Performance Optimization

1. **Run benchmarks**: `npm run test:terminal-bench`
2. **Profile build times**: `npm run build`
3. **Analyze bundle size**: Check `dist/` directory
4. **Test with large codebases**: Use integration tests

## Common Patterns & Conventions

### React Component Development (Ink)

```typescript
// Ink component pattern
import { Box, Text } from 'ink';
import React from 'react';

interface Props {
  message: string;
}

export const MyComponent: React.FC<Props> = ({ message }) => {
  return (
    <Box>
      <Text>{message}</Text>
    </Box>
  );
};
```

### Error Handling

- Use Zod for runtime type validation
- Implement proper error boundaries in React components
- Provide user-friendly error messages in CLI

### Git Workflow

- Follow conventional commits
- Link PRs to existing issues
- Ensure all checks pass before merging
- Use pre-commit hooks for quality assurance

## Troubleshooting

### Common Issues

1. **Build Failures**
   - Clear `dist/` directories: `npm run clean`
   - Rebuild: `npm run build`
   - Check TypeScript errors: `npm run typecheck`

2. **Test Failures**
   - Run tests in isolation: `npm run test -- packages/cli`
   - Check test setup: Review `test-setup.ts` files
   - Verify dependencies: `npm ci`

3. **Linting Issues**
   - Auto-fix: `npm run lint:fix`
   - Check specific rules: Review `eslint.config.js`
   - Format code: `npm run format`

### Debug Mode

```bash
# Start with debugger attached
npm run debug

# Or with verbose logging
cross-env DEBUG=1 npm run start
```

## Performance Considerations

### Token Management

- Implement session token limits for cost control
- Use compression for long conversations
- Monitor API usage through session stats

### Build Optimization

- ESBuild for fast bundling
- Parallel test execution
- Incremental builds for development

### Memory Usage

- Optimize for large codebases
- Implement proper file system abstraction
- Use streaming for large file operations

## Future Development

### Active Development Areas

1. **VS Code Extension**: IDE integration features
2. **Performance Optimization**: Token usage and response times
3. **Enhanced Workflows**: Additional automation capabilities
4. **Multi-language Support**: Internationalization (i18n)

### Contribution Guidelines

1. **CLA Required**: Google Contributor License Agreement
2. **Code Reviews**: All changes require review
3. **Small PRs**: Atomic, focused changes preferred
4. **Issue Linking**: All PRs must link to existing issues
5. **Testing**: Comprehensive test coverage required

---

This context document should be updated as the project evolves. For the most current information, always refer to the main README.md and package.json files.
