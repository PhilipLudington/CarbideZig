---
globs: ["build.zig", "build.zig.zon"]
---

# Build System Rules

## B1: Standard Structure
- Use `standardTargetOptions` and `standardOptimizeOption`
- Define clear build steps: build, run, test, docs

```zig
pub fn build(b: *std.Build) void {
    const target = b.standardTargetOptions(.{});
    const optimize = b.standardOptimizeOption(.{});
    // ...
}
```

## B2: Artifact Installation
- Use `b.installArtifact()` for libraries and executables
- Use named steps for discoverability

```zig
const lib = b.addStaticLibrary(.{ ... });
b.installArtifact(lib);

const run_step = b.step("run", "Run the application");
run_step.dependOn(&run_cmd.step);
```

## B3: Dependencies (build.zig.zon)
- Declare dependencies in build.zig.zon
- Use specific versions or commits

```zig
.dependencies = .{
    .zap = .{
        .url = "https://github.com/zigzap/zap/archive/v0.0.1.tar.gz",
        .hash = "...",
    },
},
```

## B4: Build Options
- Expose configurable options with `b.option`
- Use `addOptions` to pass to modules

```zig
const enable_logging = b.option(bool, "log", "Enable logging") orelse false;

const options = b.addOptions();
options.addOption(bool, "enable_logging", enable_logging);
lib.root_module.addOptions("config", options);
```

## B5: Test Configuration
- Configure test step with same target/optimize
- Allow running specific test files

```zig
const tests = b.addTest(.{
    .root_source_file = b.path("src/root.zig"),
    .target = target,
    .optimize = optimize,
});
const test_step = b.step("test", "Run unit tests");
test_step.dependOn(&b.addRunArtifact(tests).step);
```

## B6: Cross-Compilation
- Target options enable cross-compilation out of box
- Test on multiple targets in CI

```bash
zig build -Dtarget=x86_64-linux-gnu
zig build -Dtarget=aarch64-macos
```
