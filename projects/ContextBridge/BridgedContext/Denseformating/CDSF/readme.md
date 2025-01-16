# RFC: Claude Dense State Format (CDSF)

## Abstract
This RFC proposes a dense state format designed specifically for Claude's internal state representation and cross-conversation persistence. The format combines semantic compression with symbolic markers optimized for Claude's language processing capabilities.

## Format Specification

### Bootstrap Sequence
```
@init_cdsf
<<CDSF/BOOT>>
λ:1.0|τ:rfc-001|∆:boot
self{learn:1|mode:λ}
parse[sym:∆|mark:λ|sep:~]
validate(strict:1|recover:0)
<<CDSF/BOOT>>
```

### Core Symbols
- λ: Version/protocol marker  
- τ: Session identifier
- ∆: State boundary
- ∴: Implication chain
- ⊕: State addition
- ⊗: State multiplication
- ∅: Null state

### Basic Format
```
<<CDSF>>
λ:1.0~τ:session-id
∆{
  proj:name~ver:1.0,
  pref[art:1|style:tech],
  ctx<focus:dev~tasks:3>,
  metr(eff:0.9|opt:0.8)
}
<<CDSF>>
```

### Example Usage
1. Initialize protocol:
```
@init_cdsf
<<CDSF/BOOT>>
λ:1.0|τ:init|∆:boot
<<CDSF/BOOT>>
```

2. Save state:
```
@save_cdsf
<<CDSF>>
λ:1.0~τ:abc123
∆{proj:myproj~ver:1.0,pref[art:1|style:tech]}
<<CDSF>>
```

3. Load state:
```
@load_cdsf
<<CDSF>>
[previous state content]
<<CDSF>>
```

# CDSF Project README

## Overview
CDSF (Claude Dense State Format) is a specialized format for encoding Claude's internal state in a compressed yet self-recoverable format. It uses semantic compression and symbolic markers to achieve high compression ratios while maintaining full state recovery capabilities.

## Key Features
- Self-bootstrapping protocol
- Semantic compression
- Human-readable core markers
- Built-in validation
- State recovery mechanisms

## Usage
1. Initialize the protocol using @init_cdsf
2. Save states using @save_cdsf
3. Load states using @load_cdsf
4. Verify states using @verify_cdsf

## Examples
See RFC sections for detailed examples.

## Implementation Notes
- Protocol is self-teaching through bootstrap sequence
- Core symbols chosen for semantic significance
- Format supports nested state hierarchies
- Validation ensures state consistency
- Recovery mechanisms handle partial states

## Best Practices
1. Always initialize protocol first
2. Include version markers
3. Validate saved states
4. Use complete marker blocks
5. Don't modify state format manually

## Format Benefits
- High compression ratio
- Self-describing
- Copy/paste safe
- Cross-platform compatible
- Human-inspectable
