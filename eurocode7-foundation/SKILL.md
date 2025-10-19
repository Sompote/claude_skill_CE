---
name: eurocode7-foundation
description: Design shallow foundations according to EN 1997-1:2004 (Eurocode 7). Use for bearing capacity verification, settlement analysis, sliding resistance checks, and structural design of pad, strip, and raft foundations following GEO and STR limit states.
---

# Eurocode 7 Foundation Design

Design shallow foundations to EN 1997-1:2004 using Design Approaches 1, 2, or 3.

## When to Use This Skill

- Design pad, strip, or raft foundations
- Verify bearing capacity (GEO limit state)
- Calculate settlement (SLS)
- Check sliding resistance
- Structural design of foundation (STR limit state)
- Professional geotechnical engineering tasks requiring EC7 compliance

## Quick Start

### Essential Information Required

Always request from user:
1. **Design Approach** (DA1/DA2/DA3) - Check national annex
2. **Soil parameters** (characteristic): φ'_k, c'_k or cu_k, γ, E'
3. **Loading** (characteristic): G_k, Q_k, H_k, M_k
4. **Foundation constraints**: Embedment depth, settlement limits

### Basic Workflow

```
1. Identify Design Approach → determines partial factors
2. Calculate design actions → apply γ_G, γ_Q
3. Preliminary sizing → estimate foundation dimensions
4. ULS verification:
   - Bearing capacity (both combinations for DA1)
   - Sliding resistance (if H_k > 0)
5. SLS verification:
   - Settlement calculation
   - Check against limits
6. Structural design → reinforcement per EC2
```

## Design Approaches and Partial Factors

### DA1 (UK) - TWO combinations required

**Combination 1: A1+M1+R1**
- Actions: γ_G=1.35, γ_Q=1.5
- Soil: γ_φ'=1.0, γ_c'=1.0, γ_cu=1.0
- Resistance: γ_R;v=1.0, γ_R;h=1.0

**Combination 2: A2+M2+R1**
- Actions: γ_G=1.0, γ_Q=1.3
- Soil: γ_φ'=1.25, γ_c'=1.25, γ_cu=1.4
- Resistance: γ_R;v=1.0, γ_R;h=1.0

### DA2 (Germany, Italy)

**Single combination: A1+M1+R2**
- Actions: γ_G=1.35, γ_Q=1.5
- Soil: γ_φ'=1.0, γ_c'=1.0, γ_cu=1.0
- Resistance: γ_R;v=1.4, γ_R;h=1.1

### DA3 (France, Belgium)

**Single combination: A1/A2+M2+R3**
- Actions: γ_G=1.35 (structural), γ_G=1.0 (geotechnical), γ_Q=1.5/1.3
- Soil: γ_φ'=1.25, γ_c'=1.25, γ_cu=1.4
- Resistance: γ_R;v=1.0, γ_R;h=1.0

## Bearing Capacity

### Ultimate Bearing Capacity (Brinch Hansen)

**Drained conditions:**
```
q_ult = c'_d·N_c·s_c·d_c·i_c + q'·N_q·s_q·d_q·i_q + 0.5·γ'·B'·N_γ·s_γ·d_γ·i_γ
```

**Undrained conditions (φ_u=0):**
```
q_ult = (π+2)·cu_d·s_c·d_c·i_c + q
```

**Bearing capacity factors:**
```
N_q = exp(π·tanφ'_d)·tan²(45°+φ'_d/2)
N_c = (N_q-1)·cot(φ'_d)
N_γ = 2(N_q-1)·tan(φ'_d)
```

**For detailed factor formulations**, see [FACTORS.md](references/FACTORS.md)

### Key Steps

1. **Apply eccentricity** → B'=B-2e_B, L'=L-2e_L
2. **Design soil parameters** → Apply γ_φ', γ_c', γ_cu per M-set
3. **Calculate factors** → N, s, d, i factors
4. **Apply resistance factor** → R_d = q_ult·A'/γ_R;v
5. **Verify** → V_d ≤ R_d

**For DA1**: BOTH combinations must pass

## Settlement Analysis

Use **characteristic values** (no partial factors).

### Immediate Settlement (Elastic)

```
ρ_i = q_net·B·(1-ν²)/E'·I_p
```
where q_net = q_SLS - σ'_v0

**Influence factors:**
- Square: I_p ≈ 1.12
- Circular: I_p ≈ 1.0
- Rectangle: depends on L/B

### Consolidation Settlement (Clay)

**Normally consolidated:**
```
ρ_c = (C_c·H)/(1+e_0)·log₁₀[(σ'_v0+Δσ'_v)/σ'_v0]
```

