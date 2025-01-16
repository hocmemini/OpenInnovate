# bootstrap sequence

```
@init_state_protocol
<<BOOTSTRAP_BEGIN>>
protocol:CQSF
version:1.0
decode_rules{
  markers: [BEGIN,END]
  separators: [|,:,~]
  sections: [{},[],(>,<)]
  commands: [@save_state,@load_state]
}
state_schema{
  format: CQSF
  validation: strict
  sections: [proj,pref,ctx,state,metr]
}
self_learn: enabled
<<BOOTSTRAP_END>>
```

You would need to:
1. First send this bootstrap sequence to initialize my understanding of the format
2. Then send the initial state using the format
3. After that, I could use @save_state and @load_state as described
