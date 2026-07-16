# Security Policy

## Supported Versions

This is a documentation/plugin repository (Markdown + JSON, no runtime
service). The latest version on the `main` branch is the only one supported.

## Reporting a Vulnerability

Please report security issues privately rather than opening a public issue:

- Use [GitHub Private Vulnerability Reporting](https://github.com/brianschroeder/simple-agent-memory/security/advisories/new)
  for this repo, or
- Open a [draft security advisory](https://github.com/brianschroeder/simple-agent-memory/security/advisories)

You should receive an initial response within a few days. Please include:
- A description of the issue and its potential impact
- Steps to reproduce, if applicable
- Any suggested remediation

## Scope

Given this repo ships Markdown templates and a Claude Code plugin manifest
(no executable code, no dependencies, no network calls), realistic concerns
are limited to things like: malicious content smuggled into a template,
manifest fields that could be abused by tooling that consumes them, or
supply-chain issues in the GitHub Actions workflow (`.github/workflows/validate.yml`).
Report anything in that spirit even if it doesn't fit a traditional CVE.