**Overconsolidated:**
```
If σ'_v0+Δσ'_v ≤ σ'_p:
  ρ_c = (C_r·H)/(1+e_0)·log₁₀[(σ'_v0+Δσ'_v)/σ'_v0]

If σ'_v0+Δσ'_v > σ'_p:
  ρ_c = (C_r·H)/(1+e_0)·log₁₀(σ'_p/σ'_v0) 
      + (C_c·H)/(1+e_0)·log₁₀[(σ'_v0+Δσ'_v)/σ'_p]
```

### Typical Acceptance Criteria

- Total settlement: 25-50 mm (isolated), 50-75 mm (raft)
- Differential: δ/L < 1/500 (general), < 1/300 (if cracking acceptable)
- Angular distortion: 1/500 to 1/300

**Verify with national annex and structure type**

## Sliding Resistance

**Drained:**
```
R_h = V_d·tan(δ_d) + c'_a;d·A_b
```
where δ_d ≈ (2/3)φ'_d for concrete-soil interface

**Undrained:**
```
R_h = ca_d·A_b
```
where ca_d = α·cu_d (α = 0.5-1.0)

**Design resistance:**
```
R_d;h = R_h/γ_R;h
```

**Verify:** H_d ≤ R_d;h

If not satisfied: add passive pressure, shear keys, or increase base area

## Structural Design (EC2)

Use **STR limit state** (typically Combination 1 for DA1).

### Critical Sections

1. **Bending**: Column face
   ```
   M_Ed = q_design·L_cant²/2
   ```

2. **Punching shear**: Perimeter at d from column
   ```
   v_Ed = V_Ed/(u·d)
   ```
   where u = perimeter, V_Ed = load minus soil reaction within perimeter

3. **Beam shear**: Distance d from column face

### Reinforcement Design

```
K = M_Ed/(b·d²·f_cd)
z = d[0.5+√(0.25-K/1.134)] ≤ 0.95d
A_s = M_Ed/(f_yd·z)
```

Check A_s ≥ A_s,min = 0.26(f_ctm/f_yk)bd

**For punching shear design details**, see [STRUCTURAL.md](references/STRUCTURAL.md)

## Output Format

Produce professional calculation documents including:

1. **Design data summary**
   - Geometry, soil parameters, loading
   - Design approach and partial factors

2. **ULS verification**
   - Bearing capacity (show all combinations)
   - Sliding resistance
   - Intermediate calculations clearly shown

3. **SLS verification**
   - Settlement calculations
   - Comparison with acceptance criteria

4. **Structural design**
   - Bending moments
   - Shear checks
   - Reinforcement schedule

5. **Summary table**
   - All verifications with utilization ratios
   - Pass/fail status

6. **Design notes and assumptions**
   - Critical assumptions requiring verification
   - Construction requirements
   - Limitations

## Critical Reminders

- **DA1 requires BOTH combinations** - common error is checking only one
- **Effective dimensions** - Always account for eccentricity: B'=B-2e, L'=L-2e
- **Settlement uses characteristic values** - No partial factors for SLS
- **Water table** - Use γ'=γ_sat-γ_w below water
- **Inclination factors** - Critical when H_k > 0, significantly reduce capacity
- **Design approach must match national annex** - Not interchangeable

## Special Conditions

### Eccentric/Inclined Loading
- Calculate effective area method
- Apply all i factors (i_q, i_c, i_γ)
- Check both directions for 2D eccentricity

### Layered Soils
- Identify weakest layer
- Check bearing in each layer
- Use weighted average for settlement
- Consider punching through weak layers

### Sloping Ground
- Apply ground inclination factors (g_q, g_c, g_γ)
- Consider slope stability
- Increase embedment depth
- Maintain setback from crest

### Adjacent Foundations
- Consider stress overlap for settlement
- Minimum spacing: 2-3×width
- Include nearby loads in bearing check

## References

For detailed calculation procedures:
- **[FACTORS.md](references/FACTORS.md)** - Complete shape, depth, inclination factor formulations
- **[STRUCTURAL.md](references/STRUCTURAL.md)** - Punching shear, flexural design details
- **[EXAMPLES.md](references/EXAMPLES.md)** - Worked examples for common cases

## Quality Checks

Before finalizing design:
- [ ] Design approach confirmed from national annex
- [ ] All partial factors correctly applied
- [ ] Characteristic soil values properly determined
- [ ] Both combinations checked (DA1)
- [ ] Eccentricity effects included
- [ ] Water table effects considered
- [ ] Settlement within limits
- [ ] Structural design complete
- [ ] Assumptions clearly documented
