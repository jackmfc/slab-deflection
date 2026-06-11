# Concrete Slab Deflection — AS 3600

A self-contained, standalone browser tool that performs a **detailed deflection check of a
reinforced-concrete slab or beam to AS 3600** (Cl 8.5.3 / 9.3.3), from the UDL over the span.
Produces a branded PDF ("Comps") report.

Independent project — shares no code, server, or data with any other application.

## Run it
Open `index.html` in any browser. No build, no server. Keep `logo.js` alongside it for the PDF header.

## What it does
Pick the **member type** (one-way slab strip / two-way slab / rectangular beam) and **support
condition** (simply supported / one or both ends continuous / cantilever). Enter the section,
reinforcement, materials and the **UDL over the span** (dead G + live Q, with serviceability factors
ψs, ψl). It then computes:

- **Section properties** — gross `Ig`, cracked-transformed `Icr` (with compression steel), neutral
  axis `dn`.
- **Cracking moment** `Mcr = (0.6√f'c − σcs)·Z` (AS 3600 Cl 8.5.3.1; includes shrinkage-induced
  stress σcs).
- **Effective second moment of area** (Branson) `Ief = Icr + (Ig − Icr)(Mcr/Ms)³ ≤ Ief,max`, where
  `Ief,max = Ig` if `Ast/bd ≥ 0.005` else `0.6 Ig`.
- **Immediate** deflection `δ = β·w·Lef⁴/(Ec·Ief)` (β by support condition), for short-term and
  sustained load cases.
- **Long-term** deflection `kcs·δsustained`, with `kcs = 2 − 1.2(Asc/Ast) ≥ 0.8` (Cl 8.5.3.2).
- **Total deflection** vs the chosen limit (AS 3600 Table 2.3.2: span/250, /500, /1000, or custom).

`Ec` defaults to the AS 3600 Table 3.1.2 grade value (editable); `Es = 200 000 MPa`.

## Variables you can set
Span/Lef (and Lx, Ly + load-share for two-way) · support condition · b, D, d, d′ · Ast, Asc ·
f'c, Ec, Es, unit weight, σcs, density ρ · G, Q (with self-weight toggle) · ψs, ψl · deflection limit.

## Important limitations
Calculation aid only — not design certification. Confirm against **AS 3600-2018**, **AS 1170**, and
project docs:
- The **σcs shrinkage-induced stress** materially affects Mcr and deflection; `σcs = 0` is
  unconservative (the tool flags it). Calculate it per Cl 8.5.3.1 for real designs.
- **Continuous members** use the span-region service moment; AS 3600 permits averaging Ief along the
  member — refine for important cases.
- **Two-way slabs** use a short-span strip with a load-share factor — a simplified idealisation;
  verify by rigorous / FE analysis.
- Strength, crack control and detailing are **not** checked — this is deflection only.
