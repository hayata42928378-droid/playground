# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

A single-page TODO app built with vanilla HTML/CSS/JavaScript — no build step, no package manager, no dependencies.

## Running

Open `index.html` directly in a browser, or serve it with any static file server (e.g. VS Code's Live Server extension). There is no build, lint, or test command in this repo.

## Architecture

Three files, no bundler:

- `index.html` — page structure and static shell (form, filter buttons, list container, footer).
- `style.css` — all styling.
- `script.js` — all app logic, in one flat scope with no modules.

State lives in a single `todos` array of `{ id, text, completed }` objects, persisted to `localStorage` under the key `todo-app-items` via `loadTodos()`/`saveTodos()`. Every mutation (`addTodo`, `toggleTodo`, `deleteTodo`, `clearCompleted`) follows the same pattern: mutate `todos`, call `saveTodos()`, call `render()`. `render()` fully rebuilds the `<ul>` from `todos` and the active `currentFilter` (`all` / `active` / `completed`) rather than patching individual DOM nodes — there is no virtual DOM or diffing.

`localStorage` is origin-scoped: data saved when opened via `file://` will not appear when the same app is served from `http://localhost:...` (Live Server), and vice versa.
