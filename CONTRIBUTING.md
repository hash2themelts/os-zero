# Contributing to OS Zero

> **"FUTURE IS YOURS LEAVE A LEGACY"**

Thank you for considering contributing to OS Zero! This document outlines guidelines and procedures.

## Code of Conduct

- Be respectful and inclusive
- Welcome diverse perspectives
- Report violations to maintainers
- No discrimination or harassment

## Copyright & License

**By contributing to OS Zero, you agree that:**

1. Your contributions will be licensed under the MIT License
2. You retain copyright to your original work
3. You grant hash2themelts a perpetual license to use your code
4. You have the right to submit the work

Add this header to new files:

```rust
// SPDX-License-Identifier: MIT
// Copyright (c) 2024 [Your Name]
// 
// Contributed to OS Zero
// https://github.com/hash2themelts/os-zero
```

## Getting Started

1. **Fork** the repository
2. **Clone** your fork: `git clone https://github.com/YOUR_USERNAME/os-zero.git`
3. **Create** a branch: `git checkout -b feature/your-feature`
4. **Make** your changes
5. **Commit**: `git commit -am "Add your feature"`
6. **Push**: `git push origin feature/your-feature`
7. **Create** a Pull Request

## Contribution Areas

### High Priority

- [ ] TPM integration (secure audit log)
- [ ] Reproducible build system
- [ ] Full Ed25519 implementation (currently stub)
- [ ] Multi-user support
- [ ] Container runtime

### Medium Priority

- [ ] Performance optimizations
- [ ] More malware signatures
- [ ] Additional file format support
- [ ] Network protocol improvements
- [ ] Documentation improvements

### Low Priority

- [ ] UI enhancements
- [ ] Example programs
- [ ] Educational materials
- [ ] Tool development

## Before You Start

- [ ] Check existing issues/PRs (don't duplicate)
- [ ] Open an issue first for major changes
- [ ] Discuss in comments before coding
- [ ] Understand the security model

## Code Guidelines

### Rust Code

```rust
// SPDX-License-Identifier: MIT
// Copyright (c) 2024 [Your Name]

//! Module documentation
//! 
//! Clear explanation of what this module does

/// Function documentation with examples
/// 
/// # Example
/// ```
/// let result = my_function();
/// assert_eq!(result, expected);
/// ```
pub fn my_function() -> Result<T, E> {
    // Implementation
}
```

### Assembly Code

```asm
; SPDX-License-Identifier: MIT
; Copyright (c) 2024 [Your Name]
;
; Clear comment about what this does

section .text
    global my_function
    my_function:
        ; Implementation with clear comments
```

### Testing

```bash
# Run all tests
cargo test

# Run specific test
cargo test test_name

# Run with output
cargo test -- --nocapture
```

### Documentation

- Write clear comments
- Add function documentation
- Include examples
- Update README if needed
- Document security implications

## Security Review Checklist

Before submitting, ensure:

- [ ] No hardcoded secrets or keys
- [ ] Input validation on all boundaries
- [ ] Error handling (no unwrap in critical paths)
- [ ] No unsafe code without documentation
- [ ] Consider side-channel attacks
- [ ] Check for buffer overflows
- [ ] Verify cryptographic correctness

## Commit Messages

```
[type]: Brief description

Longer explanation of what changed and why.

- Bullet point of specific changes
- Another change

Fixes #123
```

**Types:**
- `feat:` New feature
- `fix:` Bug fix
- `docs:` Documentation
- `refactor:` Code reorganization
- `test:` Test additions
- `security:` Security fix
- `perf:` Performance improvement

## Pull Request Process

1. **Update** documentation
2. **Add** tests for new features
3. **Run** `cargo test`
4. **Check** code formatting: `cargo fmt`
5. **Lint**: `cargo clippy`
6. **Write** clear PR description
7. **Link** related issues
8. **Respond** to review feedback

### PR Description Template

```markdown
## Description
Brief overview of changes

## Motivation
Why this change is needed

## Changes
- Change 1
- Change 2

## Testing
How to test this change

## Security Implications
Any security considerations

## Checklist
- [ ] Tests pass
- [ ] Documentation updated
- [ ] No breaking changes
- [ ] Copyright/license headers added
```

## Review Process

1. **Automated checks** (CI/CD)
2. **Code review** by maintainers
3. **Security review** (if needed)
4. **Approval** and merge

Review may request:
- Changes to code
- Additional tests
- Documentation updates
- Security audit

## Licensing & Copyright

**Important:** By submitting a PR, you grant OS Zero:

- Right to use your code under MIT License
- Right to modify your code
- Right to sublicense
- Right to include in distributions

You retain:
- Copyright to your original work
- Right to use elsewhere under compatible licenses

## Questions?

- Open an issue
- Discuss in PR comments
- Check existing documentation
- Email maintainers

## Code of Conduct

### Be Respectful

- Welcome different viewpoints
- Assume good intentions
- Engage professionally
- Listen actively

### No Harassment

- No discrimination
- No personal attacks
- No inappropriate language
- No doxxing/privacy violations

### Report Issues

Found harassment? Contact maintainers immediately.

---

## License

By contributing, you agree to license your work under the MIT License.

See [LICENSE](LICENSE) for full terms.

---

**FUTURE IS YOURS LEAVE A LEGACY**

*Thank you for making OS Zero better!*
