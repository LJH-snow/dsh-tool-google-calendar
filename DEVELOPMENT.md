# Development

## Local checks

Run the same checks used by the aggregate release matrix:

```bash
npm ci
npm run typecheck
npm test
npm run build
npm pack --dry-run
```

The test suite injects `fetch` and DNS lookup. It covers OAuth URL construction, token refresh caching, pagination, bounded event mapping, error mapping, and the shared endpoint-security vectors. No Google credentials are needed.

## OAuth helper

`npm run auth:calendar` is an explicit local OAuth helper. It uses a loopback callback by default, checks the OAuth state, supports PKCE, and requests only read-only Calendar scopes. Use `--print-url` or `--no-open` when you do not want a browser to open. The helper prints a Cordis configuration snippet; never commit that snippet with real secrets.

## Release

Publishing is tag-driven by `.github/workflows/publish-npm.yml`. The tag must match the package version (`vX.Y.Z`). The workflow runs typecheck, tests, build, pack, and npm provenance publishing with GitHub OIDC Trusted Publishing.

Do not create a tag for a version already present on npm. Configure npm Trusted Publishing for owner `LJH-snow`, repository `dsh-tool-google-calendar`, workflow `publish-npm.yml`, and enable **Allow npm publish** before the first OIDC release.
