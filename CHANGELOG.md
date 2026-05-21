# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2026-05-19

### Added

- Initial release of the Best Practices Marketplace framework — a de-branded,
  reusable fork of the SLIM marketplace website.
- Registry-driven build: `static/data/registry.json` is the hand-authored source
  of truth, and `.claude-plugin/marketplace.json` is generated from it by
  `src/conf/generate-marketplace.js`.
- Multi-marketplace support: federate any number of remote registries via
  `marketplaceConfig.registries`.
