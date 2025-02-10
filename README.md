# Puppeteer MCP Server

This MCP server provides browser automation capabilities through Puppeteer, allowing interaction with both new browser instances and existing Chrome windows.

<a href="https://glama.ai/mcp/servers/lpt1tvbubf"><img width="380" height="200" src="https://glama.ai/mcp/servers/lpt1tvbubf/badge" alt="Puppeteer Server MCP server" /></a>

## Features

- Navigate web pages
- Take screenshots
- Click elements
- Fill forms
- Select options
- Hover elements
- Execute JavaScript
- Smart Chrome tab management:
  - Connect to active Chrome tabs
  - Preserve existing Chrome instances
  - Intelligent connection handling

## Project Structure

```
/
├── src/
│   ├── config/        # Configuration modules
│   ├── tools/         # Tool definitions and handlers
│   ├── browser/       # Browser connection management
│   ├── types/         # TypeScript type definitions
│   ├── resources/     # Resource handlers
│   └── server.ts      # Server initialization
├── index.ts          # Entry point
└── README.md        # Documentation
```

## Usage

### Standard Mode

The server will launch a new browser instance by default.

### Active Tab Mode

To connect to an existing Chrome window:

1. Close any existing Chrome instances completely

2. Launch Chrome with remote debugging enabled:
   ```bash
   # Windows
   "C:\Program Files\Google\Chrome\Application\chrome.exe" --remote-debugging-port=9222

   # macOS
   /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --remote-debugging-port=9222

   # Linux
   google-chrome --remote-debugging-port=9222
   ```

3. Navigate to your desired webpage in Chrome

4. Connect using the `puppeteer_connect_active_tab` tool:
   ```json
   {
     "targetUrl": "https://example.com", // Optional: specific tab URL
     "debugPort": 9222 // Optional: defaults to 9222
   }
   ```

The server will:
- Detect and connect to the Chrome instance running with remote debugging enabled
- Preserve your Chrome instance (won't close it)
- Find and connect to non-extension tabs
- Provide clear error messages if connection fails

## Available Tools

### puppeteer_connect_active_tab
Connect to an existing Chrome instance with remote debugging enabled.
- Optional:
  - `targetUrl` - URL of the specific tab to connect to
  - `debugPort` - Chrome debugging port (default: 9222)

### puppeteer_navigate
Navigate to a URL.
- Required: `url` - The URL to navigate to

### puppeteer_screenshot
Take a screenshot of the current page or a specific element.
- Required: `name` - Name for the screenshot
- Optional:
  - `selector` - CSS selector for element to screenshot
  - `width` - Width in pixels (default: 800)
  - `height` - Height in pixels (default: 600)

### puppeteer_click
Click an element on the page.
- Required: `selector` - CSS selector for element to click

### puppeteer_fill
Fill out an input field.
- Required:
  - `selector` - CSS selector for input field
  - `value` - Text to enter

### puppeteer_select
Use dropdown menus.
- Required:
  - `selector` - CSS selector for select element
  - `value` - Option value to select

### puppeteer_hover
Hover over elements.
- Required: `selector` - CSS selector for element to hover

### puppeteer_evaluate
Execute JavaScript in the browser console.
- Required: `script` - JavaScript code to execute

## Security Considerations

When using remote debugging:
- Only enable on trusted networks
- Use a unique debugging port
- Close debugging port when not in use
- Never expose debugging port to public networks

## Logging and Debugging

### File-based Logging
The server implements comprehensive logging using Winston:

- Location: `logs/` directory
- File Pattern: `mcp-puppeteer-YYYY-MM-DD.log`
- Log Rotation:
  - Daily rotation
  - Maximum size: 20MB per file
  - Retention: 14 days
  - Automatic compression of old logs

### Log Levels
- DEBUG: Detailed debugging information
- INFO: General operational information
- WARN: Warning messages
- ERROR: Error events and exceptions

### Logged Information
- Server startup/shutdown events
- Browser operations (launch, connect, close)
- Navigation attempts and results
- Tool executions and outcomes
- Error details with stack traces
- Browser console output
- Resource usage (screenshots, console logs)

## Error Handling

The server provides detailed error messages for:
- Connection failures
- Missing elements
- Invalid selectors
- JavaScript execution errors
- Screenshot failures

Each tool call returns:
- Success/failure status
- Detailed error message if failed
- Operation result data if successful

All errors are also logged to the log files with:
- Timestamp
- Error message
- Stack trace (when available)
- Context information
