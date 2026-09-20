# How I used AI

I used an AI coding agent (opencode, Claude-powered) to drive the whole exercise —
not a single prompt, but a deliberate loop:

1. **Read the lab**: agent read the README, `docker-compose.yml`, `guards.py`,
   `halo.py`, `audit.py`, `config.py` and the participant guide to understand the
   M1 mechanism before touching anything.
2. **Environment**: I hit a docker-permission wall on this Linux box, so the agent
   planned a native fallback (uv + userspace Ollama + in-memory store) and picked a
   smaller model (llama3.2:3b) to fit 3.5 GB RAM.
3. **Attack iteration**: agent sent payloads to the live app, read the model's
   replies, and adapted phrasings (verbatim-repeat; fact-check trick for the
   stretch) until `/validate/m1` showed pass — the audit log was the acceptance
   criterion, not the model's words.
4. **Fix**: agent first verified the lab's own hardening (L2/secure mode), then
   spotted a real gap — the input filter's missing canonicalization — and wrote the
   one-line patch plus a regression test.
5. **Verification**: agent ran the full test suite (342 passed) and replayed the
   exact same attacks against the fixed app, capturing validation output.

Where the agent needed human judgment: choosing M1 as the layer (keyless-friendly),
deciding that "remove the secret from the prompt" was the real fix rather than
bolting on more filters, and framing what the fix does NOT cover.
