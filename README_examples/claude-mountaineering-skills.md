# Mountaineering Skills for Claude Code - README Overview

This repository provides an automated mountain route research tool integrated with Claude Code. Here are the key points:

## Core Purpose
The plugin transforms manual mountaineering research (typically 3-5 hours) into an automated 3-5 minute workflow by aggregating "data from 10+ specialized mountaineering sources to generate detailed Markdown route beta reports."

## Main Features
- **Multi-source research** pulling from PeakBagger, SummitPost, WTA, AllTrails, and The Mountaineers
- **Current conditions data** including weather forecasts, avalanche reports, and daylight calculations
- **Automated quality validation** with systematic accuracy reviews
- **Graceful degradation** that continues functioning even when some data sources fail

## How It Works
The skill follows a seven-phase workflow starting with peak identification, moving through parallel data gathering, route analysis, report generation, and quality review before delivering a formatted Markdown file.

## Installation
Users install via Claude Code's plugin system using marketplace commands, with automatic Python dependency installation if `uv` is available.

## Scope & Limitations
The tool works best for "well-documented peaks in North America," with output quality depending on source availability. Coverage includes popular hiking destinations and climbing peaks across the continent.

## Recent Development
Latest versions (v3.4-3.5, November 2025) added automated report review and changelog synchronization, expanding from Pacific Northwest focus to broader North American coverage.

---
Source: https://github.com/dreamiurg/claude-mountaineering-skills
