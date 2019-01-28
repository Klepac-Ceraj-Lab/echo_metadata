# Discrepancies
### Recording changes made to SampleIDs of already processed samples

## Changes made on January 15, 2019
### Listed by original SampleID -> new SampleID
- C0604_1F_1A -> C2018_1F_1A
    - SubjectID and MaternalID changed to 2018
- C0604_1F_2A -> C2018_1F_2A
    - SubjectID and MaternalID changed to 2018
#### Tried to make later sample CollectionRep 2 or limit number of SampleIDs changed
- C0551_2F_1A -> C0551_1F_2A
    - TimePoint changed to 1
    - CollectionRep changed to 2
- M0742_2F_1A -> M0742_2F_2A
    - CollectionRep changed to 2
    - DOC: 7/17/18
    - 96-well position: A5
- M0742_2E_1A -> M0742_2E_2A
    - CollectionRep changed to 2
    - DOC: 2018-07-17
    - 96-well position: C9
- C0499_1F_1A -> C0499_1F_2A
    - CollectionRep changed to 2
    - DOC: 2017-09-06
- C0511_1F_1A -> C0511_1F_2A
    - CollectionRep changed to 2
    - DOC: 2017-09-19
- C0066_6F_1A -> C0066_6F_2A
    - CollectionRep changed to 2
    - DOC: 2017-09-21
- C0516_1F_1A -> C0516_1F_2A
    - CollectionRep changed to 2
    - DOC: 2017-09-26
- C0229_4F_1A -> C0229_4F_2A
    - CollectionRep changed to 2
    - DOC: 2017-10-07
- C0550_1F_1A -> C0550_1F_2A
    - CollectionRep changed to 2
    - DOC: 2018-01-30
- C0634_1E_1A -> C0634_1E_2A
    - CollectionRep changed to 2
    - DOC: 2018-03-16
- C0634_1F_1A -> C0634_1F_3A
    - CollectionRep changed to 3
    - DOC: 2018-03-16

### Fixes for previous samples

Because many SampleIDs have changed since sequencing,
and downstream analyses are based on SampleIDs contained in file names,
many file names needed to be changed.
It seemed best to start with the raw sequencing `.fastq.gz` files
found in the lab G-Drive `/Volumes/G-DRIVE\ USB-C/ECHO/sequencing/mgx`.

The raw sequencing files are found in the following subdirectories:

- `IMR_May2018`
- `IMR_Sep2018`
- `IMR_Oct2018`
- `IMR_Nov2018`

and with `mgx/` as our current working directory,
we can get the path to all `fastq.gz` files with `find`,
and then narrow what we're searching for with `grep`

```sh
$ find IMR*/*.gz
IMR_May2018/C016-3F-1A_S94_L001_R1_001.fastq.gz
IMR_May2018/C016-3F-1A_S94_L001_R2_001.fastq.gz
IMR_May2018/C016-3F-1A_S94_L002_R1_001.fastq.gz
#...

$ find IMR_*/*.gz | grep 604-1F-1A
IMR_May2018/C604-1F-1A_S5_L001_R1_001.fastq.gz
IMR_May2018/C604-1F-1A_S5_L001_R2_001.fastq.gz
IMR_May2018/C604-1F-1A_S5_L002_R1_001.fastq.gz
IMR_May2018/C604-1F-1A_S5_L002_R2_001.fastq.gz
IMR_May2018/C604-1F-1A_S5_L003_R1_001.fastq.gz
IMR_May2018/C604-1F-1A_S5_L003_R2_001.fastq.gz
IMR_May2018/C604-1F-1A_S5_L004_R1_001.fastq.gz
IMR_May2018/C604-1F-1A_S5_L004_R2_001.fastq.gz
```

#### Leading Zeros

But first, we need to fix samples that have 3 digit IDs instead of 4 digit IDs
with leading zeros.
To do this would require some arcane shell scripting, so I'll use julia instead.
From the `mgx/` directory:

```julia
function expand_id(file)
    # use regex to match the id portion of the file name
    m = match(r"^(C|M)(\d+)(-\d[A-Z]-\d[A-Z].+)", file)
    if m === nothing
        # this means that the file doesn't have the expected pattern
        return file
    else
        # left pad 4 spaces with zeros
        i = lpad(string(m.captures[2]), 4, 0)
        # concatenate string pieces
        return m.captures[1] * i * m.captures[3]
    end
end


for d in [d for d in readdir() if startswith(d, "IMR_")]
    for f in readdir(d)
        new_f = expand_id(f)
        f != new_f && mv(joinpath(d, f), joinpath(d, new_f))
    end
end
```

Now:

```sh
$ find IMR_*/*.gz | grep 604-1F-1A
IMR_May2018/C0604-1F-1A_S5_L001_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L001_R2_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L002_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L002_R2_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L003_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L003_R2_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L004_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L004_R2_001.fastq.gz
```

