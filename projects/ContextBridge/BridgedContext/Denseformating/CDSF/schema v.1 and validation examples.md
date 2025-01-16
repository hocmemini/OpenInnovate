# CDSF Schema and Validation Examples

## Base Schema (Uncompressed JSON Reference)
```json
{
  "session_metadata": {
    "project": "test_project",
    "timestamp": 1705395200,
    "version": "1.0"
  },
  "preferences": {
    "artifacts": true,
    "style": "technical",
    "notifications": false
  },
  "context": {
    "id": "abc123",
    "topic": "main",
    "achievements": 2
  }
}
```

## CDSF Compressed Format
```
<<CDSF>>
λ:1.0~τ:1705395200
∆{proj:test_project~ver:1.0,pref[art:1|style:tech|not:0],ctx<id:abc123~top:main~ach:2>}
<<CDSF>>
```

Compression Analysis:
- Original size: 238 bytes
- Compressed size: 108 bytes
- Compression ratio: 54.6%

## Validation Rules

1. Structure Validation:
```
MUST have: <<CDSF>> markers
MUST contain: λ (version), τ (timestamp)
MUST use: ∆ for state block
MUST separate: ~ (major), | (minor), : (key-value)
```

2. Content Rules:
```
Version (λ): MUST match /^\d+\.\d+$/
Timestamp (τ): MUST be valid Unix timestamp
Project: MUST be non-empty string
Preferences: MUST contain [art,style,not]
Context: MUST have valid id
```

3. Symbol Requirements:
```
Major sections: {}, [], <>
Value separators: ~, |, :
State markers: ∆, λ, τ
Optional markers: ∴, ⊕, ⊗, ∅
```

## Examples with Validation

### Valid Basic State
```
<<CDSF>>
λ:1.0~τ:1705395200
∆{proj:test~ver:1.0}
<<CDSF>>
```
Validation: PASS
- Has required markers
- Contains version and timestamp
- Uses correct separators

### Invalid Format
```
<<CDSF>>
λ:1.0|τ:1705395200
∆[proj:test:ver:1.0]
<<CDSF>>
```
Validation: FAIL
- Uses | instead of ~ for major separation
- Incorrect section markers []
- Missing required elements

### Complex Valid State
```
<<CDSF>>
λ:1.0~τ:1705395200
∆{
  proj:test~ver:1.0,
  pref[art:1|style:tech|not:0],
  ctx<id:abc~top:main>,
  metr(eff:0.9|opt:0.8)
}
<<CDSF>>
```
Compression Analysis:
- Original equivalent: ~500 bytes
- Compressed size: 146 bytes
- Compression ratio: 70.8%

### Nested State Example
```
<<CDSF>>
λ:1.0~τ:1705395200
∆{
  proj:test~ver:1.0,
  pref[art:1|style:tech],
  ctx<
    main{id:abc~type:dev},
    sub[task:1|prog:0.5],
    refs<link:xyz|dep:3>
  >
}
<<CDSF>>
```
Compression Analysis:
- Original equivalent: ~800 bytes
- Compressed size: 189 bytes
- Compression ratio: 76.4%

## Validation Functions

Basic validator pseudocode:
```javascript
function validateCDSF(state) {
  // Check markers
  if (!state.startsWith('<<CDSF>>') || !state.endsWith('<<CDSF>>'))
    return false;
    
  // Check required elements
  const hasVersion = /λ:\d+\.\d+/.test(state);
  const hasTimestamp = /τ:\d{10}/.test(state);
  const hasStateBlock = /∆{.*}/.test(state);
  
  if (!hasVersion || !hasTimestamp || !hasStateBlock)
    return false;
    
  // Check structure
  const validSeparators = 
    state.match(/[~|:]/g).length === 
    (state.match(/[a-z]+/g).length - 1);
    
  const balancedBrackets =
    checkBracketBalance(state, '{[<(')
    
  return validSeparators && balancedBrackets;
}
```

## Compression Statistics

Average compression ratios by content type:
1. Basic metadata: 50-60%
2. Simple states: 60-70%
3. Complex nested states: 70-80%
4. Large hierarchical states: 75-85%

Factors affecting compression:
- Depth of nesting
- Repetition of keys
- Length of values
- Number of sections
