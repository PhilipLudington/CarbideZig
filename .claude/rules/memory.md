---
globs: ["*.zig"]
---

# Memory Management Rules

## M1: Allocator Injection
- ALWAYS accept `Allocator` as a parameter for functions that allocate
- NEVER use global allocators like `std.heap.page_allocator` directly
- Store allocator in struct if needed for deinit

```zig
// GOOD
pub fn init(allocator: Allocator) !Self

// BAD
pub fn init() !Self  // Hidden allocation
```

## M2: Ownership Documentation
- Document who owns returned pointers/slices
- Use "Caller owns" or "Valid until" patterns

```zig
/// Caller owns the returned slice and must free it.
pub fn duplicate(allocator: Allocator, data: []const u8) ![]u8
```

## M3: Immediate Cleanup
- Place `defer` immediately after resource acquisition
- Use `errdefer` for cleanup on error paths only

```zig
const buffer = try allocator.alloc(u8, size);
defer allocator.free(buffer);  // Immediately after

const file = try openFile(path);
errdefer file.close();  // Only on error
```

## M4: Struct Lifecycle
- Use `init/deinit` for stack-allocated structs
- Use `create/destroy` for heap-allocated structs
- Set `self.* = undefined` at end of deinit to poison

## M5: Arena for Batch Operations
- Use `ArenaAllocator` when many allocations share lifetime
- Frees everything at once, simpler code

## M6: Testing Allocator
- Use `std.testing.allocator` in tests (checks leaks)
- Use `std.heap.GeneralPurposeAllocator` for debug builds
