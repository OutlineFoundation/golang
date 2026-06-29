# golang.getoutline.org

Static HTML pages that serve [Go vanity import paths](https://pkg.go.dev/cmd/go#hdr-Remote_import_paths)
for Outline, hosted at https://golang.getoutline.org.

Each page carries `go-import` / `go-source` meta tags so that
`go get golang.getoutline.org/<name>` resolves to the real GitHub repository,
and redirects browsers to the corresponding [pkg.go.dev](https://pkg.go.dev)
page.

| Import path | Repository |
|-------------|------------|
| `golang.getoutline.org/sdk` | [OutlineFoundation/outline-sdk](https://github.com/OutlineFoundation/outline-sdk) |
| `golang.getoutline.org/tunnel-server` | [OutlineFoundation/tunnel-server](https://github.com/OutlineFoundation/tunnel-server) |

## Adding a vanity path

Create `<name>.html` at the repo root (and an `<name>/` directory with nested
pages if the module has subpackages), mirroring `sdk.html` / `sdk/x.html`. Point
the `go-import` meta tag at the GitHub repo and the `meta refresh` /
`go-source` tags at the matching `pkg.go.dev` URL.

## Deployment

The site is hosted on [Cloudflare Pages](https://pages.cloudflare.com/) with the
Git integration enabled. There is **no build step** — the repository is a set of
static HTML files served as-is:

- Cloudflare Pages build command: _none_
- Build output directory: `/` (repo root)
- Production branch: `main`

Merging to `main` auto-deploys to https://golang.getoutline.org, and pull
requests get preview deployments. Cloudflare Pages serves `<name>.html` for the
extensionless request path `/<name>`, which is what the Go toolchain requests.
