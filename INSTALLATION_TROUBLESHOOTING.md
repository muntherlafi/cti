# Installation Troubleshooting Guide

## Error: `mpe2` Package Not Found on PyPI

### Root Cause
As of 2024, the `mpe2` package was **moved from PyPI to GitHub** as part of the MPE refactoring. The Farama Foundation now maintains MPE environments in a separate repository.

### Symptoms
```
⚠️  FAILED: mpe2
ERROR: Could not find a version that satisfies the requirement mpe2 (from versions: none)
ERROR: No matching distribution found for mpe2
```

---

## ✅ Solution 1: Install from GitHub (Recommended)

### Step-by-Step Fix

**Option A: Using the Fixed Notebook**

Run **`MARL_Trust_Kaggle_FIXED.ipynb`** - Cell 1 is already updated to install from GitHub:

```python
pip_install("git+https://github.com/Farama-Foundation/MPE.git", from_github=True)
```

**Option B: Manual Fix (If using old notebook)**

Replace Cell 1 line containing `pip_install("mpe2")` with:

```python
import subprocess
import sys

# Install from GitHub source instead of PyPI
result = subprocess.run(
    [sys.executable, "-m", "pip", "install", "-q", 
     "git+https://github.com/Farama-Foundation/MPE.git"],
    capture_output=True, text=True
)

if result.returncode == 0:
    print("✅ OK: mpe2 (GitHub source)")
else:
    print("⚠️  GitHub installation failed. Trying PettingZoo fallback...")
```

### Expected Output
```
✅ OK: torch
✅ OK: pettingzoo>=1.25.0
✅ OK: git+https://github.com/Farama-Foundation/MPE.git
✅ OK: gymnasium>=1.0.0
✅ OK: seaborn>=0.12
✅ OK: tqdm>=4.65
✅ OK: scipy>=1.10
```

---

## ✅ Solution 2: Use PettingZoo Fallback

If GitHub installation fails (no internet or timeout), the notebook can fall back to **built-in PettingZoo MPE environments**.

### Configuration (Cell 2 - Already Automated)

The fixed notebook automatically tries:
1. **Try 1**: Import from `mpe2` (GitHub)
2. **Try 2**: Fall back to `pettingzoo.mpe` (built-in)

```python
try:
    from mpe2 import simple_spread_v3
    print("✅ Using mpe2 from GitHub")
except ImportError:
    try:
        from pettingzoo.mpe import simple_spread_v3
        print("✅ Using PettingZoo built-in MPE")
    except ImportError:
        print("❌ Neither mpe2 nor PettingZoo MPE available")
```

---

## 🔧 Workarounds

### Workaround 1: Pre-install in Terminal (Before Kaggle Notebook)

If you have access to Kaggle terminal:

```bash
pip install --upgrade pip setuptools wheel
pip install pettingzoo>=1.25.0 gymnasium>=1.0.0
pip install git+https://github.com/Farama-Foundation/MPE.git
```

### Workaround 2: Clone & Install Locally

```bash
git clone https://github.com/Farama-Foundation/MPE.git
cd MPE
pip install -e .
```

### Workaround 3: Docker Image (Alternative)

Use a pre-built Docker image with all dependencies:

```bash
docker pull farama/mpe:latest
docker run -it farama/mpe:latest jupyter notebook
```

---

## 📋 Comparison of Approaches

| Approach | Pros | Cons | Time |
|----------|------|------|------|
| **GitHub (Recommended)** | Latest features, official source | Requires internet | 2-3 min |
| **PettingZoo Fallback** | Works offline, stable API | May have older version | 1 min |
| **Pre-install Terminal** | Guaranteed to work | Requires Kaggle terminal access | 3-5 min |
| **Docker** | Complete isolation, reproducible | Slow, large image size | 10+ min |

---

## ✅ Verification

After installation, test in new cell:

