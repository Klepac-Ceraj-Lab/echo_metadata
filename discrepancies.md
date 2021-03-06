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

- Remove well number from file names M0742_2E_1A and M0742_2E_2A

```sh
for f in $(find sequencing/mgx/rawfastq | grep 742 | grep 2A-C9); do mv -vn $f ${f/2A-C9/2A}; done
```

```
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L001_R1_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L001_R1_001.fastq.gz
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L001_R2_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L001_R2_001.fastq.gz
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L002_R1_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L002_R1_001.fastq.gz
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L002_R2_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L002_R2_001.fastq.gz
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L003_R1_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L003_R1_001.fastq.gz
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L003_R2_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L003_R2_001.fastq.gz
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L004_R1_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L004_R1_001.fastq.gz
sequencing/mgx/rawfastq/M0742-2E-2A-C9_S33_L004_R2_001.fastq.gz -> sequencing/mgx/rawfastq/M0742-2E-2A_S33_L004_R2_001.fastq.gz
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

#### Files changed during renaming

- Some files were overwritten when changing file names

1. M0742_2F_1A_S33 (all lanes)
  * batch006
  * pull from sequencing center
2. M0742_2E_1A_S7 (just lane 1 L001)
  * batch005
  * found on Harvard server

```sh
$ rsync -avzP --exclude='*.zip' mgx /Volumes/vkclab/ECHO/sequencing/

```  

## Changes made on Sept 18, 2019

C1008-3F-1A -> C1008-4F-1A
* Changing from timepoint 3 to timepoint 4
* file in `batch011`

```sh
$ cd mgx/

# Find the files
$ find RowlandRunNS43*/*.fastq.gz | grep C1008-3F-1A

RowlandRunNS43_part3/C1008-3F-1A_S95_L001_R2_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R1_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R2_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R1_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R2_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R1_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R2_001.fastq.gz

# Test code
$ for f in $(find RowlandRunNS43*/*.gz | grep C1008-3F-1A); do
    echo "mv -v $f ${f/-3F-1A/-4F-1A}"
  done

  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L001_R1_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L001_R1_001.fastq.gz
  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L001_R2_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L001_R2_001.fastq.gz
  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R1_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L002_R1_001.fastq.gz
  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R2_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L002_R2_001.fastq.gz
  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R1_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L003_R1_001.fastq.gz
  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R2_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L003_R2_001.fastq.gz
  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R1_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L004_R1_001.fastq.gz
  mv -v RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R2_001.fastq.gz RowlandRunNS43_part3/C1008-4F-1A_S95_L004_R2_001.fastq.gz

#Run code
$ for f in $(find RowlandRunNS43*/*.gz | grep C1008-3F-1A); do
    mv -v $f ${f/-3F-1A/-4F-1A}
  done

  RowlandRunNS43_part3/C1008-3F-1A_S95_L001_R1_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L001_R1_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L001_R2_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L001_R2_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R1_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L002_R1_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R2_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L002_R2_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R1_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L003_R1_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R2_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L003_R2_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R1_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L004_R1_001.fastq.gz
RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R2_001.fastq.gz -> RowlandRunNS43_part3/C1008-4F-1A_S95_L004_R2_001.fastq.gz

# Correction made upstream of `files.txt` and `.md5` files
# but downstream of `batch011_details_3.txt`

# Correcting batch011_details_3.txt
$ sed -Ei.bak 's/C1008-3F-1A/C1008-4F-1A/' batch011_details_3.txt

diff batch011_details_3.txt batch011_details_3.txt.bak
13,20c13,20
<  80491004  09-16-2019 16:23   RowlandRunNS43_part3/C1008-4F-1A_S95_L001_R1_001.fastq.gz
<  82367890  09-16-2019 16:24   RowlandRunNS43_part3/C1008-4F-1A_S95_L001_R2_001.fastq.gz
<  79960916  09-16-2019 16:23   RowlandRunNS43_part3/C1008-4F-1A_S95_L002_R1_001.fastq.gz
<  81660917  09-16-2019 16:23   RowlandRunNS43_part3/C1008-4F-1A_S95_L002_R2_001.fastq.gz
<  82320735  09-16-2019 16:24   RowlandRunNS43_part3/C1008-4F-1A_S95_L003_R1_001.fastq.gz
<  84123317  09-16-2019 16:24   RowlandRunNS43_part3/C1008-4F-1A_S95_L003_R2_001.fastq.gz
<  79820352  09-16-2019 16:23   RowlandRunNS43_part3/C1008-4F-1A_S95_L004_R1_001.fastq.gz
<  81716033  09-16-2019 16:23   RowlandRunNS43_part3/C1008-4F-1A_S95_L004_R2_001.fastq.gz
---
>  80491004  09-16-2019 16:23   RowlandRunNS43_part3/C1008-3F-1A_S95_L001_R1_001.fastq.gz
>  82367890  09-16-2019 16:24   RowlandRunNS43_part3/C1008-3F-1A_S95_L001_R2_001.fastq.gz
>  79960916  09-16-2019 16:23   RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R1_001.fastq.gz
>  81660917  09-16-2019 16:23   RowlandRunNS43_part3/C1008-3F-1A_S95_L002_R2_001.fastq.gz
>  82320735  09-16-2019 16:24   RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R1_001.fastq.gz
>  84123317  09-16-2019 16:24   RowlandRunNS43_part3/C1008-3F-1A_S95_L003_R2_001.fastq.gz
>  79820352  09-16-2019 16:23   RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R1_001.fastq.gz
>  81716033  09-16-2019 16:23   RowlandRunNS43_part3/C1008-3F-1A_S95_L004_R2_001.fastq.gz
```
