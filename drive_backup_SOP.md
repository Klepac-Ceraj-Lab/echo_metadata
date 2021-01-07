# Backing up files between systems

There are 3 locations where raw sequencing data and analysis files
for the ECHO project are canonically stored
that should be mirrors of one another.

1. `rosalind` - the large iMac inside the lab
2. `ada` - the linux desktop inside Vanja / Kevin's office in Simpson
3. `NTM` - a cloud drive managed by the college

## Introduction

Each of these systems has a particular system path
where files and directories for sensitive an irreplacable data is stored.

We use the unix utility `rsync` to transfer files between systems
and keep them in sync.
`rsync` is a powerful but complicated piece of software,
what this SOP describes only scratches the surface.

### Prerequisites

- You should have a user account on each lab computer (`ada` and `rosalind`)
  and be able to connect via SSH into both (ask Kevin if not)
- You should also be able to mount NTM ([see here](https://www.wellesley.edu/lts/techsupport/macs/ntmmac))

## Drive locations

### `rosalind`

`rosalind` is the large iMac in the lab, and has the following drives:

- `/Volumes/franklin`: external 8 Tb HDD drive (silver, with a G on the front)
- `/Volumes/elsie`: 10 Tb external HDD drive (black, stands upright)

### `ada`

`ada` is the linux desktop inside Vanja / Kevin's office in Simpson,
and has the following drives:

- `/lovelace`: external 8 Tb HDD drive (silver, with a G on the front)
- `/babbage`: external 2 Tb SSD drive (brown and black, very small)

### NTM

NTM is a 