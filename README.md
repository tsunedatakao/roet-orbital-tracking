# ROET Orbital Tracking Program

This repository contains orbital-tracking programs used for reactive orbital energy
theory (ROET) analysis. The programs track molecular orbitals along reaction pathways
or trajectories and output the tracked orbital energies and orbital indices.

## Programs

This repository includes the following scripts:

- `ROET_g16_r.py`: Tracks restricted Kohn-Sham orbitals from Gaussian 16 output files.
- `ROET_g16_u.py`: Tracks unrestricted Kohn-Sham orbitals from Gaussian 16 output files.

## Overview

The programs track molecular orbitals between consecutive geometries along a reaction path
by evaluating orbital overlaps. The resulting orbital-energy profiles can be used to analyze
molecular orbital energy variations along the reaction coordinate.

In ROET analysis, the occupied orbital that is most stabilized along the pathway is assigned
as the occupied reactive orbital (ORO), whereas the virtual orbital that is most destabilized
is assigned as the virtual reactive orbital (VRO). These assignments are not performed automatically
by the orbital-tracking programs; they should be made by analyzing the output orbital-energy profiles.

## Requirements

- Python 3
- NumPy

Additional Python packages may be required depending on the local environment and plotting workflow.

## Input preparation for Gaussian 16

For Gaussian 16 calculations, perform single-point calculations at all geometries along the reaction path
or trajectory.

The following Gaussian options are required:

```text
pop=full
NoSymm
IOp(3/33=1)
```

These options are needed for the following reasons:

- `pop=full` outputs all molecular orbitals that can be expanded in the basis functions.
- `Nosymmetry` prevents Gaussian from rotating the molecule through the standard-orientation procedure.
- `IOp(3/33=1)` prints the overlap matrix required for orbital tracking.

Both `.log` and `.fchk` files are required. The `.fchk` files should be generated from the corresponding
checkpoint files using the `formchk` command. The `.log` and `.fchk` files must be placed in the same directory.

## Usage for Gaussian 16

Run the Gaussian orbital-tracking program by passing the `.log` files for all geometries along
the reaction path in the correct order:

```bash
python ROET_g16_r.py irc_000.log irc_001.log irc_002.log ...
```

for restricted Kohn-Sham orbitals, or

```bash
python ROET_g16_u.py irc_000.log irc_001.log irc_002.log ...
```

for unrestricted Kohn-Sham orbitals.

Only the `.log` files need to be specified on the command line. The program automatically reads
the corresponding `.fchk` files from the same directory.

The output files are written to the current working directory.

A successful calculation prints the following message:

```text
Normal termination of roet-program
```

## Output files for Gaussian 16

### `result_ene.csv`

This file contains the tracked orbital energies. The orbital-energy profiles can be visualized
using spreadsheet software or plotting tools such as Matplotlib.

The most stabilized occupied orbital along the pathway corresponds to the occupied reactive orbital (ORO),
and the most destabilized virtual orbital corresponds to the virtual reactive orbital (VRO).
These orbitals should be identified by analyzing this file.

### `result_indx.csv`

This file contains the tracked orbital indices. At the initial geometry, the lowest-energy orbital
is indexed as 0, and the highest-energy orbital is indexed as the maximum value. This file allows one
to inspect changes in orbital ordering along the reaction path.

## Notes

- The calculation may be computationally demanding for large systems. It is recommended to run
the programs on a suitable computational server or cluster.
- The input calculation files and program path should be specified explicitly when running the programs.
- If the program prints `bug` and stops, an orbital-tracking problem has occurred. In this case,
check the input files, orbital ordering, and calculation settings.
- The programs perform orbital tracking but do not automatically identify the ORO and VRO.

## Citation

If you use this code, please cite the associated paper:

> T. Tsuneda and R. K. Singh, "Reactivity index based on orbital energies", Journal of Computational Chemistry
35, 1093-1100 (2014).

and

> N. Takai, T. Tsutsumi, K. Saita, T. Taketsugu and T. Tsuneda, "Revealing the electron driven mechanism
in metal catalyzed Kumada cross coupling reaction", Scientific Reports 15, 4421 (2025).


Please also cite the archived software DOI if available.

## License

This code is distributed under the MIT License.
