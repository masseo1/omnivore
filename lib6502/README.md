# lib6502 — 6502 CPU emulator for Omnivore

C emulator engine wrapping [David Buchanan's 6502-emu](https://github.com/david-buchanan/6502-emu)
core with Apple II video emulation, debugger support, and Cython bindings
for Python. Powers the `Generic6502` and `Crabapple` emulator classes in
[Omnivore](https://github.com/robmcmullen/omnivore).

## Architecture

```mermaid
%%{
  init: {
    'theme': 'base',
    'themeVariables': {
      'primaryColor': '#1a1a2e',
      'primaryTextColor': '#e0e0e0',
      'primaryBorderColor': '#4a4a6a',
      'lineColor': '#8888cc',
      'secondaryColor': '#16213e',
      'tertiaryColor': '#0f3460'
    }
  }
}%%

flowchart TB
    subgraph Python["<b>Python Layer</b>"]
        direction LR
        GE["<b>Generic6502</b><br/><i>omnivore/emulators/generic6502/</i>"]:::py
        CA["<b>Crabapple</b><br/><i>omnivore/emulators/apple2/</i>"]:::py
    end

    subgraph Cython["<b>Cython Bridge</b>"]
        PYX["<b>lib6502.pyx</b><br/>def next_frame() → C"]:::cy
    end

    subgraph Glue["<b>Glue Layer</b>"]
        WRAP["<b>6502-emu_wrapper.c</b><br/>Frame loop · Save/restore ·<br/>Breakpoint check · Input"]:::glue
    end

    subgraph Extras["<b>Extensions</b>"]
        CRAB["<b>libcrabapple.c</b><br/>Apple II video emulation<br/>Hi-res/Lo-res/Text modes<br/>Soft-switches $C050-$C057"]:::ext
        DBG["<b>libdebugger</b><br/>Breakpoints · Stepping"]:::ext
        UDIS["<b>libudis/history.c</b><br/>Instruction history"]:::ext
    end

    subgraph Core["<b>CPU Core</b>"]
        CPU["<b>6502-emu/6502.c</b><br/>56 opcodes · Lookup-table decode<br/>Decimal mode · init_tables()"]:::cpu
    end

    GE -->|imports| PYX
    CA -->|imports| PYX
    PYX -->|calls| WRAP
    WRAP -->|includes| CPU
    WRAP -->|calls| CRAB
    WRAP -->|calls| DBG
    WRAP -->|calls| UDIS

    classDef py fill:#2d5a27,stroke:#4caf50,stroke-width:2px,color:#e0e0e0
    classDef cy fill:#5c2a7a,stroke:#9c27b0,stroke-width:2px,color:#e0e0e0
    classDef glue fill:#7a5a1a,stroke:#ffc107,stroke-width:2px,color:#e0e0e0
    classDef ext fill:#1a4a6a,stroke:#2196f3,stroke-width:2px,color:#e0e0e0
    classDef cpu fill:#6a1a1a,stroke:#f44336,stroke-width:2px,color:#e0e0e0
```

## Components

| Component | Description |
|-----------|-------------|
| **6502-emu/6502.c** | 6502 CPU core — 56 NMOS opcodes, lookup-table decode, decimal mode |
| **6502-emu_wrapper.c** | Frame loop, save/restore, breakpoint integration, keyboard input |
| **libcrabapple.c** | Apple II video — hi-res, lo-res, text modes, soft-switches |
| **libdebugger** | Breakpoint and single-stepping support |
| **libudis/history.c** | Instruction execution history recording |
| **lib6502.pyx** | Cython bindings — zero-copy NumPy buffer interface |

## Data flow (one frame)

```
Python: next_frame(input, output, breakpoints, history)
  ↓
wrapper: lib6502_next_frame() — copies keyboard → $C000
  ↓
debugger: libdebugger_calc_frame() → calls step repeatedly
  ↓
wrapper: lib6502_step_cpu() — decode opcode, execute, check breakpoints
  ↓
CPU: 6502.c instruction handler — modifies A,X,Y,PC,SP,SR,memory
  ↓
video: liba2_copy_video() — translate $2000/$4000 → 40×192 framebuffer
  ↓
Python: output['video'] rendered by Omnivore
```

## License

- `lib6502.pyx`, `6502-emu_wrapper.c`, `libcrabapple.c`: MPL 2.0
- `6502-emu/`: MIT — © 2017 David Buchanan

## Author

Rob McMullen — <feedback@playermissile.com>