#### Renaming fixes

Now we have to rename files based on changes to SampleIDs.
I'll do this with `find`, `grep` and a for loop.

For example, to change

- **`C0604_1F_1A` -> `C2018_1F_1A`**
- **`C0604_1F_2A` -> `C2018_1F_2A`**

```sh
$ for f in $(find IMR_*/*.gz | grep 0604); do
    # this syntax says 'use variable f, but replace "0604" with "2018"'
    mv -v $f ${f/0604/2018}
  done
```
```
IMR_May2018/C0604-1F-1A_S5_L001_R1_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L001_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L001_R2_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L001_R2_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L002_R1_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L002_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L002_R2_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L002_R2_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L003_R1_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L003_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L003_R2_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L003_R2_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L004_R1_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L004_R1_001.fastq.gz
IMR_May2018/C0604-1F-1A_S5_L004_R2_001.fastq.gz -> IMR_May2018/C2018-1F-1A_S5_L004_R2_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L001_R1_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L001_R1_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L001_R2_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L001_R2_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L002_R1_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L002_R1_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L002_R2_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L002_R2_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L003_R1_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L003_R1_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L003_R2_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L003_R2_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L004_R1_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L004_R1_001.fastq.gz
IMR_May2018/C0604-1F-2A_S17_L004_R2_001.fastq.gz -> IMR_May2018/C2018-1F-2A_S17_L004_R2_001.fastq.gz
```

**C0551_2F_1A -> C0551_1F_2A**

```sh
$ for f in $(find IMR_*/*.gz | grep 0551-2F); do
    mv -v $f ${f/0551-2F-1A/0551-1F-2A}
  done
```
```
IMR_May2018/C0551-2F-1A_S40_L001_R1_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L001_R1_001.fastq.gz
IMR_May2018/C0551-2F-1A_S40_L001_R2_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L001_R2_001.fastq.gz
IMR_May2018/C0551-2F-1A_S40_L002_R1_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L002_R1_001.fastq.gz
IMR_May2018/C0551-2F-1A_S40_L002_R2_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L002_R2_001.fastq.gz
IMR_May2018/C0551-2F-1A_S40_L003_R1_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L003_R1_001.fastq.gz
IMR_May2018/C0551-2F-1A_S40_L003_R2_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L003_R2_001.fastq.gz
IMR_May2018/C0551-2F-1A_S40_L004_R1_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L004_R1_001.fastq.gz
IMR_May2018/C0551-2F-1A_S40_L004_R2_001.fastq.gz -> IMR_May2018/C0551-1F-2A_S40_L004_R2_001.fastq.gz
```

- **M0742_2F_1A -> M0742_2F_2A**
    - 96-well position: A5
    - S13

```sh
$ for f in $(find rawfastq/*.gz | grep 0742| grep S13); do
    mv -v $f ${f/-2F-1A/-2F-2A}
  done
```
```
rawfastq/M0742-2F-1A_S13_L001_R1_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L001_R1_001.fastq.gz
rawfastq/M0742-2F-1A_S13_L001_R2_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L001_R2_001.fastq.gz
rawfastq/M0742-2F-1A_S13_L002_R1_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L002_R1_001.fastq.gz
rawfastq/M0742-2F-1A_S13_L002_R2_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L002_R2_001.fastq.gz
rawfastq/M0742-2F-1A_S13_L003_R1_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L003_R1_001.fastq.gz
rawfastq/M0742-2F-1A_S13_L003_R2_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L003_R2_001.fastq.gz
rawfastq/M0742-2F-1A_S13_L004_R1_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L004_R1_001.fastq.gz
rawfastq/M0742-2F-1A_S13_L004_R2_001.fastq.gz -> rawfastq/M0742-2F-2A_S13_L004_R2_001.fastq.gz
```


- **M0742_2E_1A -> M0742_2E_2A**
    - 96-well position: C9
    - S33

```sh
$ for f in $(find rawfastq/*.gz | grep 0742-2E-1A | grep S33); do
    mv -v $f ${f/2E-1A/2E-2A}
  done
```

```
rawfastq/M0742-2E-1A-C9_S33_L001_R1_001.fastq.gz -> rawfastq/M0742-2E-2A-C9_S33_L001_R1_001.fastq.gz
rawfastq/M0742-2E-1A-C9_S33_L001_R2_001.fastq.gz -> rawfastq/M0742-2E-2A-C9_S33_L001_R2_001.fastq.gz
rawfastq/M0742-2E-1A-C9_S33_L002_R1_001.fastq.gz -> rawfastq/M0742-2E-2A-C9_S33_L002_R1_001.fastq.gz
rawfastq/M0742-2E-1A-C9_S33_L002_R2_001.fastq.gz -> rawfastq/M0742-2E-2A-C9_S33_L002_R2_001.fastq.gz
rawfastq/M0742-2E-1A-C9_S33_L003_R1_001.fastq.gz -> rawfastq/M0742-2E-2A-C9_S33_L003_R1_001.fastq.gz
rawfastq/M0742-2E-1A-C9_S33_L003_R2_001.fastq.gz -> rawfastq/M0742-2E-2A-C9_S33_L003_R2_001.fastq.gz
rawfastq/M0742-2E-1A-C9_S33_L004_R1_001.fastq.gz -> rawfastq/M0742-2E-2A-C9_S33_L004_R1_001.fastq.gz
```

