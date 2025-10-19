# Installation Guide for Claude Skills

## Quick Start: Create ZIP Files

### For macOS/Linux:

```bash
# Navigate to the skills directory
cd /Users/sompoteyouwai/env/claude_skill

# Create ZIP for foundation design
zip -r eurocode7-foundation.zip eurocode7-foundation/

# Create ZIP for steel design
zip -r aisc360-steel.zip aisc360-steel/
```

### For Windows (PowerShell):

```powershell
# Navigate to the skills directory
cd C:\path\to\claude_skill

# Create ZIP for foundation design
Compress-Archive -Path eurocode7-foundation -DestinationPath eurocode7-foundation.zip

# Create ZIP for steel design
Compress-Archive -Path aisc360-steel -DestinationPath aisc360-steel.zip
```

## Upload to Claude Desktop

### Method 1: Settings Upload (If Available)

1. Open Claude Desktop
2. Go to **Settings** > **Custom Skills** (or similar option)
3. Click **"Upload Skill"** or **"Import Skill"**
4. Select the ZIP file (`eurocode7-foundation.zip` or `aisc360-steel.zip`)
5. Restart Claude Desktop
6. Test by typing `@eurocode7-foundation` or `@aisc360-steel`

### Method 2: Manual Copy

1. **Find Claude's skills directory:**
   - macOS: `~/.claude/skills/`
   - Linux: `~/.claude/skills/`
   - Windows: `%USERPROFILE%\.claude\skills\`

2. **Copy folders directly:**
   ```bash
   # Create directory if it doesn't exist
   mkdir -p ~/.claude/skills/

   # Copy both skills
   cp -r eurocode7-foundation ~/.claude/skills/
   cp -r aisc360-steel ~/.claude/skills/
   ```

3. **Restart Claude Desktop**

## Verify Installation

### Test Foundation Design Skill:
```
@eurocode7-foundation Design a 2m square pad foundation for 500kN dead load
```

### Test Steel Design Skill:
```
@aisc360-steel Design a W310x52 column for 500kN axial load, length 4m
```

## File Structure

After installation, your skills directory should look like:

```
~/.claude/skills/
├── eurocode7-foundation/
│   ├── SKILL.md
│   └── references/
│       ├── EXAMPLES.md
│       ├── FACTORS.md
│       └── STRUCTURAL.md
└── aisc360-steel/
    ├── SKILL.md
    ├── README.md
    └── skill.yaml
```

## Troubleshooting

### Skill Not Found
- Ensure the folder name matches exactly
- Restart Claude Desktop after copying
- Check that SKILL.md exists in the folder

### Skill Not Working
- Verify SKILL.md has correct YAML frontmatter
- Check folder permissions (should be readable)
- Look at Claude Desktop logs for errors

### Where Are the ZIP Files?
After running the zip commands, the ZIP files will be in:
- macOS/Linux: `/Users/sompoteyouwai/env/claude_skill/`
- Windows: Current directory where you ran the command

## Usage Examples

### Foundation Design:
```
@eurocode7-foundation I need to design a pad foundation:
- Column: 400x400mm
- Dead load: 800kN
- Live load: 300kN
- Soil: φ'=32°, c'=10kPa, γ=19kN/m³
- Design Approach: DA1 (UK)
- Settlement limit: 30mm
```

### Steel Design:
```
@aisc360-steel Design a tension member:
- Section: L150x150x15 angle
- Factored load: Pu=250kN
- Connection: 3 bolts, shear lag factor U=0.60
- Material: A36 steel
- Method: LRFD
```

## Support

For issues:
- GitHub: https://github.com/Sompote/claude_skill_CE
- Check README.md in each skill folder for detailed documentation
