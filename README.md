# CarbideZig

**Hardened Zig Development Standards for AI-Assisted Programming**

CarbideZig is a comprehensive coding standards framework designed for Claude Code, enabling developers to write safe, maintainable, and trustworthy Zig code with AI assistance.

## Installation

### Option A: Add CarbideZig to an Existing Project

This is the most common use case. Copy the CarbideZig configuration files into your existing Zig project:

```bash
# Clone CarbideZig somewhere on your system
git clone https://github.com/yourusername/CarbideZig.git ~/CarbideZig

# Navigate to your existing Zig project
cd /path/to/your/zig-project

# Copy the Claude Code rules (required for AI assistance)
cp -r ~/CarbideZig/.claude .

# Copy the standards documentation (optional but recommended)
cp ~/CarbideZig/STANDARDS.md .
cp ~/CarbideZig/CARBIDE.md .
```

**What you get:**
- `.claude/rules/` — 10 rule files automatically loaded by Claude Code for context-aware assistance
- `.claude/commands/` — Slash commands for code review, validation, and project creation
- `STANDARDS.md` — Complete coding standards reference
- `CARBIDE.md` — Quick reference card

### Option B: Create a New Project from Scratch

Once CarbideZig is installed in a directory, you can use the `/carbide-init` command:

```bash
# First, set up a temporary directory with CarbideZig rules
mkdir my-workspace && cd my-workspace
cp -r ~/CarbideZig/.claude .

# Now start Claude Code and use the init command
claude
# Inside Claude Code:
/carbide-init my-project
```

This creates a complete project structure with `build.zig`, `src/main.zig`, and all standard files.

### Option C: Use the Template Directly

```bash
cp -r ~/CarbideZig/templates/project my-project
cd my-project
```

## Quick Start

After installation, build and run your project:

```bash
cd my-project
zig build          # Build
zig build run      # Run
zig build test     # Test
zig fmt src/       # Format
```

## Using AI-Assisted Development

Once `.claude/rules/` is in your project, Claude Code automatically loads the CarbideZig standards. You'll get:

- **Automatic rule enforcement** — Claude follows Zig best practices for memory, errors, naming, etc.
- **Slash commands** — Use `/carbide-review`, `/carbide-check`, and `/carbide-safety`
- **Context-aware suggestions** — Claude understands your project follows CarbideZig patterns

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

## Verifying Installation

After adding CarbideZig to your project, verify it's working:

1. Start Claude Code in your project directory: `claude`
2. Ask Claude about Zig memory management — it should reference CarbideZig rules
3. Run `/carbide-check` to validate your build configuration
4. Run `/carbide-review` to review existing code against standards

## Requirements

- Zig 0.13.0 or later
- Claude Code (for AI-assisted features)

## License

MIT License — See [LICENSE](LICENSE) for details.

---

*CarbideZig — Explicit over implicit. Simple over clever. Safe over fast.*
