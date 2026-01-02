# GitHub Copilot Instructions

This document provides guidelines for GitHub Copilot when working with this Node.js project.

## Project Overview

This is a Node.js/TypeScript project that updates Firewalla target lists using ASN (Autonomous System Number) data from BGP routing tables.

## Code Style and Formatting

- **Linter/Formatter**: Use Biome for both linting and formatting
- **Code Style**: Follow the Biome configuration in `biome.json`
- **Formatting**: 
  - Use single quotes for strings
  - Use 2 spaces for indentation
  - Line width: 100 characters
  - Always use semicolons
  - ES5 trailing commas

## Language and Runtime

- **Language**: TypeScript
- **Runtime**: Node.js 12 or higher
- **Target**: ES2018 for compatibility with Node 12+
- **Module System**: CommonJS

## Development Workflow

### Before Making Changes
1. Run `npm run check` to verify code quality
2. Review existing code patterns and follow them

### Making Changes
1. Write clear, self-documenting code
2. Add comments only when necessary to explain complex logic
3. Follow existing naming conventions
4. Keep functions focused and single-purpose

### After Making Changes
1. Run `npm run format` to format code
2. Run `npm run lint:fix` to auto-fix linting issues
3. Run `npm run check:fix` to fix all issues automatically
4. Run `npm run build` to ensure the project builds successfully

## Building and Bundling

- The project uses **esbuild** to bundle the TypeScript code into a standalone Node.js executable
- Build command: `npm run build`
- Output: `dist/asn-updater.js` (bundled, standalone file with shebang)
- The bundled file can be executed directly with `node dist/asn-updater.js`

## Dependencies

### Production Dependencies
- `axios`: HTTP client for API calls and data fetching

### Development Dependencies
- `@biomejs/biome`: Linting and formatting
- `esbuild`: Fast bundler for TypeScript
- `typescript`: TypeScript compiler
- `@types/node`: Node.js type definitions

## Project Structure

```
.
├── src/                    # Source TypeScript files
│   └── asn-updater.ts     # Main application file
├── dist/                   # Build output (bundled JS)
├── .github/               # GitHub configuration
│   └── copilot-instructions.md
├── biome.json             # Biome configuration
├── tsconfig.json          # TypeScript configuration
└── package.json           # Project metadata and scripts
```

## Error Handling

- Always use try-catch blocks for async operations
- Provide meaningful error messages
- Log errors to console with context
- Exit with appropriate error codes (0 for success, 1 for errors)

## Environment Variables

The application uses environment variables for configuration, loaded from a .env file or shell environment:
- `MSP_DOMAIN`: Firewalla MSP domain (required)
- `FIREWALLA_TOKEN`: Firewalla API token (required)

The application uses dotenv to load environment variables from a .env file in the project root.

## Command-Line Arguments

The application supports the following command-line arguments:
- `--dry-run`, `-d`: Preview changes without updating target lists
- `--help`, `-h`: Show help message

## API Integration

- Use axios for HTTP requests
- Always set appropriate timeouts
- Include user-agent headers where required
- Handle HTTP errors gracefully

## Code Patterns to Follow

1. **Async/Await**: Prefer async/await over promises for better readability
2. **Type Safety**: Use TypeScript types and interfaces
3. **Immutability**: Prefer const over let where possible
4. **Error Messages**: Include context in error messages
5. **Console Logging**: Use descriptive log messages for operations

## Testing

- Manual testing: Set environment variables in .env file and run the bundled output
- Always test with --dry-run flag first

## Security

- Never commit tokens or credentials
- Use environment variables for sensitive data
- Validate all external data inputs
- Use secure HTTPS connections for API calls

## Performance

- Cache data when appropriate (e.g., BGP table data)
- Use concurrent operations where possible (Promise.all)
- Set reasonable timeouts for network requests

## Documentation

- Keep README.md updated with usage instructions
- Document environment variables and their purpose
- Include examples in documentation
- Add inline comments for complex algorithms only
