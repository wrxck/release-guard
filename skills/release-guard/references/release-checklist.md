# Release Checklist Reference

Detailed procedures for each step of the open source package release process.

## npm publish workflow

### Authentication

```bash
# Check if logged in
npm whoami

# Login if needed
npm login

# For scoped packages, verify org access
npm org ls <org-name>
```

### Pre-publish checks

```bash
# Verify what will be published
npm pack --dry-run

# Check for sensitive files that shouldn't be published
# Ensure .npmignore or "files" field in package.json is correct

# Run prepublish scripts
npm run prepublishOnly  # if it exists
npm run build           # if there's a build step
```

### Publishing

```bash
# Standard publish
npm publish

# Scoped package (first time or public)
npm publish --access public

# With a specific tag
npm publish --tag next
npm publish --tag beta

# With OTP for 2FA
npm publish --otp=123456
```

### Post-publish verification

```bash
# Verify the version is live
npm view <package-name> version
npm view <package-name> dist-tags

# Verify the package installs correctly
cd /tmp && npm install <package-name>@<version>
```

### 2FA considerations

- If 2FA is enabled on the npm account, `--otp` is required for publish
- The OTP changes every 30 seconds - have the authenticator app ready
- Consider using `npm token create` for CI/CD pipelines with automation tokens

## GitHub release creation

### Using gh CLI

```bash
# Create a release with auto-generated notes
gh release create v1.2.3 --generate-notes

# Create a release with custom notes
gh release create v1.2.3 --title "v1.2.3" --notes "Release notes here"

# Create a release from a specific branch/commit
gh release create v1.2.3 --target main

# Create a pre-release
gh release create v1.2.3-beta.1 --prerelease

# Create a draft release
gh release create v1.2.3 --draft

# Attach assets to a release
gh release create v1.2.3 ./dist/package.tgz
```

### Release notes format

```markdown
## What's Changed

### Added
- New feature X for handling Y

### Fixed
- Bug where Z would fail under condition W

### Changed
- Updated dependency A to v2.0

**Full Changelog**: https://github.com/owner/repo/compare/v1.1.0...v1.2.3
```

## Semver rules

### When to bump what

| Change type | Bump | Example |
|---|---|---|
| Bug fix (no API change) | patch | 1.2.3 -> 1.2.4 |
| Documentation only | patch | 1.2.3 -> 1.2.4 |
| Dependency update (compatible) | patch | 1.2.3 -> 1.2.4 |
| New feature (backwards compatible) | minor | 1.2.3 -> 1.3.0 |
| New export or option | minor | 1.2.3 -> 1.3.0 |
| Deprecation (old way still works) | minor | 1.2.3 -> 1.3.0 |
| Breaking API change | major | 1.2.3 -> 2.0.0 |
| Removed export or option | major | 1.2.3 -> 2.0.0 |
| Changed default behavior | major | 1.2.3 -> 2.0.0 |
| Minimum Node.js version bump | major | 1.2.3 -> 2.0.0 |

### Pre-release versions

- Alpha: `1.2.3-alpha.1` - early, unstable, API may change significantly
- Beta: `1.2.3-beta.1` - feature complete, may have bugs
- RC: `1.2.3-rc.1` - release candidate, should be stable

### Version 0.x.y

- Before 1.0.0, the API is not considered stable
- Minor bumps in 0.x can include breaking changes
- Use 1.0.0 to signal "this API is stable and follows semver properly"

## Changelog format

Follow [Keep a Changelog](https://keepachangelog.com/) conventions.

### Template

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/),
and this project adheres to [Semantic Versioning](https://semver.org/).

## [Unreleased]

## [1.2.3] - 2026-04-06

### Added
- New feature description

### Changed
- Changed behavior description

### Deprecated
- Feature that will be removed in future versions

### Removed
- Feature that was removed

### Fixed
- Bug fix description

### Security
- Security fix description

## [1.2.2] - 2026-03-15

### Fixed
- Previous bug fix

[Unreleased]: https://github.com/owner/repo/compare/v1.2.3...HEAD
[1.2.3]: https://github.com/owner/repo/compare/v1.2.2...v1.2.3
[1.2.2]: https://github.com/owner/repo/compare/v1.2.1...v1.2.2
```

### Guidelines

- Keep an `[Unreleased]` section at the top for ongoing changes
- Date format: YYYY-MM-DD (ISO 8601)
- List changes under the appropriate category
- Write entries for humans, not machines - be descriptive
- Link to issues or PRs where relevant
- Include comparison links at the bottom of the file

## Documentation site patterns

### Expected domains

For a package named `my-package`:

- Primary: `my-package.hesketh.pro`
- Alternative: `my-package.matthesketh.pro`

### Minimum documentation content

A good documentation site should include:

1. **Hero / landing section** - one-sentence description of what the package does
2. **Installation** - `npm install` command, supported environments
3. **Quick start** - minimal working example (copy-pasteable)
4. **API reference** - every export, function, option, and type documented
5. **Examples** - real-world usage patterns
6. **Changelog** - link to or inline the changelog
7. **License** - link to the license

### Deployment options

- **Cloudflare Pages** - recommended for hesketh.pro domains (already using Cloudflare DNS)
- **GitHub Pages** - simple alternative, works well for static docs
- **Vercel** - good for Next.js-based documentation sites

### DNS setup

For `*.hesketh.pro` domains:

1. Add a CNAME record pointing to the deployment platform
2. Configure the custom domain in the deployment platform settings
3. Ensure HTTPS is enabled (automatic with Cloudflare)
