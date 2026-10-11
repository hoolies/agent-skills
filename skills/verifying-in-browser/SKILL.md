---
name: verifying-in-browser
description: After a UI change, open the app in Cursor's built-in browser and verify rendering, console errors, and network health. Use the server that is already running, or the project's own start command. Do not assume npm or port 3000. Skip when the change has no page to open.
---

# Verify in Browser

Use this skill proactively after making code changes to verify the app actually works. Don't just trust that the code is correct — launch it and check.

## Steps

1. **Find the server.** Skip this skill when the change has no page to open (shell, library, config-only).

   - Read the terminals. If a dev server is already printing a localhost URL, use that URL.
   - Otherwise read the project, not a default. Check `package.json` scripts, `pyproject.toml`, `Makefile`, `docker-compose`, README, and nginx or similar config. Use the command that project documents.
   - If several commands could start it, ask which one. Do not run `npm run dev` unless that script exists.
   - Start it in the background and wait until it prints the URL and port. Use that URL. Do not assume port 3000.

2. **Open that URL** in the side browser and screenshot it. Navigate to `http://localhost:3000` only when that is the URL the server printed.

3. **Quick health check** — run these three checks:

   **Console errors:**
   ```
   Tool: browser_console_messages
   ```
   Flag any errors. Warnings about deprecations can be noted but aren't blockers.

   **Network failures:**
   ```
   Tool: browser_network_requests
   ```
   Flag any 4xx/5xx responses, failed fetches, or CORS errors.

   **Visual check:**
   Review the screenshot. Does the page render? Is the layout correct? Are there any blank screens or loading spinners that never resolve?

4. **Navigate to the changed page** — if your changes are on a specific route, navigate there and repeat the checks.

5. **Test interactions** — if you changed a form, button, or interactive element:
   - Take a `browser_snapshot` to get element refs
   - Click, fill, or hover on the changed elements
   - Take another screenshot to verify the result

6. **Report the verdict** — tell the user:
   - "Verified in browser — page renders correctly, no console errors, all network requests healthy."
   - Or: "Found issues: [list of problems]"

## When to Use This

- After changing any React component, CSS, or layout code
- After modifying API routes or data fetching
- After updating environment variables or configuration
- After installing or removing dependencies
- Before committing — as a final sanity check

## Notes

- Use `position: "side"` to see code and browser at the same time.
- If a server is already running, use its printed URL.
- For SSR apps, check both the initial server-rendered HTML and the client-hydrated state.
