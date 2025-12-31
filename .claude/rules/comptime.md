---
globs: ["*.zig"]
---

# Comptime Programming Rules

## CT1: Compile-Time Constants
- Use `const` at module level for compile-time values
- Use snake_case for comptime constants

```zig
pub const max_buffer_size = 4096;
pub const version = std.SemanticVersion{ .major = 1, .minor = 0, .patch = 0 };
```

## CT2: Comptime Functions
- Use comptime functions for static computation
- Generate lookup tables, validate configs

```zig
fn generateLookupTable() [256]bool {
    var table: [256]bool = undefined;
    for (0..256) |i| {
        table[i] = isValidChar(@intCast(i));
    }
    return table;
}

const lookup = generateLookupTable();  // Computed at compile time
```

## CT3: Generic Types
- Use `comptime T: type` for generic containers
- Return struct from function for generated types

```zig
pub fn ArrayList(comptime T: type) type {
    return struct {
        const Self = @This();
        items: []T,
        // ...
    };
}
```

## CT4: Type Reflection
- Use `@typeInfo` for introspection
- Use `@TypeOf` to get type of value
- Use `inline for` to iterate struct fields

```zig
inline for (@typeInfo(T).Struct.fields) |field| {
    // Process each field at compile time
}
```

## CT5: Comptime Validation
- Validate generic parameters at compile time
- Use `@compileError` for clear error messages

```zig
pub fn Buffer(comptime size: usize) type {
    if (size == 0) {
        @compileError("Buffer size must be > 0");
    }
    return struct {
        data: [size]u8 = undefined,
    };
}
```

## CT6: @embedFile
- Use for embedding static resources
- File contents available at compile time

```zig
const schema = @embedFile("schema.json");
```
