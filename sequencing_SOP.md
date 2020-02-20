# Instructions for downloading and archiving sequencing files

When an e-mail comes in from the sequencing facility
indicating that raw seqences are ready,
the following steps need to be taken:

- [ ] [Download the `zip` file](#download-the-zip-file-from-dropbox-to-the-g-drive-attached-to-rosalind) from dropbox to the G-Drive attached to `rosalind`
  (the large Mac in the lab)
- [ ] [Document the contents of the `zip` file](#document-the-contents-of-the-zip-file)
  - [ ] [Create `batch###_details.txt`](#create-batch###_details.txt)
- [ ] [Extract the `zip` file into its own folder](#extract-the-zip-file-into-its-own-folder)
  - [ ] [Create `batch###_files.txt`](#create-batch###_files.txt)
  - [ ] [Create batch###.md5](#create-batch###.md5)
- [ ] [Verify that there are no conflicting file names](#verify-that-there-are-no-conflicting-file-names-in-echo-sequencing-mgx-rawfastq-or-echo-sequencing-16S-rawfastq) in
  `echo/sequencing/mgx/rawfastq` or `echo/sequencing/16S/rawfastq`
- [ ] Verify that all samples included in batch have correct
  number of files (usually 8 for mgx; 4 lanes each of fwd and rev)
- [ ] [Move files into `echo/sequencing/mgx/rawfastq`](#move-files-into-echo-sequencing-mgx-rawfastq)
- [ ] [Rsync files to lovelace and ntm](#rsync-files-to-lovelace-and-ntm)

**NOTE:** If the sequencing center sends multiple links for a sigle batch,
See bottom of this page for special notes.

## Download the `zip` file from dropbox to the G-Drive attached to `rosalind`

The sequencing facility will e-mail a dropbox link, such as
`https://www.dropbox.com/s/52468hvj9cmcizq/RowlandMetaG.zip?dl=1`.
The `?dl=1` at the end is important, if it's not there it can be added
(or if it's `?dl=0`, change it to `?dl=1`).

Log in or `ssh` into `rosalind` (the large Mac in the lab), and open the
terminal app. Start a new `tmux` session in your terminal window.

```sh
$ tmux new -s seq
# seq = [name of session]
# Re-attach to tmux session
$ tmux a -t seq
```

Change the directory to the echo sequencing folder.

```sh
$ cd /Volumes/franklin/echo/sequencing/
$ cd mgx
# (or 16S)
```

To download, use the `curl` program.
Replace the url below with the one provided by the sequencing facility.

NOTE: The file name below is just an example -
use something more specific to prevent overwriting previous sequences.
For batches split into multiple dropbox links, add `_#` to the file name,
as shown below.

```sh
$ curl -L -o nov2018_1.zip https://www.dropbox.com/s/52468hvj9cmcizq/RowlandMetaG.zip?dl=1
```

You will see the download status displayed below the command.

## Document the contents of the `zip` file

Once the zip file has downloaded, check the contents of the file.
The `unzip -l` command lists the contents of a zip archive,
including files, their size/time of creation etc.

```sh
$ unzip -l nov2018_1.zip
Archive:  nov2018_1.zip
  Length      Date    Time    Name
---------  ---------- -----   ----
        0  01-10-2019 15:05   RowlandMetaG/
107159560  01-10-2019 12:27   RowlandMetaG/C0047-7F-1A_S9_L001_R1_001.fastq.gz
108768855  01-10-2019 12:27   RowlandMetaG/C0047-7F-1A_S9_L001_R2_001.fastq.gz
104446740  01-10-2019 12:28   RowlandMetaG/C0047-7F-1A_S9_L002_R1_001.fastq.gz
106427050  01-10-2019 12:28   RowlandMetaG/C0047-7F-1A_S9_L002_R2_001.fastq.gz
#...
148792326  01-10-2019 14:12   RowlandMetaG/M0828-1F-1A_S62_L002_R2_001.fastq.gz
151575616  01-10-2019 14:06   RowlandMetaG/M0828-1F-1A_S62_L003_R1_001.fastq.gz
152547600  01-10-2019 14:06   RowlandMetaG/M0828-1F-1A_S62_L003_R2_001.fastq.gz
 48739518  01-10-2019 14:15   RowlandMetaG/M0828-1F-1A_S62_L004_R1_001.fastq.gz
150465356  01-10-2019 14:12   RowlandMetaG/M0828-1F-1A_S62_L004_R2_001.fastq.gz
---------                     -------
 7388142279                     529 files
```

Makes sure that the correct number of files is present -
there should be 8 files per `mgx` sample (4 lanes each of forward and reverse).
In this case, there were 66 samples, plus one "file" for the folder itself

`66 samples x 2 x 4 lanes + 1 == 529`

For `16S` samples, there should be 2 files per sample.

### Create `batch###_details.txt`

Save the information from this output into a file called `batch###_details.txt`,
where `###` is the batch number (eg `006`), and put it in the `details/` folder.

_Note: if 16S samples, label 16S_batch###. If files were split into multiple dropbox links, add "_#" to end of file name, as shown below.

```sh
$ unzip -l nov2018_1.zip > details/batch006_details_1.txt
```

## Extract the `zip` file into its own folder

Use `unzip` to extract the contents of the `.zip` file.
For example:

```sh
$ unzip nov2018.zip
```

This will take a while, and creates a new folder in the current working directory.
Once this is finished, check to make sure that all of the files were extracted.
The result of `ls -l RowlandMetaG` (or whatever folder they were extracted into),
especially at the end, should match the content of the `_details.txt` file.

_Note: folder names can be determined from the original dropbox link_

for example

```sh
$ tail -5 details/batch006_details.txt
mgx tail -5 details/batch006_details.txt
152547600  01-10-2019 14:06   RowlandMetaG/M0828-1F-1A_S62_L003_R2_001.fastq.gz
148739518  01-10-2019 14:15   RowlandMetaG/M0828-1F-1A_S62_L004_R1_001.fastq.gz
150465356  01-10-2019 14:12   RowlandMetaG/M0828-1F-1A_S62_L004_R2_001.fastq.gz
---------                     -------
77388142279                     529 files
$ ls RowlandMetaG/ | tail -5
ls -l  RowlandMetaG/ | tail -5
-rw-r--r--  1 kevin  staff  148792326 Jan 10 14:12 M0828-1F-1A_S62_L002_R2_001.fastq.gz
-rw-r--r--  1 kevin  staff  151575616 Jan 10 14:06 M0828-1F-1A_S62_L003_R1_001.fastq.gz
-rw-r--r--  1 kevin  staff  152547600 Jan 10 14:06 M0828-1F-1A_S62_L003_R2_001.fastq.gz
-rw-r--r--  1 kevin  staff  148739518 Jan 10 14:15 M0828-1F-1A_S62_L004_R1_001.fastq.gz
-rw-r--r--  1 kevin  staff  150465356 Jan 10 14:12 M0828-1F-1A_S62_L004_R2_001.fastq.gz
```

You can also test that you have the right number of files:

```sh
$ ls RowlandMetaG/*.fastq.gz | wc -l
    528
```

### Create `batch###_files.txt`

We also want a file that contains only the file names,
so if we ever need the files from a single batch in the future,
we can easily get them. For batches split into multiple folders, make only **one** `batch###_files.txt` for the batch.

_Note: if 16S samples, label 16S_batch###_

```sh
$ ls RowlandMetaG | grep fastq.gz
C0047-7F-1A_S9_L001_R1_001.fastq.gz
C0047-7F-1A_S9_L001_R2_001.fastq.gz
C0047-7F-1A_S9_L002_R1_001.fastq.gz
C0047-7F-1A_S9_L002_R2_001.fastq.gz
C0047-7F-1A_S9_L003_R1_001.fastq.gz
# ...
```

Now pipe the results of this command to a file called `batch###_files.txt`.

```sh
$ ls RowlandMetaG | grep fastq.gz >> batch006_files.txt
```

### Create batch###.md5

To reduce the overhead of checking the integrity of these files in the future,
we'll use `md5sum` which creates a hash of the file contents
(note - this may also take a while). Create a `.md5` for each part of a batch.

_Note: if 16S samples, label 16S_batch###_

```sh
$ cd RowlandMetaG_1
$ md5sum *.fastq.gz > ../details/batch006_1.md5
```

Which looks like this:

```sh
$ head -5 ../details/batch006_1.md5
66f8abc4a71d7ed945ae6a487629816c  C0047-7F-1A_S9_L001_R1_001.fastq.gz
68801cafb15d7993e7542e1c7e152147  C0047-7F-1A_S9_L001_R2_001.fastq.gz
292b2ed5a70872b536997bace2ed4b89  C0047-7F-1A_S9_L002_R1_001.fastq.gz
700784ef5544c13e5752cc4fdc8fb1cd  C0047-7F-1A_S9_L002_R2_001.fastq.gz
a98e4c376a5b5bcbaadb8a7fb5f3c3f5  C0047-7F-1A_S9_L003_R1_001.fastq.gz
```

## Verify that there are no conflicting file names in `echo/sequencing/mgx/rawfastq` or `echo/sequencing/16S/rawfastq`

```sh
$ cd ..
$ for f in $(cat batch006_files.txt); do if [ -f rawfastq/$f ]; then echo "$f already exists"; fi; done
```

## Move files into `echo/sequencing/mgx/rawfastq`

```sh
$ mv -nv RowlandMetaG/* rawfastq/
```

## Rsync files to lovelace and ntm

Assuming the lab drive on NTM is mounted and can ssh into `ada` from `rosalind`.

_Note: Run this in a tmux window_

```
# From rosalind, franklin to lovelace
$ rsync -avzP /Volumes/franklin/echo/sequencing/mgx/ ada:/lovelace/echo/sequencing/mgx/

# From rosalind, franklin to ntm
$ rsync -avzP /Volumes/franklin/echo/sequencing/mgx/ /Volumes/vkclab/echo/sequencing/mgx/
```

## Handling multiple dropbox links in a single batch.

If the sequencing center sends multiple links for a single batch

- each dropbox link should be downloaded to a separate zip file (append `_#.zip`)
- Each zip file should have a separate `_details_#.txt`
- The contents of each zip file should have their own md5 checksum (append `_#.md5`)
- All files from all of of the zip files can be added to a single `_files.txt`
  - be sure to use `>>` instead of `>` to append to the file instead of overwriting it.

## Handling other projects processed in the same batch

If the batch downloaded from the sequencing center is not exclusively ECHO samples

- Download all samples as described above and append `_all` to `.txt` and `.md5` files
- Separate files and move into new folder
  - Check the correct number of files transferred (# samples x 2)
- Remake `details.txt, files.txt, .md5` files

```sh
$ cd /Volumes/G-DRIVE\ USB-C/ECHO/sequencing/16S/RowlandMetaG
$ ls | grep -E "[MC]\d{4}-\d[EF]-\d\w" > files_to_move.txt
$ rsync --files-from=files_to_move.txt RowlandMetaG/ RowlandMetaG_ECHO/
$ ls RowlandMetaG/ | wc -l

$ ls -lh RowlandMetaG_ECHO/ > details/batch00#_details.txt
# resulting details.txt file looks different from other details.txt files
$ ls RowlandMetaG_ECHO/ | grep fastq.gz > batch00#_files.txt
$ cd RowlandMetaG_ECHO
$ md5sum *.fastq.gz > ../details/batch00#.md5
```

- Make new folder for all samples from the sequencing batch on NTM
- Copy all sequence files and connected files

```sh
$ cd /Volumes/vkclab/RawSequenceData
$ mkdir IMR_MMMYYYY_mgx
$ rsync -avzP /Volumes/G-DRIVE\ USB-C/ECHO/sequencing/mgx/RowlandMetaG/ IMR_MMMYYYY_mgx/
$ rsync -avzP /Volumes/G-DRIVE\ USB-C/ECHO/sequencing/16S/RowlandMetaG/details/batch00#_all_details.txt IMR_MMMYYYY_mgx/
$ rsync -avzP /Volumes/G-DRIVE\ USB-C/ECHO/sequencing/16S/RowlandMetaG/batch00#_all_files.txt IMR_MMMYYYY_mgx/
$ rsync -avzP /Volumes/G-DRIVE\ USB-C/ECHO/sequencing/16S/RowlandMetaG/details/batch00#_all.md5 IMR_MMMYYYY_mgx/
```

## Renaming files

If a file was assigned the incorrect `SampleID` when sent for sequencing, it is best to rename the file during documenting the sequencing files before rsyncing across drives.

```sh
$ for f in $(find RowlandMetaG_*/*.gz | grep 0604); do
    # this syntax says 'use variable f, but replace "0604" with "2018"'
    mv -v $f ${f/0604/2018};
  done
```
