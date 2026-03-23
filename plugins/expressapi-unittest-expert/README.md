# expressapi-unittest-expert

A GitHub Copilot CLI plugin that provides the `expressapi-unittest-engineer` custom agent for writing and improving unit tests for Node.js and Express APIs.

## Overview

The `expressapi-unittest-expert` plugin is focused on backend API testing with Jest and supertest. The bundled `expressapi-unittest-engineer` agent helps teams add coverage for Express routes, middleware, and services while following CommonJS patterns and practical HTTP-level testing practices.

The agent supports two working modes:

- **TDD Greenfield** for new API functionality
- **Retrofit** for existing code that needs tests, characterization coverage, and refactoring guidance

## Features

- **Express API test generation** for routes, middleware, and service logic
- **Jest and supertest guidance** for request/response-level tests
- **Retrofit testability assessment** for existing code with recommendations for safer refactoring
- **Coverage-oriented workflows** including edge cases, error paths, and invalid input handling
- **Database mocking patterns** for projects using `pg` or similar infrastructure dependencies
- **CommonJS-first conventions** for codebases that use `require()` and `module.exports`

## Installation

To install this plugin locally for development:

```bash
cd /path/to/plugin-repository-example
copilot plugin install ./plugins/expressapi-unittest-expert
```

To install from this marketplace:

```bash
copilot plugin marketplace add kwkraus/plugin-repository-example
copilot plugin install expressapi-unittest-expert@plugin-marketplace-example
```

## Usage

In a Copilot CLI interactive session, invoke the bundled agent:

```text
/agent expressapi-unittest-engineer
```

Example requests:

- "Write Jest tests for this Express route handler."
- "Add supertest coverage for these API endpoints."
- "Assess the testability of this existing Express module and suggest a retrofit plan."
- "Create characterization tests before I refactor this middleware."
- "Mock the database layer and add coverage for error cases and 404 responses."

## Agent Workflow

### TDD Greenfield

Use this mode when creating new route handlers, middleware, or services.

1. Clarify the required behavior and failure cases.
2. Write failing Jest tests first.
3. Run tests to confirm the red state.
4. Implement the smallest amount of code needed to pass.
5. Refactor while keeping the suite green.
6. Extend coverage for boundary conditions and error handling.

### Retrofit

Use this mode when adding tests to an existing API.

1. Score testability of the current code.
2. Start with quick wins such as route handlers, middleware, and utilities.
3. Add characterization tests for tightly coupled code.
4. Recommend refactors that improve isolation and dependency injection.
5. Expand coverage incrementally and track before/after results.

## Testing Patterns Covered

The agent is designed to help with:

- Route handler tests using Express and supertest
- Middleware tests with mocked `req`, `res`, and `next`
- Multi-query request flows such as create-and-fetch operations
- Header-driven behaviors such as `X-User-Id` request handling
- Error responses including `400`, `404`, and server failures
- Mocking database access instead of mocking the router itself

## Recommended Stack

- **Framework**: Jest
- **HTTP testing**: supertest
- **Runtime style**: CommonJS
- **API framework**: Express
- **Data layer**: Mock `pg` or other external I/O dependencies

## Prerequisites

Before using the agent effectively, your API project should have:

- GitHub Copilot CLI installed and configured
- A Node.js and Express codebase
- Jest installed as a dev dependency
- supertest installed as a dev dependency
- A testable app entry point such as `createApp()` that does not auto-start the server during tests

Example setup:

```bash
npm install --save-dev jest supertest
```

Typical package scripts:

```json
{
  "test": "jest",
  "test:watch": "jest --watch",
  "test:coverage": "jest --coverage"
}
```

## Best Practices Followed

- Test behavior rather than implementation details
- Mock external dependencies, especially databases and network calls
- Keep tests isolated so they pass in any order
- Use descriptive test names tied to observable API behavior
- Prefer HTTP-level coverage for routes instead of mocking the route layer
- Add characterization tests before changing hard-to-test legacy code

## Support

For issues, questions, or suggestions:

1. Check the [GitHub Copilot CLI documentation](https://docs.github.com/en/copilot/how-tos/copilot-cli)
2. Review the [GitHub Copilot CLI plugin reference](https://docs.github.com/en/copilot/reference/cli-plugin-reference)
3. Open an issue in the [plugin repository](https://github.com/kwkraus/plugin-repository-example)

## License

MIT

## Version

1.0.0

## Author

Kevin Kraus (kevin.kraus@microsoft.com)
