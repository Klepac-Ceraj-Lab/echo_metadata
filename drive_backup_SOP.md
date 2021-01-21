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

NTM is a cloud drive managed by Wellesley.
The lab's drive is at `/BiologicalSciences/VKCLab/`

## Folder organization

Inside each of these drives, is the same set of directories and files
inside the `echo/` subdirectory.

```
 echo
├──  analysis
│  ├──  biobakery2
│  │  ├──  batch001
│  │  ├──  batch002
│  │  ├── ... # batch003-012
│  │  ├──  batch013
│  │  ├──  batches.csv
│  │  ├──  hr-files-batch1-4.txt
│  │  ├──  hr-files.txt
│  │  ├──  Manifest.toml
│  │  ├──  profile_audit.jl
│  │  ├──  Project.toml
│  │  └──  scratch.jl
│  ├──  biobakery3
│  │  ├──  batch001
│  │  ├──  batch002
│  │  ├── ...
│  │  └──  batch014
│  ├──  gutbrain
│  │  ├──  GBMs
│  │  └──  GBMs.zip
│  └──  bb3_missing.txt
├──  attic
│  ├──  databases
│  └──  sequencing
├──  databases
│  ├──  <Databases.fmp12>
│  ├──  master_fecal_samples.csv
│  └──  Recover.log
└──  sequencing
   ├──  16S
   │  ├──  batch001
   │  ├── ...
   │  └──  README.rtf
   ├──  its
   │  ├──  details
   │  ├──  rawfastq
   │  ├──  RowlandITS
   │  ├──  submissions
   │  ├──  its_batch001_files.txt
   │  └──  jan2020.zip
   ├──  mgx
   │  ├──  details
   │  ├──  rawfastq
   │  ├──  submissions
   │  ├──  batch001_files.txt
   │  ├──  batch002_files.txt
   │  ├──  batch003_files.txt
   │  ├──  batch004_files.txt
   │  ├──  batch005_files.txt
   │  ├──  batch006_files.txt
   │  ├──  batch007_files.txt
   │  ├──  batch008_files.txt
   │  ├──  batch009_files.txt
   │  ├──  batch010_files.txt
   │  ├──  batch011_files.txt
   │  ├──  batch012_files.txt
   │  ├──  batch013_files.txt
   │  ├──  batch013_zymo_files.txt
   │  ├──  batch014_files.txt
   │  ├──  batch014_urls.txt
   │  ├──  batch014_zymo_files.txt
   │  └──  README.rtf
   ├──  oral_mgx
   │  └──  rawfastq
   └──  scripts
      ├──  bin
      ├──  Manifest.toml
      └──  Project.toml
```

## Syncing between drives

In order to sync the drives, we will use the `rsync` commnad line tool.
This allows transferring accross systems using `ssh` protocol,
advanced file filtering directives,
and is intelligent about skipping files that have not been changed.

The basic structure of an `rsync` call is

```sh
$ rsync [options...] <source> <destination>
```

The most common options we will use are:
- `-a` or `--archive`,
  which recurses through directories and preserves file attributes
  (like permissions, modification times etc.)
- `-v` or `--verbose, which shows details of the transfer
- `-P` or `--partial --progress`, which makes it easier to pick up and resume
  if the transfer is interrupted
- `--dry-run`, which will show everything that will be transferred
  without actually going through with the transfer.
  This should almost always be used first to double check a new command.

Both the source and destination can be local (on your current system)
or remote (accessible by `ssh`) paths.
Local paths are specified like normal,
and remote paths are specified using `username@url:/path/goes/here`.

For example, to send files from your `~/Documents` folder on your local machine
to `/lovelace/kevin` on `ada`, you could write

```sh
$ rsync -avP ~/Documents/ username@vkclab-ada.wellesley.edu:/lovelace/kevin/
```

In addition, if you have entry in your `~/.ssh/config`
that specifies your username and url such as

```
Host ada
    Hostname vkclab-ada.wellesley.edu
    User yourname
    IdentityFile ~/.ssh/ada-key
```

then you can more simply write

```sh
$ rsync -avP ~/Documents/ ada:/lovelace/kevin/
```

Ask Kevin to get this set up if you don't already

### Backing up bewteen `lovelace` and `franklin`

The easiest way backup if you have access to the lab is to set up on `rosalind`.
Assuming you have `ssh` set up to connect to `ada` from rosalind`,
open a terminal and enter

```sh
$ rsync -avP /Volumes/franklin/echo/ ada:/lovelace/echo/ --dry-run
```

