# CarbideZig

**Hardened Zig Development Standards for AI-Assisted Programming**

CarbideZig is a comprehensive coding standards framework designed for Claude Code, enabling developers to write safe, maintainable, and trustworthy Zig code with AI assistance.

## Quick Start

### 1. Create a New Project

```bash
# Using the slash command (in Claude Code)
/carbide-init my-project

# Or manually copy the template
cp -r templates/project my-project
```

### 2. Build and Run

```bash
cd my-project
zig build          # Build
zig build run      # Run
zig build test     # Test
zig fmt src/       # Format
```

### 3. Use AI-Assisted Development

With Claude Code, the `.claude/rules/` are automatically loaded, providing context-aware assistance that follows CarbideZig standards.

## Features

### Comprehensive Standards

- **[STANDARDS.md](STANDARDS.md)** — Complete coding standards covering naming, memory, errors, API design, testing, security, and more

### AI-Optimized Rules

10 rule files in `.claude/rules/` for automatic Claude Code integration:

| Rule | Description |
|------|-------------|
| `memory.md` | Allocator patterns, ownership, defer |
| `errors.md` | Error unions, try/catch, errdefer |
| `naming.md` | Zig naming conventions |
| `api-design.md` | Slices, optional types, config structs |
| `security.md` | Input validation, safety modes |
| `testing.md` | Test patterns, testing allocator |
| `concurrency.md` | Threads, atomics, mutexes |
| `logging.md` | std.log patterns |
| `comptime.md` | Compile-time programming |
| `build.md` | build.zig patterns |

### Slash Commands

| Command | Description |
|---------|-------------|
| `/carbide-init` | Create new CarbideZig project |
| `/carbide-review` | Code review against standards |
| `/carbide-check` | Run build, test, format checks |
| `/carbide-safety` | Security-focused review |

### Pattern Documentation

Detailed implementation guides in `docs/patterns/`:

- Memory management patterns
- Error handling patterns
- API design patterns
- Resource lifecycle patterns

### Security Documentation

Security-focused guides in `docs/security/`:

- Zig safety features
- Avoiding undefined behavior
- Memory safety practices

## Project Structure

```
CarbideZig/
├── STANDARDS.md           # Comprehensive coding standards
├── CARBIDE.md             # Quick reference card
├── .claude/
│   ├── commands/          # Slash commands
│   └── rules/             # AI rules (auto-loaded)
├── docs/
│   ├── patterns/          # Implementation patterns
│   └── security/          # Security guides
├── templates/
│   ├── build.zig          # Build template
│   └── project/           # Project scaffold
└── examples/
    └── hello/             # Example project
```

## Core Principles

1. **Leverage the Type System** — Let the compiler catch errors at compile time
2. **Explicit Resource Management** — Every allocation has an owner and a cleanup path
3. **Fail Loudly** — Errors should be visible and handled, never silently ignored
4. **Comptime Over Runtime** — Prefer compile-time computation when possible
5. **Minimal Dependencies** — Standard library first, external dependencies sparingly

## Example

```zig
const std = @import("std");
const Allocator = std.mem.Allocator;

pub const Config = struct {
    max_size: usize = 1024,
    timeout_ms: u32 = 5000,
};

pub const Client = struct {
    allocator: Allocator,
    buffer: []u8,

    pub fn init(allocator: Allocator, config: Config) !Client {
        const buffer = try allocator.alloc(u8, config.max_size);
        errdefer allocator.free(buffer);

        return Client{
            .allocator = allocator,
            .buffer = buffer,
        };
    }

    pub fn deinit(self: *Client) void {
        self.allocator.free(self.buffer);
        self.* = undefined;
    }
};
```

## Naming Conventions

| Element | Style | Example |
|---------|-------|---------|
| Types | PascalCase | `HttpClient` |
| Functions | camelCase | `readFile` |
| Variables | camelCase | `bufferSize` |
| Constants | snake_case | `max_size` |
| Files | snake_case | `http_client.zig` |

## Integrating with Existing Projects

1. Copy `STANDARDS.md` and `CARBIDE.md` to your project root
2. Copy `.claude/` directory for AI assistance
3. Review code against standards using `/carbide-review`

## Requirements

- Zig 0.13.0 or later
- Claude Code (for AI-assisted features)

## License

MIT License — See [LICENSE](LICENSE) for details.

---

*CarbideZig — Explicit over implicit. Simple over clever. Safe over fast.*
