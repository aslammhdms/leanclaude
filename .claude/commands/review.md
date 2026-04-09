Run a structured code review on the current changes or specified files.

Usage:
  /review              — review all uncommitted changes (git diff HEAD)
  /review path/to/file — review a specific file
  /review #123         — review a specific PR by number (requires gh CLI)

Steps:
1. If a PR number is given, fetch the diff with `gh pr diff <number>`
2. If a file path is given, read that file and compare to git HEAD
3. Otherwise, run `git diff HEAD` to get current changes
4. Invoke the `code-reviewer` agent with the diff as context
5. Present the full CODE REVIEW report to the user
6. Ask: "Would you like me to fix any of the blocking issues?"
