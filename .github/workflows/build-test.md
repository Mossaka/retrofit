---
on:
  workflow_dispatch:

engine: copilot

network:
  allowed:
    - defaults
    - java
    - github

safe-outputs:
  create-issue:
    max: 1
---

# Build and Test

Explore this repository to understand its structure and purpose. Then build the project and run all tests. Finally, create an issue titled "Build and Test Report" with a detailed summary of:
- What the project is
- Build results (success/failure, any errors)
- Test results (number of tests passed/failed, any error details)
- Overall assessment of whether the build and tests succeeded
