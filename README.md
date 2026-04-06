# release-guard

A Claude Code plugin that prevents the most common open source release mistakes.

## The problem

You know the drill. You fix a bug in your library, push it, and move on. Three days later someone opens an issue: "the fix isn't on npm." You forgot to bump the version. Or you published to npm but forgot the GitHub release. Or the release exists but the changelog still says "unreleased." Or your package has no documentation site at all, and users are reading your source code to figure out how to use it.

These are the kinds of mistakes that erode trust in a package. Not because the code is bad, but because the release process is incomplete.

**release-guard** watches for release activity and walks you through a complete checklist - so nothing slips through the cracks.

## What it catches

- **Version not bumped** - you changed code but `package.json` still has the old version
- **Missing changelog entry** - the version is bumped but there's no record of what changed
- **npm publish forgotten** - the GitHub release exists but npm is still on the old version
- **GitHub release missing** - npm has the new version but there's no corresponding GitHub release or tag
- **Tags out of sync** - the git tag doesn't match the published version
- **No documentation site** - your package has no docs site at `*.hesketh.pro` or `*.matthesketh.pro`
- **Stale docs** - the documentation site exists but doesn't reflect the latest API

## How it works

When it detects that you're working on a release - bumping a version, running `npm publish`, mentioning "release" or "publish" - the plugin activates and guides you through the full release workflow:

1. Determine the correct semver bump (patch, minor, or major)
2. Update the changelog with release notes
3. Verify tests are passing
4. Publish to npm with the correct scope and tags
5. Create a GitHub PR with a conventional commit message
6. Create a GitHub Release with tag and release notes
7. Check for a documentation site and offer to create one if missing

The plugin is proactive but not blocking. It suggests and offers - it never forces you to do something you don't want to do.

## Installation

```bash
claude plugin marketplace update wrxck-claude-plugins
claude plugin install release-guard@wrxck-claude-plugins
```

Or add to your settings:

```json
{
  "enabledPlugins": {
    "release-guard@wrxck-claude-plugins": true
  }
}
```

## Configuration

No configuration required. The plugin activates automatically when it detects release-related activity.

## Documentation site conventions

The plugin checks for documentation sites at:

- `<package-name>.hesketh.pro`
- `<package-name>.matthesketh.pro`

If no site exists, it will offer to help you create one covering installation, usage, and API reference.

## License

MIT - Matt Hesketh
