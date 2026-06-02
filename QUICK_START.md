# MARL Trust Notebook — Installation & Usage Guide

## 🚨 CRITICAL FIX: mpe2 Package Installation

The original notebook failed because `mpe2` was moved to GitHub. This has been **FIXED**.

---

## 📋 Which File to Use?

### For New Users (RECOMMENDED)
**File**: `MARL_Trust_Kaggle_FIXED.ipynb`

**Why**: 
- ✅ Corrected Cell 1 with GitHub installation
- ✅ Automatic fallback to PettingZoo if GitHub fails
- ✅ All 13 cells (config, environment, training, tuning, checkpoints)
- ✅ Works even with poor internet

**How to use**:
1. Copy URL: `https://github.com/muntherlafi/cti/blob/master/MARL_Trust_Kaggle_FIXED.ipynb`
2. Open Kaggle → Create Notebook → Import from GitHub
3. Paste URL → Load → Run all cells

---

### For Reference/Advanced Users
**Files**:
- `MARL_Trust_Kaggle_Full.ipynb` - Complete (13-cell) version with all features
- `README_MARL_KAGGLE.md` - Comprehensive documentation
- `HYPERPARAMETER_TUNING_GUIDE.md` - Tuning recipes & workflows
- `INSTALLATION_TROUBLESHOOTING.md` - Detailed troubleshooting

---

## 🔧 What Was Fixed

### Problem
```
⚠️  FAILED: mpe2
ERROR: Could not find a version that satisfies the requirement mpe2
```

### Root Cause
PyPI version of `mpe2` doesn't exist (moved to GitHub in 2024).

### Solution
**Cell 1 now installs from GitHub source:**

```python
pip install git+https://github.com/Farama-Foundation/MPE.git
```

**With automatic fallback to PettingZoo if GitHub fails:**

```python
try:
    from mpe2 import simple_spread_v3  # GitHub version
except ImportError:
    from pettingzoo.mpe import simple_spread_v3  # Fallback
```

---

## ✅ Quick Start (3 Steps)

### Step 1: Import Fixed Notebook to Kaggle
```
1. Go to https://kaggle.com/notebooks
2. Click "Create" → "Notebook"
3. Click "File" → "Import notebook"
4. Paste URL: https://github.com/muntherlafi/cti/blob/master/MARL_Trust_Kaggle_FIXED.ipynb
5. Click "Load"
```

### Step 2: Enable GPU (Optional but Recommended)
```
Settings → Accelerator → GPU (P100 or T4)
```

### Step 3: Run All Cells
```
Click "Run all" or run each cell sequentially
Expected time: ~2 hours for full run
```

---

## 📊 Notebook Structure (13 Cells)

| Cell | Purpose | Status |
|------|---------|--------|
| 1 | **Install dependencies** (FIXED) | ✅ Corrected |
| 2 | Imports & validation | ✅ Works |
| 3 | Configuration | ✅ Works |
| 4 | DefectorWrapper | ✅ Works |
| 5 | ReputationTracker | ✅ Works |
| 6 | Networks (Actor/Critic) | ✅ Works |
| 7 | ReplayBuffer | ✅ Works |
| 8 | MADDPGAgent | ✅ Works |
| 9 | Training loop | ✅ Works |
| 10 | **Hyperparameter tuning** (NEW) | ✅ Added |
| 11 | **Checkpoint management** (NEW) | ✅ Added |
| 12 | Run experiments | ✅ Works |
| 13 | Visualization | ✅ Works |

---

## 🎯 Next Steps After Import

### For First-Time Users
```
1. Read: README_MARL_KAGGLE.md (5 min)
2. Run: Cells 1-2 (validation)
3. Configure: Cell 3 (default settings)
4. Run: Cells 4-9 (environment & agent setup)
5. Tune: Cell 10 (optional, use BASELINE mode)
6. Train: Cell 12 (run experiments, ~1 hour)
7. Analyze: Cell 13 (visualize results)
```

### For Experienced Users
```
1. Run Cell 1-2 (setup)
2. Jump to Cell 10 (set TUNING_MODE)
3. Run Cell 12 (experiments)
4. Run Cell 13 (results)
```

### For Optimization
```
1. Run SENSITIVITY mode (Cell 10)
2. Analyze outputs
3. Run GRID_SEARCH on top parameters
4. Finalize with best config
```

---

## 🆘 Troubleshooting

### Cell 1 Still Fails?
See **`INSTALLATION_TROUBLESHOOTING.md`** for:
- Alternative installation methods
- Fallback approaches
- Diagnostic commands
- Manual implementation

### Quick Fixes
```python
# If Cell 1 fails, try this in Cell 1:
# Option A: Use PettingZoo only
pip install pettingzoo>=1.25.0 gymnasium>=1.0.0

# Option B: Manual GitHub install
subprocess.run([sys.executable, "-m", "pip", "install", "-q",
                "git+https://github.com/Farama-Foundation/MPE.git"])

# Then proceed to Cell 2
```

