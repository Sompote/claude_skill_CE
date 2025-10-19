# Claude Skills for Civil Engineering Design

A collection of professional Claude AI skills for civil engineering calculations and design, currently featuring Eurocode 7 foundation design capabilities.

## Overview

This repository contains Claude AI skills that enable AI-assisted civil engineering design calculations following international standards. These skills provide detailed, step-by-step design procedures with proper verification methods and professional output formatting.

## Available Skills

### Eurocode 7 Foundation Design (`eurocode7-foundation`)

Professional shallow foundation design tool compliant with EN 1997-1:2004 (Eurocode 7). Supports all three Design Approaches (DA1, DA2, DA3) used across European countries.

**Capabilities:**
- Pad, strip, and raft foundation design
- Bearing capacity verification (GEO limit state)
- Settlement analysis (SLS)
- Sliding resistance checks
- Structural design with reinforcement calculations (STR limit state)
- Support for eccentric/inclined loading
- Layered soil conditions
- Multiple design approach combinations

**Standards Compliance:**
- EN 1997-1:2004 (Eurocode 7: Geotechnical Design)
- EN 1992-1-1:2004 (Eurocode 2: Structural Design)

## Installation & Setup

### Prerequisites

- Claude Desktop app or Claude Code CLI
- Basic understanding of geotechnical engineering principles

### Installation Steps

1. **Clone this repository:**
   ```bash
   git clone https://github.com/yourusername/claude_skill.git
   cd claude_skill
   ```

2. **Locate your Claude skills directory:**
   - **macOS/Linux:** `~/.claude/skills/`
   - **Windows:** `%USERPROFILE%\.claude\skills\`

3. **Copy the skill to Claude's skills directory:**
   ```bash
   # Create skills directory if it doesn't exist
   mkdir -p ~/.claude/skills/

   # Copy the eurocode7-foundation skill
   cp -r eurocode7-foundation ~/.claude/skills/
   ```

4. **Restart Claude Desktop** or reload your Claude Code session to load the new skill.

## Usage

### Activating a Skill

In Claude Desktop or Claude Code, you can activate a skill by mentioning it in your conversation:

```
@eurocode7-foundation Design a square pad foundation for a 300x300mm column
with dead load 500kN and live load 200kN. Soil has φ'=30°, c'=5kPa, γ=18kN/m³.
Use Design Approach 1 (UK). Allowable settlement is 25mm.
```

### Example Workflow

1. **Start a conversation** and reference the skill:
   ```
   @eurocode7-foundation I need to design a foundation
   ```

2. **Provide design parameters** when prompted:
   - Design Approach (DA1/DA2/DA3)
   - Soil parameters (φ', c', cu, γ, E')
   - Loading (Gk, Qk, Hk, Mk)
   - Foundation constraints (depth, settlement limits)

3. **Receive comprehensive design output** including:
   - Design data summary
   - ULS verification (bearing capacity, sliding)
   - SLS verification (settlement)
   - Structural design (reinforcement)
   - Summary table with utilization ratios
   - Design notes and assumptions

### Design Approaches

The skill supports all three Eurocode 7 Design Approaches:

- **DA1 (UK, Sweden):** Two combinations required (A1+M1+R1 and A2+M2+R1)
- **DA2 (Germany, Italy, Poland):** Single combination (A1+M1+R2)
- **DA3 (France, Belgium, Austria):** Single combination with geotechnical/structural distinction

## Skill Structure

```
eurocode7-foundation/
├── SKILL.md              # Main skill prompt and procedures
└── references/
    ├── FACTORS.md        # Shape, depth, and inclination factors
    ├── STRUCTURAL.md     # Reinforcement and structural design details
    └── EXAMPLES.md       # Worked examples for common cases
```

## Features

### Comprehensive Design Verification

- Multiple limit state checks (ULS and SLS)
- Proper application of partial safety factors
- Effective dimension calculations for eccentric loading
- Inclination factors for horizontal loads

### Professional Output

- Clearly formatted calculation sheets
- Step-by-step verification procedures
- Utilization ratios for all checks
- Construction notes and assumptions
- Design summary tables

### Quality Assurance

Built-in quality checks ensure:
- Correct design approach application
- Proper partial factor selection
- Eccentricity effects included
- Water table effects considered
- Both combinations verified (for DA1)

## Example Use Cases

1. **Residential Building Foundations**
   - Design pad foundations for columns
   - Combined footings
   - Strip foundations for load-bearing walls

2. **Industrial Structures**
   - Heavy machinery foundations
   - Crane bases with moment loading
   - Foundations on weak soils

3. **Infrastructure Projects**
   - Bridge abutments
   - Retaining wall foundations
   - Tower foundations

## Limitations

- Currently supports **shallow foundations only** (pad, strip, raft)
- Deep foundations (piles) not yet implemented
- Assumes static loading (no dynamic/seismic analysis)
- Foundation on rock requires engineering judgment

## Contributing

Contributions are welcome! To add new skills or improve existing ones:

1. Fork this repository
2. Create a feature branch (`git checkout -b feature/new-skill`)
3. Follow the existing skill structure and documentation format
4. Add comprehensive examples and references
5. Submit a pull request

## Roadmap

Planned future skills:
- Deep foundations (pile design to EN 1997-1)
- Retaining wall design (EN 1997-1)
- Soil slope stability analysis
- Pavement design
- Concrete member design (beams, columns, slabs)
- Steel connection design

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Author

Sompote Youwai

## Acknowledgments

- Based on EN 1997-1:2004 (Eurocode 7) standards
- References Brinch Hansen bearing capacity theory
- Follows UK National Annex guidance

## Support

For issues, questions, or suggestions:
- Open an issue on GitHub
- Contact: [your-email@example.com]

## Disclaimer

These skills are provided as engineering tools to assist qualified professionals. All designs should be reviewed by a chartered/professional engineer before implementation. The author assumes no liability for designs produced using these skills.

---

**Note:** Always verify calculations independently and follow local building codes and regulations. These skills supplement but do not replace professional engineering judgment.
