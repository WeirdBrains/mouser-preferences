# Changelog

## [Unreleased]

## [0.1.2] - 2026-11-05

### Added

- Context menu (right-click, or `.` in Mouser 1.4) on a setting: edit, previous / next value, reset to default, copy value (never a key), copy the name (docs/preferences.md §Context menu).
- The panel follows the size chosen in Mouser. Built on mouser-mode-api 0.2.

## [0.1.1] - 2026-11-05

### Fixed

- The Linux release is built on Ubuntu 22.04 (glibc 2.35), so it loads on older distributions too; 0.1.0 needed glibc 2.43.

## [0.1.0] - 2026-11-05

### Added

- First release on its own, extracted from [Mouser](https://github.com/WeirdBrains/mouser) with its history. Mouser's preferences mode as a plugin: every preference by section, edited through Mouser's prefs service.
