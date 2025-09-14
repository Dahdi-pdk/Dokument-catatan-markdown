```python

from rdkit import Chem
from rdkit.Chem import AllChem
import numpy as np
import matplotlib.pyplot as plt
from mpl_toolkits.mplot3d import Axes3D  # noqa: F401

# Buat molekul etena dari SMILES, tambahkan atom H, lalu embed & optimisasi geometri
smiles = "C=C"  # etena
mol = Chem.MolFromSmiles(smiles)
mol = Chem.AddHs(mol)

# Embed 3D dan optimisasi menggunakan UFF (Universal Force Field)
AllChem.EmbedMolecule(mol, randomSeed=42)
AllChem.UFFOptimizeMolecule(mol)

# Ambil koordinat 3D dari konformer
conf = mol.GetConformer()
coords = np.array([list(conf.GetAtomPosition(i)) for i in range(mol.GetNumAtoms())])

# Ambil simbol atom untuk pelabelan
atom_symbols = [atom.GetSymbol() for atom in mol.GetAtoms()]

# Buat plot 3D
fig = plt.figure(figsize=(6, 6))
ax = fig.add_subplot(111, projection='3d')

# Gambar titik atom
ax.scatter(coords[:, 0], coords[:, 1], coords[:, 2], s=120)

# Tambahkan label atom
for i, (x, y, z) in enumerate(coords):
    ax.text(x, y, z, f" {atom_symbols[i]}", size=12)

# Gambar ikatan sebagai garis
for bond in mol.GetBonds():
    i = bond.GetBeginAtomIdx()
    j = bond.GetEndAtomIdx()
    x_vals = [coords[i, 0], coords[j, 0]]
    y_vals = [coords[i, 1], coords[j, 1]]
    z_vals = [coords[i, 2], coords[j, 2]]
    ax.plot(x_vals, y_vals, z_vals, linewidth=2)

# Set label sumbu
ax.set_xlabel('X (Å)')
ax.set_ylabel('Y (Å)')
ax.set_zlabel('Z (Å)')
ax.set_title('Etena (C2H4) — Struktur 3D')

# Atur aspek agar proporsional
max_range = coords.max(axis=0) - coords.min(axis=0)
max_range = max(max_range.max(), 1e-6)
mid = coords.mean(axis=0)
ax.set_xlim(mid[0] - max_range/2, mid[0] + max_range/2)
ax.set_ylim(mid[1] - max_range/2, mid[1] + max_range/2)
ax.set_zlim(mid[2] - max_range/2, mid[2] + max_range/2)

plt.show()
```

