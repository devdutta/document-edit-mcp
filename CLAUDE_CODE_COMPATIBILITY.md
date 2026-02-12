# Claude Code CLI Compatibility Note

## Status: Not Compatible

This FastMCP-based document operations server is **not compatible with Claude Code CLI** despite multiple troubleshooting attempts.

## What Was Tried

1. **Manual Configuration** - Added server config to `.mcp.json` manually
2. **FastMCP Version Fix** - Fixed `FastMCP()` initialization for MCP SDK 1.16.0 → 1.26.0
3. **Server Name Fix** - Changed from "Document Operations" to "document-operations" (no spaces)
4. **Proper Installation** - Used `fastmcp install claude-code` command (reported success)
5. **Multiple Restarts** - Restarted Claude Code CLI multiple times

## Results

- ✓ Server starts successfully (confirmed in logs)
- ✓ FastMCP installation succeeds
- ✓ Configuration appears correct
- ✗ **Claude Code CLI does not detect/load the server**
- ✓ Other MCP servers (n8n, playwright, github, notion, context7) work fine

## Root Cause

**FastMCP framework incompatibility with Claude Code CLI.** Claude Code CLI appears to only recognize certain MCP server implementations, and FastMCP-based servers are not among them (as of Feb 2026).

## Working Solution: python-docx Direct

Instead of using this MCP server, we use **python-docx** directly via Python scripts:

### Location
`D:\Claude_Code_Test\PolicyIntel_Platform\scripts\edit_architecture_guide.py`

### Example Usage
```python
from docx import Document

doc = Document(r"path\to\document.docx")
# Add heading
doc.add_heading("New Section", level=1)
# Add paragraph
doc.add_paragraph("Content here")
# Save
doc.save(r"path\to\document.docx")
```

### Advantages
- ✓ Direct control over document structure
- ✓ No MCP server dependencies
- ✓ Works reliably on Windows
- ✓ Full python-docx library features available
- ✓ Can be version controlled and tested

## Files Modified (Kept for Reference)

- `claude_document_mcp/server.py` - Fixed FastMCP initialization
- This repository contains working document operation tools, just not loadable in Claude Code CLI

## Recommendation

For DOCX editing in this workspace, continue using the python-docx script approach demonstrated in:
- `D:\Claude_Code_Test\PolicyIntel_Platform\scripts\edit_architecture_guide.py`

---

**Last Updated:** 2026-02-12
**Conclusion:** FastMCP servers are not compatible with Claude Code CLI. Use python-docx directly.
