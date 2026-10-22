# MCP connector documentation

This public repository contains connector documentation and `server.json` registry
metadata. It does not host the running server implementation. Read [README.md](README.md)
and [SECURITY.md](SECURITY.md) before changes.

- Preserve existing Git changes and stage only your own files. Keep the human author.
- Describe capabilities and link to `https://legalize.dev/mcp` for current coverage,
  allowances and tools. Do not copy changing counts into descriptions.
- Preserve supported clients and their setup documentation. Claude references here
  describe a supported client; they are not legacy agent instructions to remove.
- Keep public documentation free of private service internals and credentials.
- Validate JSON with `python3 -m json.tool server.json` and check changed links.
  There is no local application build or server test suite here.
- Publishing registry metadata is a separate action; editing it does not publish it.
