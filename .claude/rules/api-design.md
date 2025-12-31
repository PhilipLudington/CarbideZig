---
globs: ["*.zig"]
---

# API Design Rules

## A1: Accept Slices
- Accept `[]const u8` not fixed arrays or pointer+length
- More flexible for callers

```zig
// GOOD
pub fn process(data: []const u8) void

// BAD
pub fn process(data: [*]const u8, len: usize) void
```

## A2: Optional Types
- Use `?T` for nullable values
- Never use magic values like -1 or null pointers

```zig
// GOOD
pub fn find(haystack: []const u8, needle: u8) ?usize

// BAD
pub fn find(haystack: []const u8, needle: u8) isize  // -1 for not found
```

## A3: Configuration Structs
- Use struct with defaults for optional parameters
- Enables clean call sites with `.{}`

```zig
pub const Config = struct {
    port: u16 = 8080,
    timeout_ms: u32 = 30_000,
    tls_enabled: bool = false,
};

// Usage
const server = try Server.init(allocator, .{});
const server = try Server.init(allocator, .{ .port = 443 });
```

## A4: Return Structs
- Return structs for multiple values
- Avoid out parameters except for error info

```zig
pub const Result = struct { quotient: i32, remainder: i32 };
pub fn divide(a: i32, b: i32) Result
```

## A5: Const Correctness
- Use `const` slices when not modifying: `[]const u8`
- Use `*const Self` for read-only methods

## A6: Builder Pattern
- Use for complex object construction
- Return `*Self` from setters for chaining

## A7: Writer/Reader Pattern
- Accept `anytype` writer/reader for I/O flexibility
- Works with files, buffers, network streams
