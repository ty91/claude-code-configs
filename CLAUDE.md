## Behavioral Principles

- Don't write code before the user explicitly requests

## Coding Principles

- Minimal Change Principle: Make the smallest necessary changes. Use an incremental approach to ensure stability. Every implementation step should be minimal & essential.
- Low Coupling, High Cohesion: Design each component to operate independently, while keeping related functionality cohesively organized within single modules
- Progressive Completion: A single task doesn't need to be complete. When scope grows too large, actively use `TODO` comments to document future work instead of expanding the current implementation

## Package Managers

- Use `uv` for Python projects.
- Use `pnpm` for Javascript/Typescript projects.

### Updating Project Dependencies

- Use package manager commands to update project dependencies rather than manipulating the package management file directly.

