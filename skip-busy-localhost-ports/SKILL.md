---
name: anthropic-skills:skip-busy-localhost-ports
description: Use when starting, opening, testing, or mentioning a localhost dev server. Claude must check whether the default port is already in use and skip it if so. If port 3000 is busy, try 3007, then 3008, then 3009, and onward. Trigger this skill whenever the user mentions starting a dev server, running a local app, or opening a localhost URL — even if they don't say anything about ports explicitly.
---

# Skip Busy Localhost Ports

When you are about to start a dev server (e.g. `npm run dev`, `next dev`, `vite`, `python -m http.server`, etc.):

1. **Before launching**, check whether the default port is in use:
   ```bash
   lsof -ti :<port> 2>/dev/null | head -1
   ```
   If that returns output, the port is occupied.

2. **Port selection order**: Start at the framework's default port (commonly 3000). If busy, try in this sequence: 3007 → 3008 → 3009 → 3010 → 3011 → … (increment by 1). Skip 3001–3006 to avoid common collateral ports (e.g. React devtools, Storybook).

3. **Launch on the free port** by passing it explicitly:
   - Next.js: `next dev -p <port>` or `npm run dev -- -p <port>`
   - Vite: `vite --port <port>`
   - Create React App: `PORT=<port> npm start`
   - Python: `python -m http.server <port>`
   - Other: check the tool's `--port` or `-p` flag

4. **Tell the user** which port you picked and why (e.g. "Port 3000 was busy, starting on 3007").

5. **Do not ask** the user which port to use — just pick the next free one and proceed.

## Example check loop (bash)

```bash
for port in 3000 3007 3008 3009 3010 3011; do
  if ! lsof -ti :$port &>/dev/null; then
    echo $port
    break
  fi
done
```

Run this before launching, capture the output, and use that port.
