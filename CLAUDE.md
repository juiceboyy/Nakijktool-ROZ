# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Nakijktool ROZ** is a single-file HTML grading tool for Dutch law students (HBO Rechten). It assesses student responses to administrative law objections (*Reactie op Zienswijze*) concerning the Windpark Zuidervaart wind farm case. The entire application lives in one file: [nakijktool_zienswijze_v3.html](nakijktool_zienswijze_v3.html).

## Running the Application

Open `nakijktool_zienswijze_v3.html` directly in any modern browser — no server, build step, or dependencies required. For AI grading features, the Anthropic API key must be entered in the UI at runtime.

## Architecture

The application is a self-contained HTML file (~746 lines) with three sections:

1. **CSS** (lines 7–215): Styling with responsive grid layout (breakpoint at 700px).
2. **HTML** (lines 216–530): Two-tab UI:
   - **Brief tab** (45 pts): Six manually scored rubric categories (A–F) with numeric inputs and auto-validation.
   - **Argumentatiestructuur AI tab** (10 pts): AI-powered argument structure evaluation with manual override.
3. **JavaScript** (lines 531–746): Vanilla JS handling score calculation, grade conversion, tab switching, and Claude API calls.

### Key Logic

- **Score → Grade mapping**: `calculateGrade()` converts raw score (0–55) to Dutch 1–10 scale; ≥5.5 passes.
- **AI grading** (`runAINakijk()`): Sends student text to `claude-sonnet-4-20250514` via direct `fetch` to `https://api.anthropic.com/v1/messages`. Expects JSON response with scores and Dutch feedback.
- **System prompt** (lines 567–639): Contains the full legal assessment rubric. Update this if grading criteria change.
- Input validation prevents exceeding per-category maximums with real-time visual feedback.

## Making Changes

All edits go into the single HTML file. The three logical sections (CSS / HTML / JS) are clearly separated but not split into separate files. When modifying the AI assessment criteria, update both the rubric UI elements and the system prompt together to keep them in sync.
