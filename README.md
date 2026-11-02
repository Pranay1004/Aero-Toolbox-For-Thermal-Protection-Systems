# Aero-Toolbox: Re-entry Aerothermodynamics & Hypersonic Heating Library

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Python: 3.9+](https://img.shields.io/badge/Python-3.9%2B-blue.svg)]()
[![Domain](https://img.shields.io/badge/Domain-Hypersonics%20%7C%20Re--entry-orange.svg)]()

Deterministic computational library and interactive web platform for hypersonic atmospheric re-entry mission analysis, aerothermodynamic stagnation heating, Thermal Protection System (TPS) sizing, and trajectory corridor constraints.

---

## Theoretical Models Implemented

Implements canonical formulations from *Re-entry Missions* (Ch. 15):

1. **Stagnation-Point Convective Heating**:
   - Scott et al. stagnation formulation:
     $$q_t = \frac{18300}{\sqrt{R_N}} \sqrt{\rho_\infty} \left(\frac{V_\infty}{10^4}\right)^{3.05} \quad [\text{W/cm}^2]$$
   - Atmospheric reference model:
     $$q_t = \frac{11030}{\sqrt{R_N}} \left(\frac{\rho_\infty}{\rho_0}\right)^{0.5} \left(\frac{V_\infty}{V_0}\right)^{3.15}$$
2. **Radiative Stagnation Heating**:
   - Martin radiative shock-layer formulation:
     $$q_r = 100 R_N \left(\frac{V_\infty}{10^4}\right)^{8.5} \left(\frac{\rho_\infty}{\rho_0}\right)^{1.6}$$
3. **Thermal Protection System (TPS) Sizing**:
   - **Heat-Sink TPS**: $Q = m \cdot C_v \cdot \Delta T$
   - **Ablative Mass Loss**: $Q = h_v \cdot \Delta m$
   - **Radiative-Equilibrium Wall Temperature**: $q_{net} \approx \epsilon \sigma_{SB} T_w^4$
4. **Trajectory & Structural Flight Corridor Constraints**:
   - Dynamic pressure envelope: $q = \frac{1}{2} \rho V^2 \le q_{max}$
   - Normal load-factor constraint: $n_z = \frac{D}{m} \sqrt{1 + (L/D)^2} \le n_{max}$
   - Equilibrium glide boundary: $\frac{L}{m} \cos(\sigma) \le \left(g - \frac{V^2}{r}\right) \cos(\gamma)$

---

## Directory Structure

```
aero-toolbox/
├── api/
│   └── index.py            # Serverless FastAPI/Flask calculation router
├── static/
│   ├── app.js              # Interactive client calculator logic
│   ├── index.html          # Clean technical web interface
│   └── style.css           # Modern technical UI styling
├── reentry_lib.py          # Core mathematical physics library
├── requirements.txt        # Runtime dependencies (numpy, scipy)
├── vercel.json             # Cloud deployment routing configuration
├── .gitignore              # Python build ignore rules
├── LICENSE                 # MIT License
└── README.md               # Mathematical documentation and API guide
```

---

## Usage

### Python API
```python
from reentry_lib import stagnation_convective_heat_flux, tps_ablative_mass_loss

# Evaluate peak stagnation convective heat flux (RN = 0.5 m, V = 7800 m/s, rho = 1.2e-4 kg/m3)
q_dot = stagnation_convective_heat_flux(rho_inf=1.2e-4, v_inf=7800.0, r_nose=0.5)
print(f"Stagnation Heat Flux: {q_dot:.2f} W/cm^2")

# Size ablative TPS for total integrated heat load Q = 450 MJ, hv = 30 MJ/kg
m_ablated = tps_ablative_mass_loss(q_total=4.5e8, h_v=3.0e7)
print(f"Required Ablator Mass: {m_ablated:.2f} kg")
```

---

## Deployment
Deploy locally or to cloud serverless endpoints:
```bash
pip install -r requirements.txt
python3 -m unittest discover tests/
```
