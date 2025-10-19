# Bearing Capacity Factors - Detailed Formulations

Complete formulations for all bearing capacity factors per Brinch Hansen method.

## Table of Contents

1. [Bearing Capacity Factors (N)](#bearing-capacity-factors)
2. [Shape Factors (s)](#shape-factors)
3. [Depth Factors (d)](#depth-factors)
4. [Inclination Factors (i)](#inclination-factors)
5. [Base Inclination Factors (b)](#base-inclination-factors)
6. [Ground Inclination Factors (g)](#ground-inclination-factors)

---

## Bearing Capacity Factors

### Drained Analysis

```
N_q = exp(π × tan φ'_d) × tan²(45° + φ'_d/2)

N_c = (N_q - 1) × cot(φ'_d)

N_γ = 2 × (N_q - 1) × tan(φ'_d)
```

### Undrained Analysis (φ_u = 0)

```
N_c = π + 2 ≈ 5.14
N_q = 1.0
N_γ = 0
```

---

## Shape Factors

### For Drained Conditions

**Rectangular foundation (B' ≤ L'):**

```
s_q = 1 + (B'/L') × sin(φ'_d)

s_c = (s_q × N_q - 1) / (N_q - 1)

s_γ = 1 - 0.3 × (B'/L')
```

**Special cases:**
- Square (B' = L'): s_q = 1 + sin(φ'_d)
- Strip (B'/L' → 0): s_q = 1.0, s_c = 1.0, s_γ = 1.0
- Circular (B' = L' = diameter): Use square formulas

### For Undrained Conditions (φ_u = 0)

```
s_c = 1 + 0.2 × (B'/L')  [rectangular]
s_c = 1.2                 [square/circular]
s_q = 1.0
s_γ = 1.0
```

---

## Depth Factors

### For Drained Conditions

**When D/B ≤ 1:**

```
d_q = 1 + 2 × tan(φ'_d) × (1 - sin φ'_d)² × (D/B')

d_c = d_q - (1 - d_q) / (N_c × tan φ'_d)

d_γ = 1.0
```

**When D/B > 1:**

```
d_q = 1 + 2 × tan(φ'_d) × (1 - sin φ'_d)² × arctan(D/B')

d_c = d_q - (1 - d_q) / (N_c × tan φ'_d)

d_γ = 1.0
```

Note: arctan in radians

### For Undrained Conditions (φ_u = 0)

**When D/B ≤ 1:**
```
d_c = 1 + 0.4 × (D/B')
```

**When D/B > 1:**
```
d_c = 1 + 0.4 × arctan(D/B')
```

```
d_q = 1.0
d_γ = 1.0
```

---

## Inclination Factors

For load with vertical component V and horizontal component H.

### For Drained Conditions

**Determine m value:**

For H parallel to B':
```
m = m_B = (2 + B'/L') / (1 + B'/L')
```

For H parallel to L':
```
m = m_L = (2 + L'/B') / (1 + L'/B')
```

**Calculate factors:**

```
i_q = [1 - H / (V + A' × c'_d × cot φ'_d)]^m

i_c = i_q - (1 - i_q) / (N_c × tan φ'_d)

i_γ = [1 - H / (V + A' × c'_d × cot φ'_d)]^(m+1)
```

**Special cases:**
- For c'_d = 0 (cohesionless soil):
  ```
  i_q = (1 - H/V)^m
  i_γ = (1 - H/V)^(m+1)
  ```

- If expression inside brackets becomes negative or zero, bearing capacity = 0

### For Undrained Conditions (φ_u = 0)

```
i_c = 0.5 × [1 + √(1 - H/(A' × cu_d))]

i_q = 1.0

i_γ = 1.0
```

If H > A' × cu_d, then i_c = 0 (bearing capacity = 0)

---

## Base Inclination Factors

For foundation base inclined at angle α to horizontal.

### For Drained Conditions

```
b_q = b_γ = (1 - α × tan φ'_d)²

b_c = b_q - (1 - b_q) / (N_c × tan φ'_d)
```

where α is in radians

### For Undrained Conditions (φ_u = 0)

```
b_c = 1 - 2α / (π + 2)

b_q = 1.0

b_γ = 1.0
```

where α is in radians

**Note:** For horizontal base (α = 0), all b factors = 1.0

---

## Ground Inclination Factors

For ground surface inclined at angle β to horizontal.

### For Drained Conditions

```
g_q = g_γ = (1 - tan β)²

g_c = g_q - (1 - g_q) / (N_c × tan φ'_d)
```

**Important:** These formulas apply when β ≤ φ'_d

### For Undrained Conditions (φ_u = 0)

```
g_c = β / (5.14 × 147°) = β / 757.4°

g_q = 1.0

g_γ = 1.0
```

where β is in degrees

**Note:** For level ground (β = 0), all g factors = 1.0

---

## Factor Application Summary

**Complete bearing capacity equation:**

```
q_ult = c'_d × N_c × s_c × d_c × i_c × b_c × g_c
      + q' × N_q × s_q × d_q × i_q × b_q × g_q
      + 0.5 × γ' × B' × N_γ × s_γ × d_γ × i_γ × b_γ × g_γ
```

**Application order:**
1. Calculate N factors from φ'_d
2. Calculate s factors from geometry (B'/L')
3. Calculate d factors from embedment (D/B')
4. Calculate i factors from loading (H/V)
5. Calculate b factors from base inclination (α)
6. Calculate g factors from ground slope (β)

**Default values (standard case):**
- Horizontal base (α = 0): b_q = b_c = b_γ = 1.0
- Level ground (β = 0): g_q = g_c = g_γ = 1.0
- No horizontal load (H = 0): i_q = i_c = i_γ = 1.0

---

## Quick Reference Table

| Factor | Affects Term | Key Dependencies |
|--------|--------------|------------------|
| N_c, N_q, N_γ | All | φ'_d only |
| s_c, s_q, s_γ | All | B'/L' ratio |
| d_c, d_q, d_γ | All | D/B' ratio, φ'_d |
| i_c, i_q, i_γ | All | H/V ratio, c'_d |
| b_c, b_q, b_γ | All | Base angle α |
| g_c, g_q, g_γ | All | Ground slope β |

---

## Common Calculation Errors

1. **Wrong m value** - Ensure m relates to direction of H
2. **Units in arctan** - Must use radians
3. **Negative brackets** - Check H/(V + A'c'cotφ) < 1
4. **φ'_d = 0 division** - Use undrained formulas when φ_u = 0
5. **Forgetting A'** - Effective area, not total area
6. **Sign conventions** - All angles positive as defined

---

## Validation Checks

Before using calculated factors:
- [ ] All i factors between 0 and 1
- [ ] s_γ ≤ 1.0 for rectangular foundations
- [ ] s_q ≥ 1.0 always
- [ ] d factors ≥ 1.0 (embedment helps)
- [ ] For standard case (level, no H): only N and s factors vary from 1.0
