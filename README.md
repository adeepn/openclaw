# OpenClaw local patches

Changes that are open upstream and not merged yet, kept applicable to released
OpenClaw versions so a release build can carry them.

## telegram-dice

- `fix(telegram): deliver inbound dice rolls to the agent` — https://github.com/openclaw/openclaw/pull/137112
- `feat(telegram): add a dice action to the message tool` — https://github.com/openclaw/openclaw/pull/138202
- Feature request the second patch implements: https://github.com/openclaw/openclaw/issues/137110

Code and tests only. The documentation for both changes lives in the upstream pull
requests and not in this series: upstream reorganizes `docs/channels/telegram*`
between releases, and docs hunks are the only part of this work that conflicts when
the series moves to a new tag. Dropping them keeps the series applying cleanly.

## Layout

```
telegram-dice/<tag>/0001-*.patch
telegram-dice/<tag>/0002-*.patch
```

`<tag>` is the upstream release the series was rebased onto and validated against.

## Apply to a release checkout

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
git checkout <tag>
git am /path/to/telegram-dice/<tag>/*.patch
pnpm install
pnpm build
```

## Move the series to a newer release

The working branch is `patches/telegram-dice` in `adeepn/openclaw`; each published
series is also tagged `dice-patches/<tag>` so an older base stays reachable after the
branch moves.

```bash
git fetch upstream --tags
git checkout patches/telegram-dice
git rebase --onto <new-tag> <old-tag>
git tag dice-patches/<new-tag>
git format-patch <new-tag>..patches/telegram-dice -o telegram-dice/<new-tag>
```

Validate on the new base before publishing the artifacts: `pnpm install`, then
`pnpm check:changed --base <new-tag>` and the telegram, outbound, and agent-tool test
lanes.
