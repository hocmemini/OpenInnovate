# Claude Quick State Format (CQSF)

## Format Overview
CQSF is a self-contained format for copying and pasting Claude states using plain text markers and base64-style encoding for compression.

## Usage Instructions

### To Save State:
1. Type in chat: `@save_state`
2. Claude will respond with:
```
<<CQSF_BEGIN>>
proj|test~1.0|dev:focus
pref{art:1|style:tech|notif:0}
ctx[id:123|topic:main|ach:2]
state<active:dev|tasks:3>
metr(eff:0.9|opt:0.8|spd:0.7)
<<CQSF_END>>
```
3. Copy everything between and including the markers

### To Load State:
1. Paste the entire block including markers
2. Type: `@load_state`
3. Claude will automatically recognize and load the state

## Format Structure

The format uses simple ASCII characters for maximum compatibility:
- `<<CQSF_BEGIN>>` and `<<CQSF_END>>` as boundary markers
- `|` for value separation
- `{}[]()<>` for section grouping
- `:` for key-value pairs
- `~` for version markers

## Example

Save current state:
```
@save_state
<<CQSF_BEGIN>>
proj|myproject~2.0|prod:active
pref{art:1|style:tech|notif:1}
ctx[id:xyz|topic:feature|ach:5]
state<active:dev|tasks:2>
metr(eff:0.95|opt:0.85|spd:0.9)
<<CQSF_END>>
```

Load a state:
```
<<CQSF_BEGIN>>
proj|myproject~2.0|prod:active
pref{art:1|style:tech|notif:1}
ctx[id:xyz|topic:feature|ach:5]
state<active:dev|tasks:2>
metr(eff:0.95|opt:0.85|spd:0.9)
<<CQSF_END>>
@load_state
```

## Quick Reference

1. Save: `@save_state`
2. Copy the entire output including markers
3. Paste wherever needed
4. Load: `@load_state`

## Format Benefits
- Direct copy/paste support
- No encoding/decoding needed
- Human-readable
- No special characters required
- Works across all platforms

Remember:
- Always include the marker lines when copying
- Don't modify the format manually
- Use `@save_state` and `@load_state` commands
- Copy and paste the entire block as is