### GPU Memory Issues?
Edit Cell 3:
```python
cfg = Config(
    batch_size=64,        # Reduce from 128
    buffer_size=25_000,   # Reduce from 50_000
    eval_episodes=10,     # Reduce from 20
)
```

---

## 📚 Documentation Files

| File | Purpose | Read Time |
|------|---------|-----------|
| `README_MARL_KAGGLE.md` | Complete guide (overview, metrics, tuning) | 20 min |
| `HYPERPARAMETER_TUNING_GUIDE.md` | 4 tuning modes + recipes + debugging | 15 min |
| `INSTALLATION_TROUBLESHOOTING.md` | Deep troubleshooting for mpe2 & other issues | 10 min |
| This file | Quick reference & fixes | 5 min |

---

## 🚀 Example Workflows

### Workflow A: Quick Demo (30 minutes)
```python
# Cell 3
cfg = Config(n_episodes=50)  # Very short

# Cell 10
TUNING_MODE = "BASELINE"

# Run Cells 12-13
# Output: Quick results for testing
```

### Workflow B: Full Experiment (2-3 hours)
```python
# Cell 3 (default)
cfg = Config()

# Cell 10
TUNING_MODE = "BASELINE"

# Run Cells 12-13
# Output: Complete metrics + error bars
```

### Workflow C: Hyperparameter Optimization (4-5 hours)
```python
# Cell 10 - Iteration 1
TUNING_MODE = "SENSITIVITY"
# Run 12-13 → Analyze outputs

# Cell 10 - Iteration 2
TUNING_MODE = "GRID_SEARCH"
# Run 12-13 → Find optimal params

# Cell 3 - Iteration 3 (update with optimal params)
cfg = Config(rep_alpha=optimal_alpha, ...)

# Cell 10 - Iteration 3
TUNING_MODE = "BASELINE"
# Run 12-13 → Final results
```

---

## 📊 Expected Outputs

After Cell 13:
```
/kaggle/working/marl_trust/
├── metrics.csv           # Raw results
├── results.png          # 4-panel visualization
└── checkpoints/         # Model weights
    ├── soft_run0/
    │   ├── actor_ep50.pth
    │   ├── critic_ep50.pth
    │   └── ...
```

**Metrics CSV Format**:
```
condition,run,config,team_reward,landmark_coverage,convergence_speed,false_positive_rate
none,0,baseline,-12.3456,0.750,inf,0.1250
binary,0,baseline,-11.8901,0.850,45.00,0.0820
soft,0,baseline,-10.2345,0.925,32.00,0.0450
```

---

## ✨ New Features (vs. Original)

| Feature | Original | Fixed Version |
|---------|----------|---------------|
| Installation | ❌ Breaks | ✅ Fixed |
| Fallback | ❌ None | ✅ Auto-fallback to PettingZoo |
| Hyperparameter Tuning | ❌ Manual | ✅ 4 interactive modes |
| Checkpoint Resume | ❌ Manual | ✅ Auto-detect & resume |
| Documentation | ❌ Minimal | ✅ 3 comprehensive guides |
| GPU Memory | ⚠️ May OOM | ✅ Optimized |
| Error Handling | ❌ Crashes | ✅ Graceful degradation |

---

## 💡 Pro Tips

1. **First time**: Always run with `n_episodes=100` and `TUNING_MODE="BASELINE"` to validate
2. **Save outputs**: Download metrics.csv after each run
3. **Compare configs**: Use multiple runs to build comparison table
4. **Resume training**: Use checkpoints to continue if interrupted
5. **Monitor GPU**: Watch GPU memory in Kaggle settings

---

## 📞 Support

### If installation still fails:
1. Check `INSTALLATION_TROUBLESHOOTING.md`
2. Try workarounds (PettingZoo fallback, pre-install, Docker)
3. Comment on Kaggle notebook for community help
4. Check MPE GitHub issues: https://github.com/Farama-Foundation/MPE/issues

### For other issues:
1. Check `README_MARL_KAGGLE.md` troubleshooting section
2. Enable verbose output: Add `print()` statements
3. Check cell outputs line-by-line
4. Run diagnostic commands in new cells

---

## 📝 Files in Repository

```
muntherlafi/cti/
├── MARL_Trust_Kaggle_FIXED.ipynb           ← START HERE (13 cells, all features)
├── MARL_Trust_Kaggle_Full.ipynb            (Alternative full version)
├── README_MARL_KAGGLE.md                   (Comprehensive guide)
├── HYPERPARAMETER_TUNING_GUIDE.md          (Tuning reference)
├── INSTALLATION_TROUBLESHOOTING.md         (Detailed troubleshooting)
├── QUICK_START.md                          (This file)
└── ORIGINAL_MARL_COLAB.ipynb               (Original Colab version, for reference)
```

---

**Version**: 1.1 (FIXED) | **Date**: June 2, 2026 | **Status**: ✅ Ready for Kaggle

**Start with**: `MARL_Trust_Kaggle_FIXED.ipynb`
