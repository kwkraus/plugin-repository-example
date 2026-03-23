# react-ts-unittest-expert

A specialized GitHub Copilot CLI agent for generating high-quality unit tests for React and TypeScript components.

## Overview

The `react-ts-unittest-expert` agent is designed to help developers create comprehensive, maintainable unit tests for React and TypeScript components. It provides expert guidance and generates test cases that follow modern user-centric testing practices using Vitest and React Testing Library.

## Features

- **Component Analysis**: Thoroughly analyzes React/TypeScript components to understand structure, props, state, and behavior
- **Test Generation**: Creates comprehensive unit tests covering:
  - Component rendering with various props
  - User interactions and event handling
  - State changes and side effects
  - Conditional rendering
  - Hook behavior (useState, useEffect, useContext, etc.)
  - Error states and edge cases
  - Async operations and loading states
- **Best Practices**: Generates tests following React Testing Library best practices and user-centric testing patterns
- **Project-Aware Guidance**: Adapts to the target project's existing test setup, helpers, and coverage policy
- **Prescriptive Mocking Guidance**: Recommends Vitest mocks by default and MSW for request-driven UI tests
- **Expert Guidance**: Provides recommendations for test structure, improvements, and testability concerns

## Installation

To install this plugin locally for development:

```bash
cd /path/to/plugin-repository-example
copilot plugin install ./plugins/react-ts-unittest-expert
```

To install from the marketplace (once published):

```bash
copilot plugin marketplace add kwkraus/plugin-repository-example
copilot plugin install react-ts-unittest-expert@plugin-marketplace-example
```

## Usage

### Using the Agent

In a Copilot CLI interactive session, invoke the agent:

```
/agent react-ts-unittest-expert
```

Then provide your request:
- "Generate unit tests for this component" (share the component code)
- "Create comprehensive test coverage for this React hook"
- "Write tests for this TypeScript component with async operations"
- "Help me test this form component"

### Example Workflow

1. Share your component code with the agent
2. The agent will analyze the component structure
3. It will generate comprehensive unit tests
4. Tests will include documentation and best practice patterns
5. You can request modifications or additional test cases

## Testing Patterns

The agent specializes in:

- **Component Testing**: Rendering, props validation, conditional rendering
- **User Interaction Testing**: Clicks, form inputs, submits, keyboard events
- **State Management**: useState, useReducer, context testing
- **Side Effects**: useEffect testing, mocking API calls
- **Hook Testing**: Custom hooks, hook composition, hook lifecycle
- **Async Testing**: Promises, async/await, API mocking, loading states
- **TypeScript**: Type-safe tests, generic component testing

## Mocking Recommendation

The default mocking approach for this plugin is:

- Use **Vitest built-in mocks** such as `vi.mock()`, `vi.fn()`, and `vi.spyOn()` for module boundaries, callbacks, utilities, router hooks, browser APIs, and third-party libraries.
- Use **MSW** for component and hook tests that depend on HTTP behavior so the UI exercises realistic request flows.
- Use direct `fetch` stubbing only for low-level API client unit tests or when the target project explicitly avoids MSW.

This keeps React tests aligned with Vitest while avoiding brittle UI tests that over-mock network behavior.

## Recommended Testing Tools

- **Framework**: Vitest
- **Testing Library**: @testing-library/react
- **User Events**: @testing-library/user-event
- **Mocking**: Vitest built-in mocks (`vi.mock`, `vi.fn`, `vi.spyOn`)
- **Preferred network mocking**: MSW (Mock Service Worker)
- **Async Testing**: waitFor, findBy queries, act()

## Best Practices Followed

- **User-Centric Testing**: Tests focus on user behavior, not implementation details
- **Meaningful Assertions**: Clear, descriptive test names and assertions
- **Test Isolation**: Each test is independent and can run in any order
- **Accessibility**: Tests verify accessible markup and interactions
- **DRY Principle**: Shared test utilities and fixtures to reduce duplication
- **Performance**: Tests are written to be fast and reliable (avoid flakiness)
- **Project Alignment**: Follow the target project's existing scripts, file layout, and coverage policy
- **Mocking Discipline**: Mock boundaries and network behavior deliberately instead of mocking component internals

## Requirements

- A React and/or TypeScript project
- Vitest selected or installed as the project's test runner
- React Testing Library or similar testing utilities
- GitHub Copilot CLI installed and configured

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
