# homebrew-structkit

Homebrew tap for [StructKit](https://structkit.app) - YAML-first project scaffolding tool with AI/MCP integration.

## Installation

```bash
brew tap httpdss/structkit
brew install structkit
```

## Usage

Once installed, you can use the `structkit` command:

```bash
# Check version
structkit --version

# List available structures
structkit list

# Generate a project structure
structkit generate --vars module_name=my-module terraform/modules/generic ./my-module
```

For full documentation, visit [structkit.app/docs](https://structkit.app/docs/).

## Updating the Formula

When a new version of StructKit is released on PyPI, update the formula:

### Steps to bump the version:

1. **Download and calculate the SHA256 hash for the new version:**

   ```bash
   VERSION=3.2.2  # Replace with the new version
   curl -sL "https://files.pythonhosted.org/packages/source/s/structkit/structkit-${VERSION}.tar.gz" \
     --output "/tmp/structkit-${VERSION}.tar.gz"
   sha256sum "/tmp/structkit-${VERSION}.tar.gz"
   ```

2. **Update `Formula/structkit.rb`:**

   - Update the `url` line with the new version number
   - Update the `sha256` value with the hash from step 1
   - Check if any new dependencies were added by extracting the tarball and reviewing `pyproject.toml`

3. **Test the formula locally:**

   ```bash
   brew uninstall structkit  # If previously installed
   brew install --build-from-source Formula/structkit.rb
   structkit --version
   ```

4. **Commit and push the changes:**

   ```bash
   git add Formula/structkit.rb
   git commit -m "Bump structkit to version ${VERSION}"
   git push origin main
   ```

### Checking for new dependencies:

```bash
VERSION=3.2.2  # Replace with the new version
cd /tmp
tar -xzf "structkit-${VERSION}.tar.gz"
cat "structkit-${VERSION}/pyproject.toml" | grep -A 20 "dependencies ="
```

If new dependencies are added, you'll need to:

1. Download each dependency from PyPI
2. Calculate its SHA256 hash
3. Add a new `resource` block in `Formula/structkit.rb`

Example resource block:

```ruby
resource "package-name" do
  url "https://files.pythonhosted.org/packages/.../package-name-X.Y.Z.tar.gz"
  sha256 "abcdef123456..."
end
```

## License

This tap is maintained following Homebrew conventions. StructKit itself is licensed under Apache-2.0.
