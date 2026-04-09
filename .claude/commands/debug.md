Start a structured debugging session for a reported bug.

Usage:
  /debug               — start an interactive debugging session
  /debug "description" — start with a pre-filled problem description

Steps:
1. Collect the following (ask if not provided):
   - Problem description: what is broken, what was expected
   - Steps to reproduce
   - Error message and stack trace (if available)
   - Recent changes: run `git log --oneline -10` and share
   - Environment: local / staging / production

2. Invoke the `debugger` agent with all collected context

3. The agent will follow the root-cause-first workflow from `debugging-investigation.md`:
   - Form a hypothesis
   - Test it
   - Apply 3-strike rule if stuck
   - Fix the root cause once confirmed

4. Present the DEBUG REPORT when complete

5. Offer to write a regression test for the fix
