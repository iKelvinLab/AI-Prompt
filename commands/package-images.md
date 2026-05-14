---
description: Maintain or run Docker buildx bake packaging for backend/frontend images
---

# Package Images

Use this command when the user asks to create, update, verify, or run the Docker image packaging workflow.

## Project Root

First locate the project root from the current directory:

1. Prefer `git rev-parse --show-toplevel`.
2. Otherwise walk upward until finding `AGENTS.md`, `Makefile`, `docker-bake.hcl`, or `docker-compose.yml`.
3. Read local instructions such as `AGENTS.md`, `CLAUDE.md`, `.opencode/opencode.json`, or `.codex/*` before editing.
4. Run all file reads, edits, and verification from the detected root.

## Behavior

Maintain root-level `Makefile` and `docker-bake.hcl` for Docker buildx bake packaging.

Required image targets:

- `backend`: context `./backend`, dockerfile `Dockerfile`
- `frontend`: context `./frontend`, dockerfile `Dockerfile`
- `release`: group containing both targets

Default platforms:

```hcl
["linux/amd64", "linux/arm64"]
```

## Version Rule

The default `VERSION` in `Makefile` must be:

```make
VERSION ?= $(shell git describe --tags --exact-match 2>/dev/null || git rev-parse --short=8 HEAD)
```

This means:

- If `HEAD` exactly matches a Git tag, use that tag.
- Otherwise use the first 8 characters of the current commit hash.
- Keep manual override support: `make package VERSION=1.2.3`.

## Image Tags

Do not add an extra `v` before the version.

Use:

```text
$(REGISTRY):back-$(VERSION)
$(REGISTRY):front-$(VERSION)
```

## Make Targets

Use these targets:

```make
package:
	docker buildx bake --file $(BAKE_FILE) $(BAKE_TARGET) --push

push: package

local:
	docker buildx bake --file $(BAKE_FILE) $(BAKE_TARGET) \
		--set "*.platform=$(LOCAL_PLATFORM)" \
		--load

backend:
	docker buildx bake --file $(BAKE_FILE) backend --push

frontend:
	docker buildx bake --file $(BAKE_FILE) frontend --push

print:
	docker buildx bake --file $(BAKE_FILE) $(BAKE_TARGET) --push --print
```

Important: never use `package push:` as a multi-target rule. It makes `make package push` build twice. `push` must be an alias dependency on `package`.

## Final Output

After successful `package`, print the complete image addresses with ANSI color highlighting. Highlight both the title and image addresses.

Recommended variables:

```make
COLOR_RESET := \033[0m
COLOR_IMAGE := \033[1;36m
```

Recommended output:

```make
	@printf "$(COLOR_IMAGE)Images:$(COLOR_RESET)\n"
	@printf "  $(COLOR_IMAGE)$(REGISTRY):back-$(VERSION)$(COLOR_RESET)\n"
	@printf "  $(COLOR_IMAGE)$(REGISTRY):front-$(VERSION)$(COLOR_RESET)\n"
```

For `backend` and `frontend`, print `Image:` and the single full image address with the same color.

## Modes

Interpret `$ARGUMENTS` as follows:

- Empty or `apply`: create or update packaging files, then verify with dry-run commands.
- `verify`: do not edit unless needed to make verification meaningful; run dry-run checks and report gaps.
- `run` or `push`: run the real packaging command after confirming the user intended a real build and registry push.

## Verification

Before claiming completion, run dry-run verification:

```bash
make -n push VERSION=d7ea54bf
make -n package push VERSION=d7ea54bf
make -n backend VERSION=d7ea54bf
make -n frontend VERSION=d7ea54bf
make print VERSION=d7ea54bf
```

Confirm:

- `make push` uses `docker buildx bake ... --push`.
- `make package push` shows only one `docker buildx bake`.
- Image tags are `back-<VERSION>` and `front-<VERSION>`, without `back-v` or `front-v`.
- `Images:` or `Image:` and image addresses use ANSI color codes.
- `make print` shows push output in the resolved bake config.