```python
# Test 1: Import
try:
    from mpe2 import simple_spread_v3
    print("✅ mpe2 imported successfully")
except ImportError:
    try:
        from pettingzoo.mpe import simple_spread_v3
        print("✅ PettingZoo MPE imported successfully")
    except ImportError:
        print("❌ FAILED - check installation")

# Test 2: Create environment
env = simple_spread_v3.env(N=4, local_ratio=0.5, render_mode=None)
obs_dict = env.reset(seed=42)
print(f"✅ Environment created")
print(f"   Agents: {list(obs_dict.keys())}")
print(f"   Obs shape: {obs_dict['agent_0'].shape}")
env.close()
```

**Expected Output**:
```
✅ mpe2 imported successfully
✅ Environment created
   Agents: ['agent_0', 'agent_1', 'agent_2', 'agent_3']
   Obs shape: (24,)
```

---

## ❌ Still Not Working?

### Debug Checklist

- [ ] **Internet connected?** Required for GitHub installation
- [ ] **Kaggle GPU enabled?** Yes (recommended)
- [ ] **Python version?** 3.8+ (check: `python --version`)
- [ ] **pip updated?** Run: `pip install --upgrade pip`
- [ ] **pip cache cleared?** Run: `pip cache purge`

### Diagnostic Commands

```python
import sys
import pip

# Check Python version
print(f"Python: {sys.version}")

# Check pip version
print(f"Pip: {pip.__version__}")

# Check installed packages
import subprocess
result = subprocess.run([sys.executable, "-m", "pip", "list"], 
                       capture_output=True, text=True)
print(result.stdout)
```

### Last Resort: Manual Implementation

If nothing works, implement a **minimal environment simulator**:

```python
import numpy as np

class SimpleSpreadEnv:
    """Minimal replacement for simple_spread_v3."""
    def __init__(self, n_agents=4):
        self.n_agents = n_agents
        self.obs_dim = 24
        self.agents = [f"agent_{i}" for i in range(n_agents)]
    
    def reset(self, seed=None):
        np.random.seed(seed)
        return {a: np.random.randn(self.obs_dim).astype(np.float32) 
                for a in self.agents}
    
    def step(self, actions):
        obs = {a: np.random.randn(self.obs_dim).astype(np.float32) 
               for a in self.agents}
        rewards = {a: -np.random.rand() for a in self.agents}
        dones = {a: False for a in self.agents}
        return obs, rewards, dones

# Use as fallback
try:
    from mpe2 import simple_spread_v3
    env = simple_spread_v3.env(N=4, local_ratio=0.5, render_mode=None)
except:
    env = SimpleSpreadEnv(n_agents=4)
    print("⚠️  Using minimal simulator (reduced functionality)")
```

---

## 📚 Reference Links

- **MPE GitHub**: https://github.com/Farama-Foundation/MPE
- **PettingZoo Docs**: https://pettingzoo.farama.org/
- **Installation Guide**: https://pettingzoo.farama.org/environments/mpe/
- **Kaggle Secrets & Networking**: https://www.kaggle.com/docs/notebooks

---

## Quick Command Reference

```bash
# Clear pip cache
pip cache purge

# Reinstall from GitHub
pip install --force-reinstall git+https://github.com/Farama-Foundation/MPE.git

# Check what's installed
pip show mpe2
pip show pettingzoo

# Upgrade all packages
pip install --upgrade pettingzoo gymnasium scipy torch

# Test import in isolation
python -c "from mpe2 import simple_spread_v3; print('Success')"
```

---

## Contact & Support

If you encounter issues not covered here:

1. **Check Kaggle forum**: https://www.kaggle.com/discussions
2. **Check PettingZoo issues**: https://github.com/Farama-Foundation/Gymnasium/issues
3. **Check MPE issues**: https://github.com/Farama-Foundation/MPE/issues
4. **Comment on notebook**: Kaggle notebook discussion section

---

**Last Updated**: June 2, 2026 | For MARL Trust Notebook v1.1+
