# Claude Skills for Structural Engineering

Professional AI-assisted structural design calculations following international standards. These skills enable Claude to perform complex engineering calculations with proper code implementation and design verification.

## Available Skills

### 1. Foundation Design (`eurocode7-foundation`)
**Standard:** EN 1997-1:2004 (Eurocode 7)

Design shallow foundations with complete bearing capacity, settlement, and structural calculations.

**Capabilities:**
- Pad, strip, and raft foundations
- All Design Approaches (DA1, DA2, DA3)
- Bearing capacity verification
- Settlement analysis
- Sliding resistance
- Reinforcement design

### 2. Steel Design (`aisc360-steel`)
**Standard:** AISC 360-22 (16th Edition)

Comprehensive steel member design with Python calculation implementations.

**Capabilities:**
- Tension, compression, and flexural members
- Shear and combined loading
- Beam-columns with interaction equations
- Bolted and welded connections
- Both LRFD and ASD methods
- Complete Python code generation

## Installation

### Option 1: Direct Copy (Recommended)

1. **Locate Claude's skills directory:**
   - macOS/Linux: `~/.claude/skills/`
   - Windows: `%USERPROFILE%\.claude\skills\`

2. **Copy skills:**
   ```bash
   mkdir -p ~/.claude/skills/
   cp -r eurocode7-foundation ~/.claude/skills/
   cp -r aisc360-steel ~/.claude/skills/
   ```

3. **Restart Claude Desktop** or reload Claude Code

### Option 2: ZIP Upload to Claude Desktop

1. **Create ZIP files for each skill:**
   ```bash
   cd /path/to/claude_skill
   zip -r eurocode7-foundation.zip eurocode7-foundation/
   zip -r aisc360-steel.zip aisc360-steel/
   ```

2. **Upload to Claude Desktop:**
   - Open Claude Desktop
   - Go to Settings > Custom Skills
   - Click "Upload Skill"
   - Select the ZIP file
   - Restart Claude Desktop

## Usage

### Activating Skills

**In Claude Desktop or Claude Code:**
```
@eurocode7-foundation Design a 2m x 2m pad foundation for 500kN dead load
and 200kN live load. Soil: φ'=30°, c'=5kPa, γ=18kN/m³. Use DA1 (UK).
```

```
@aisc360-steel Design a W310x52 beam-column for Pu=500kN and Mu=150kN·m.
Length 4m, lateral bracing at 2m. A992 steel, LRFD method.
```

### What You Get

**Foundation Design:**
- Complete design calculations
- ULS and SLS verification
- Reinforcement schedule
- Design summary with utilization ratios
- Professional calculation sheets

**Steel Design:**
- Python code for all calculations
- Member capacity verification
- Interaction checks
- Connection design
- Step-by-step analysis
- Utilization ratios

## Examples

### Foundation Design Example
```
Design a square pad foundation:
- Column: 300x300mm
- Loads: Gk=500kN, Qk=200kN
- Soil: φ'=30°, c'=5kPa, γ=18kN/m³, E'=25MPa
- Design Approach: DA1 (UK)
- Settlement limit: 25mm
```

### Steel Design Example
```
Design a tension member:
- Member: L100x100x10 angle
- Load: Pu=150kN (LRFD)
- Connection: 2 bolts, U=0.80
- Steel: A36 (Fy=250MPa, Fu=400MPa)
```

## Features

### Professional Output
- Clear calculation procedures
- Code-compliant design
- Quality assurance checks
- Design assumptions documented

### Comprehensive Analysis
- Multiple limit states
- Proper safety factors
- Effective dimensions
- Load combinations

### Code Implementation
- Python functions ready to use
- Complete with comments
- Unit conversions handled
- Verification examples

## Standards Compliance

| Skill | Standard | Edition | Region |
|-------|----------|---------|--------|
| eurocode7-foundation | EN 1997-1 | 2004 | Europe |
| aisc360-steel | AISC 360 | 16th (2022) | USA |

## Skill Structure

```
eurocode7-foundation/
├── SKILL.md              # Main skill prompt
└── references/
    ├── FACTORS.md        # Design factors
    ├── STRUCTURAL.md     # Structural design
    └── EXAMPLES.md       # Worked examples

aisc360-steel/
├── SKILL.md              # Complete design guide with Python
├── README.md             # Skill documentation
└── skill.yaml            # Skill configuration
```

## Requirements

- Claude Desktop app or Claude Code CLI
- Basic understanding of structural engineering
- For Python code: Python 3.7+ with numpy

## Limitations

**Foundation Design:**
- Shallow foundations only (no piles)
- Static loading (no seismic)
- Requires soil investigation data

**Steel Design:**
- Standard sections (W-shapes, angles, HSS)
- No plate girders or complex sections
- Assumes compact sections for flexure

## Support

For issues or questions:
- GitHub: https://github.com/Sompote/claude_skill_CE
- Review calculations independently
- Verify with licensed engineer before construction

## License

MIT License - Free to use and modify

## Disclaimer

**IMPORTANT:** These skills are professional tools to assist qualified engineers. All designs must be reviewed and sealed by a licensed professional engineer before implementation. The author assumes no liability for designs produced using these skills.

Always follow local building codes and regulations.

---

**Author:** Sompote Youwai
**Version:** 1.0
**Last Updated:** October 2025
