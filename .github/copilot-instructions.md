# Copilot Instructions for Execution APIs

## Project Overview

This repository contains the Ethereum JSON-RPC API specification. It is the canonical interface between users and the Ethereum network. The specification is written in OpenRPC format and split into multiple files for improved readability.

## Build and Test

### Setup
```bash
npm install
```

### Build Commands
- **Build everything**: `npm run build` (builds spec, docs, and test tools)
- **Build specification only**: `npm run build:spec`
- **Build documentation**: `npm run build:docs`
- **Build test tools**: `npm run build:test` (requires Go to be installed)

### Testing
- **Run tests**: `npm test` (runs speccheck validation)
- **Lint and validate**: `npm run lint` (validates OpenRPC spec and GraphQL schema)

### Development
- **Watch mode**: `npm run watch` (auto-rebuilds and serves docs on changes)
- **Serve docs locally**: `npm run serve` (runs at http://localhost:8000)

## Code Structure

### Key Directories
- **`src/`**: OpenRPC specification files organized by namespace
  - `src/eth/`: Ethereum JSON-RPC methods
  - `src/engine/`: Engine API methods
  - `src/debug/`: Debug namespace methods
  - `src/schemas/`: JSON schema definitions
- **`docs/`**: Documentation and reference guides
  - `docs/reference/contributors-guide.md`: Contribution guidelines
  - `docs/reference/tests.md`: Test generation guide
- **`scripts/`**: Build and validation scripts
- **`tests/`**: Test cases for the specification

### Important Files
- **`openrpc.json`**: Compiled specification (generated, do not edit directly)
- **`package.json`**: NPM dependencies and scripts
- **`open-rpc-generator-config.json`**: Configuration for OpenRPC generator

## Contribution Guidelines

### Making Changes to the Specification

1. **Edit source files** in the `src/` directory, not the compiled `openrpc.json`
2. **Follow OpenRPC specification**: Refer to [OpenRPC spec](https://spec.open-rpc.org/) and [JSON Schema spec](https://json-schema.org/)
3. **Add test cases**: Include test cases for new methods or changes (see `docs/reference/tests.md`)
4. **Build and validate**: Always run `npm run build:spec` and `npm run lint` before committing
5. **Run tests**: Execute `npm test` to validate changes with speccheck

### Guiding Principles

When proposing changes, consider:
- **Necessity**: Is this method strictly necessary? Will it be widely used?
- **Implementation Complexity**: Can all clients reasonably implement this?
- **Backwards Compatibility**: Never break existing methods; propose new ones instead

### Code Style and Standards

- Maintain consistent formatting in YAML specification files
- Use clear, descriptive method and parameter names
- Provide comprehensive descriptions for all methods, parameters, and return values
- Include examples where helpful
- Follow existing patterns in the codebase

## CI/CD Workflow

The repository uses GitHub Actions for continuous integration:
- **Linting**: Validates OpenRPC spec, GraphQL schema, and runs spellcheck
- **Testing**: Runs speccheck to validate specification against test cases
- **Node.js version**: Tests run on Node.js 22
- **Go version**: Requires Go 1.18+ for speccheck tool

## Common Tasks for Copilot

### Adding a New JSON-RPC Method

1. Add the method definition in the appropriate `src/` subdirectory
2. Define schemas for parameters and results in `src/schemas/`
3. Create test cases in `tests/`
4. Run `npm run build:spec` to compile
5. Run `npm run lint` to validate
6. Run `npm test` to verify tests pass

### Updating Documentation

1. Edit files in `docs/` directory
2. Update `README.md` if necessary (it gets copied to docs)
3. Run `npm run build:docs` to generate updated documentation
4. Preview with `npm run serve`

### Fixing Validation Errors

1. Run `npm run lint` to see specific validation errors
2. Check OpenRPC and JSON Schema specifications for proper syntax
3. Ensure all `$ref` references are valid and resolvable
4. Rebuild with `npm run build:spec` after fixes

## Important Notes

- This is a specification repository, not an implementation
- Changes require consensus from Ethereum client teams
- See `docs/reference/contributors-guide.md` for the full standardization process
- Breaking changes to existing methods are not accepted
- Always coordinate major changes through AllCoreDevs calls
