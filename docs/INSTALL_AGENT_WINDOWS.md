# Install CRATOR Local Agent (Windows)

The CRATOR Local Agent runs only on your PC and lets the browser UI control OMR processing. No sheet data leaves your computer.

## Requirements
- Windows 10/11
- No global Python required for packaged EXE
- Python environment only needed when running from source

## Option A: Use the Packaged EXE
1. Download the latest CRATOR package from Releases.
2. Extract the ZIP.
3. Run `CRATOR.exe`.
4. The browser opens at `http://127.0.0.1:5757`.
5. Verify the UI shows `Agent: Connected`.

The `CRATOR_Files` folder must stay next to `CRATOR.exe`. It contains the runner, UI, and templates used automatically by CRATOR.

## Option B: Run From Source
1. Install the project dependencies in the `omr` environment.
2. Build the web UI:
```bat
cd web
npm install
npm run build
cd ..
```
3. Start the agent:
```bat
python -m agent.run_server
```
4. Health check:
```bat
curl http://127.0.0.1:5757/health
```

## Troubleshooting
- Port in use: change `port` in `agent/config.py`.
- Firewall prompt: allow access for local connections.
- UI cannot connect: confirm the agent is running and the browser can access localhost.
