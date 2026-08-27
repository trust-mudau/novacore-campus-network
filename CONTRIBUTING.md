# Contributing to NovaCore Campus Network

Thanks for helping improve the lab. Keep each contribution focused so configuration changes are easy to review and verify.

## Before making a change

1. Open an issue describing the current behavior and expected result.
2. Create a short branch from `main`, such as `fix/guest-acl` or `docs/dhcp-setup`.
3. Update the configuration and the related documentation together.

## Configuration standards

- Preserve the addressing plan unless the change explicitly redesigns it.
- Keep interface descriptions and device hostnames consistent with the topology.
- Use named ACLs and include a short `remark` for each policy block.
- Never commit real passwords, API keys, private IP details from a production network, or personal information.
- Treat credentials already present in the project as classroom placeholders only.

## Verification evidence

Run the relevant items in `TEST_PLAN.md`. In the pull request, include:

- The tests performed and their results
- Relevant `show` command output or Packet Tracer screenshots
- Any expected connectivity change
- Whether the addressing plan or topology diagram changed

## Pull requests

Use a descriptive title and explain why the change is needed. A reviewer should be able to connect each modified configuration line to an expected test result.
