---
name: verify-third-party-sources
description: Verify third-party libraries, SDKs, CLIs, frameworks, services, protocols, and external software against available source code or version-matched official documentation before relying on API behavior. Use when a user asks an agent to write, edit, review, debug, upgrade, configure, or explain code that depends on packages, tools, APIs, plugins, schemas, command-line flags, configuration formats, or other non-project-owned software.
---

# Verify Third Party Sources

## Overview

Ground claims about third-party software in the version the project actually uses. Prefer local source, installed package files, type definitions, generated schemas, lockfiles, and official versioned documentation over memory.

## Workflow

1. Identify the dependency and version.
   - Inspect manifests and locks first: `package.json`, lockfiles, `pyproject.toml`, `requirements*.txt`, `Pipfile.lock`, `poetry.lock`, `Cargo.toml`, `Cargo.lock`, `go.mod`, `go.sum`, `Gemfile.lock`, `.csproj`, `packages.lock.json`, `composer.lock`, Dockerfiles, tool config, vendored code, or generated metadata.
   - Before checking globally installed packages or browsing the internet, look for active or project-local dependency environments such as `.venv`/`venv`, `node_modules`, workspace package stores, vendored dependency directories, language toolchains, module caches, containers, and SDK installations.
   - Prefer commands executed through the project-local environment's binary or package manager over global commands. Examples: `.venv/bin/python -m pip show`, `.venv/bin/python -c ...`, `./node_modules/.bin/<tool>`, package-manager workspace commands, container shell commands, or toolchain-specific project commands.
   - Do not treat "not installed globally" as evidence that the dependency is absent or undocumented. If a project-local environment exists or may exist, inspect it before going to internet documentation.
   - If runtime inspection is available and safe, use the project-local package manager or language tooling to confirm the installed version.
   - If the version cannot be determined, say so and avoid precise claims that depend on version-specific behavior.

2. Check local authoritative material.
   - Prefer source code already present in the workspace, local dependency environments, vendored dependencies, installed package files, type declarations, generated clients, schemas, `.d.ts` files, docstrings, tests, examples, changelogs, and release notes shipped with the package.
   - Use fast local search (`rg`, package-specific search, or language tooling) to locate symbols, options, defaults, overloads, error paths, and behavior.
   - Treat implementation code and tests as stronger evidence than README snippets when they disagree.

3. Check upstream source when local source is insufficient.
   - If the dependency is open source and network access is available, inspect the upstream repository at the tag, commit, or release matching the project's version.
   - Prefer primary sources: official repository, official docs, release notes, API reference, standards documents, or vendor-maintained migration guides.
   - Avoid relying on third-party blog posts, Stack Overflow, issue comments, or model memory unless primary sources are unavailable; label them as weaker evidence if used.

4. Check official documentation for the exact version.
   - Look for version selectors, release branches, tags, archived docs, compatibility matrices, and migration notes.
   - If only latest docs are available, verify whether the documented behavior existed in the project's version through changelogs, release notes, source tags, or package metadata.
   - For cloud services and APIs without pinned local versions, check current official docs and mention the retrieval date when behavior is likely to change.

5. Apply the evidence.
   - Implement against verified APIs, options, defaults, and constraints.
   - When changing code, keep the dependency version in mind; do not silently use APIs introduced after the pinned version.
   - When reviewing or explaining code, distinguish confirmed behavior from inference.

## Evidence Rules

- Prefer exact-version evidence over latest documentation.
- Prefer source code, tests, type declarations, and generated schemas over prose docs.
- Prefer official docs and repositories over unofficial sources.
- Prefer local project evidence over assumptions about the ecosystem.
- Prefer project-local environments over global installations; a missing global package is not evidence when a local environment, lockfile, workspace store, container, or vendored dependency may contain the package.
- Quote or cite links when browsing was used and the final answer depends on them.
- State uncertainty explicitly when exact-version evidence is unavailable.

## Internet Use

Use internet search or browsing when:

- the dependency source or docs are not present in the workspace, project-local dependency environment, vendored code, container, SDK installation, or accessible package cache;
- the user asks for latest/current behavior;
- the software, API, CLI, or service is likely to have changed;
- version compatibility, deprecation, licensing, security, or migration behavior matters;
- local evidence conflicts with remembered knowledge.

Before browsing because a package appears missing, verify that the lookup used the project-local environment rather than only the global interpreter, global CLI, or system package store.

When browsing, search official domains or repositories first. Use primary sources such as release tags, package registries, official docs, changelogs, RFCs, standards, and vendor migration guides.

## Reporting

Keep the final answer concise, but include enough provenance for the user to trust the work:

- name the package/tool and version checked when it affected the result;
- mention whether evidence came from local source, installed package files, upstream source, or official docs;
- include links for browsed sources;
- call out any unresolved version mismatch or unverified assumption.