The following are all from batch 1, which came before we changed naming conventions
and will need different handling.

- C0499_1F_1A -> C0499_1F_2A
- C0511_1F_1A -> C0511_1F_2A
- C0066_6F_1A -> C0066_6F_2A
- C0516_1F_1A -> C0516_1F_2A
- C0229_4F_1A -> C0229_4F_2A

**C0550_1F_1A -> C0550_1F_2A**

This one was in the same batch, and got resolved as
`C0550-1F-1A_S28` and `C0550-1F-1A-dup_S41`.
The one with `dup` is supposed to be 2A based on the well number, so

```sh
$ for f in $(find IMR_*/*.gz | grep 0550 | grep dup); do
    mv -v $f ${f/1A-dup/2A}
  done
```
```
IMR_May2018/C0550-1F-1A-dup_S41_L001_R1_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L001_R1_001.fastq.gz
IMR_May2018/C0550-1F-1A-dup_S41_L001_R2_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L001_R2_001.fastq.gz
IMR_May2018/C0550-1F-1A-dup_S41_L002_R1_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L002_R1_001.fastq.gz
IMR_May2018/C0550-1F-1A-dup_S41_L002_R2_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L002_R2_001.fastq.gz
IMR_May2018/C0550-1F-1A-dup_S41_L003_R1_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L003_R1_001.fastq.gz
IMR_May2018/C0550-1F-1A-dup_S41_L003_R2_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L003_R2_001.fastq.gz
IMR_May2018/C0550-1F-1A-dup_S41_L004_R1_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L004_R1_001.fastq.gz
IMR_May2018/C0550-1F-1A-dup_S41_L004_R2_001.fastq.gz -> IMR_May2018/C0550-1F-2A_S41_L004_R2_001.fastq.gz
```

**C0634_1E_1A -> C0634_1E_2A**

This is the one in `batch002` (sent in May),

```sh
$ for f in $(find IMR_*/*.gz | grep 0634-1E-1A | grep May); do
    mv -v $f ${f/1E-1A/1E-2A}
  done
```
```
IMR_May2018/C0634-1E-1A_S46_L001_R1_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L001_R1_001.fastq.gz
IMR_May2018/C0634-1E-1A_S46_L001_R2_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L001_R2_001.fastq.gz
IMR_May2018/C0634-1E-1A_S46_L002_R1_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L002_R1_001.fastq.gz
IMR_May2018/C0634-1E-1A_S46_L002_R2_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L002_R2_001.fastq.gz
IMR_May2018/C0634-1E-1A_S46_L003_R1_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L003_R1_001.fastq.gz
IMR_May2018/C0634-1E-1A_S46_L003_R2_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L003_R2_001.fastq.gz
IMR_May2018/C0634-1E-1A_S46_L004_R1_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L004_R1_001.fastq.gz
IMR_May2018/C0634-1E-1A_S46_L004_R2_001.fastq.gz -> IMR_May2018/C0634-1E-2A_S46_L004_R2_001.fastq.gz
```

**C0634_1F_1A -> C0634_1F_3A**

Based on the well numbers for the sequencing plate, this is the one labeled S33

```sh
$ for f in $(find IMR_*/*.gz | grep 0634-1F-1A | grep S33); do
    mv -v $f ${f/1F-1A/1F-3A}
  done
```
```
IMR_May2018/C0634-1F-1A_S33_L001_R1_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L001_R1_001.fastq.gz
IMR_May2018/C0634-1F-1A_S33_L001_R2_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L001_R2_001.fastq.gz
IMR_May2018/C0634-1F-1A_S33_L002_R1_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L002_R1_001.fastq.gz
IMR_May2018/C0634-1F-1A_S33_L002_R2_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L002_R2_001.fastq.gz
IMR_May2018/C0634-1F-1A_S33_L003_R1_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L003_R1_001.fastq.gz
IMR_May2018/C0634-1F-1A_S33_L003_R2_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L003_R2_001.fastq.gz
IMR_May2018/C0634-1F-1A_S33_L004_R1_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L004_R1_001.fastq.gz
IMR_May2018/C0634-1F-1A_S33_L004_R2_001.fastq.gz -> IMR_May2018/C0634-1F-3A_S33_L004_R2_001.fastq.gz
```
