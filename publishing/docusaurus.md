# Publishing with Docusaurus

## Minimal Setup

1. Initialize Docusaurus in a publishing repository or docs site branch.
2. Copy this repository's content into the Docusaurus `docs` directory.
3. Copy `docusaurus/sidebars.js` to the site root as `sidebars.js`.
4. Configure navbar and docs route in `docusaurus.config.js`.

## Run and Preview Locally

From the parent directory of this repository:

```bash
npx create-docusaurus@latest security-docs classic
cd security-docs
rm -rf docs/*
cp -R ../platform-docs/* docs/
rm -rf docs/docusaurus docs/.github
cp ../platform-docs/docusaurus/sidebars.js ./sidebars.js
npm ci
npm run start
```

Production-like local preview:

```bash
cd security-docs
npm run build
npm run serve
```

If `npm run start` fails, run `npm run build` to get actionable validation output.

## CI Workflows In This Repository

Two workflows build and publish this content automatically; no local scaffold needs to be committed because each workflow generates a transient Docusaurus site at runtime:

- GitHub Pages deployment: [.github/workflows/publish-platform-docs-pages.yml](../.github/workflows/publish-platform-docs-pages.yml)
- GHES-friendly static branch publish: [.github/workflows/publish-platform-docs-static-branch.yml](../.github/workflows/publish-platform-docs-static-branch.yml)

### 1) GitHub.com Pages Flow

- Builds a transient Docusaurus site at runtime from the current repository docs content and `docusaurus/sidebars.js`.
- Deploys generated `build` output using Pages actions.

Prerequisites:

1. Enable GitHub Pages in repository settings.
2. Ensure workflow permissions allow `pages:write` and `id-token:write`.

### 2) GHES or Generic Static Hosting Flow

- Builds a transient Docusaurus site and force-pushes static files to a target branch (default `gh-pages`).
- Works for GHES and GitHub.com where an external/static host serves branch content.

Manual dispatch input:

- `target_branch`: branch to receive built site content.

Typical hosting integration:

1. Configure your internal static host to serve the `gh-pages` branch.
2. Trigger workflow on merge to `main` or manually for controlled releases.
3. Validate published site from host URL.

## Versioning

Use Docusaurus docs versioning for major workflow contract changes (`v1`, `v2`).

## CI/CD

Publish with GitHub Pages or internal static hosting.

## Split Repository Publishing

If you split content into multiple repositories, publish each repository with its
own Docusaurus site or aggregate multiple repositories into a single site via CI
sync jobs that copy each repo into one combined docs tree before build.
