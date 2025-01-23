# OpenServ Documentation Repository

## Overview

The `openserv-docs` repository is the centralized hub for all OpenServ documentation, enabling synchronization with our GitBook documentation site. This setup allows us to maintain a single URL (`docs.openserv.ai`) for all project documentation, complying with GitBook's limitation of one repository sync per space.

The `SUMMARY.md` file in the root directory is pivotal as it outlines our documentation's Table of Contents on GitBook, directly influencing the structure and navigation of our online docs.

## Repository Structure

- **packages/**: Contains folders named after the repositories they represent, each with a README.md that acts as the documentation for that repo.

### Adding New Repositories

To add a new repository to the documentation system:

1. **Create a Folder**: Under `packages/`, create a new folder named after the repository. This maintains our naming convention.
2. **Add README.md**: Place a README.md file inside this folder. Adhere to the uppercase convention for README.md to ensure compatibility with our CI scripts.
3. **Update SUMMARY.md**: Add the new folder's README.md path to the `SUMMARY.md` file in a format that enhances readability on GitBook, such as: * [TypeScript SDK](packages/sdk/README.md).
4. **Add GitHub Actions to Automate Documentation Sync**: Implement a GitHub Actions workflow in the original repository to automatically update the documentation in `openserv-docs` whenever the README.md is modified. This keeps the original repository as the source of truth and ensures all changes are reflected in our centralized documentation.

### Important Note on File Naming and Paths

Please note that the automation of documentation synchronization relies heavily on the consistency of file names and paths. The CI workflows are configured to detect changes specifically to `README.md` files at predefined paths. Any changes to the file names or their locations within the repository must be manually updated in the CI configuration to ensure that the documentation continues to sync correctly. This manual update is crucial to maintain the integrity of the automated processes.

## Continuous Integration (CI) Workflow

Each public repository within the OpenServ organization, like `agent-examples` and `sdk`, contains a `.github/workflows/docs-sync.yml` CI workflow that syncs its README.md with `openserv-docs`. Here’s how the CI process facilitates seamless documentation updates:

### Workflow Steps

- **Detect Changes**: Triggered by commits to the `main` branch that modify the README.md.
- **Sync README**: Copies the updated README.md to the corresponding directory in `openserv-docs`.
- **Deploy**: Changes pushed to `openserv-docs` trigger an automatic rebuild in GitBook, which subsequently redeploys the updated documentation to `docs.openserv.ai`.

### Example CI Configuration

Here is a sample configuration for a repository CI workflow (`docs-sync.yml`):

```yaml
name: Sync README to openserv-docs

on:
  push:
    branches:
      - main
    paths:
      - 'README.md'

jobs:
  sync-readme:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v2
      - name: Checkout openserv-docs repository
        uses: actions/checkout@v2
        with:
          repository: 'openserv-labs/openserv-docs'
          token: ${{ secrets.GH_TOKEN }}
          path: 'openserv-docs'
      - name: Copy README.md to openserv-docs
        run: |
          cp README.md openserv-docs/packages/${{ github.event.repository.name }}/README.md
      - name: Commit and push if changed
        working-directory: ./openserv-docs
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add packages/${{ github.event.repository.name }}/README.md
          git commit -m "Update README from ${{ github.event.repository.name }}" || exit 0
          git push
```
