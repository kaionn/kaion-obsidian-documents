# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is an Obsidian vault for managing knowledge accumulation and documentation in Japanese. It is NOT a code project but a documentation repository managed with Obsidian, a markdown-based note-taking application.

## Directory Structure & Workflow

The vault follows a workflow-based organization system:

- `00_inbox/` - Intake for newly added notes and unorganized information. Start here for new content.
- `01_sources/` - Reference materials and source information
- `02_working/` - Documents and projects currently in progress
- `03_output/` - Completed documents and deliverables
- `Clippings/` - Web clippings and saved information from external sources

**Workflow**: New information → `00_inbox/` → Organize/classify → Move to appropriate directory → Work in `02_working/` → Complete in `03_output/`

## Important Notes

- **Language**: Primary language is Japanese. Content, filenames, and documentation should be in Japanese unless specified otherwise.
- **File Format**: All notes are markdown (`.md`) files that Obsidian uses for linking and organization.
- **Links**: Obsidian uses `[[wiki-style links]]` for internal linking between notes. Preserve this format when editing.
- **Version Control**: The `.obsidian/` directory contains Obsidian-specific configuration and is excluded from version control via `.gitignore`.
- **No Build Process**: This is a documentation repository - there are no build, test, or compile commands.

## Working with Files

When creating or moving notes:
1. Use descriptive Japanese filenames that reflect the content
2. Place new notes in `00_inbox/` unless there's a clear destination
3. Maintain the workflow by moving organized notes to the appropriate directory
4. Use `.gitkeep` files to preserve empty directories in Git

## Reference

For context on the intended use of this vault, see: [Obsidian と Claude Code を使ったドキュメント活用 - Zenn](https://zenn.dev/oikon/articles/obsidian-claude-code)
