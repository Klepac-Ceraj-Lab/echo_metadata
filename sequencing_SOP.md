# Instructions for downloading and archiving sequencing files

When an e-mail comes in from the sequencing facility
indicating that raw seqences are ready,
the following steps need to be taken:

- [ ] Download the `zip` file from dropbox to the G-Drive attached to `rosalind`
  (the large Mac in the lab)
- [ ] Document the contents of the `zip` file.
  - [ ] Create `batch###_details.txt`
  - [ ] Create `batch###_files.txt`
- [ ] Extract the `zip` file into its own folder
  - [ ] Create batch###.md5
- [ ] Verify that there are no conflicting file names in
  `ECHO/sequencing/mgx/rawfastq`
- [ ] Verify that all samples included in batch have correct
  number of files (usually 8; 4 lanes each of fwd and rev)
- [ ] Move files into `ECHO/sequencing/mgx/rawfastq`

## Download the `zip` file from dropbox to the G-Drive attached to `rosalind`

The sequencing facility will e-mail a dropbox link, such as
`https://www.dropbox.com/s/52468hvj9cmcizq/RowlandMetaG.zip?dl=1`.
The `?dl=1` at the end is important, if it's not there it can be added
(or if it's `?dl=0`, change it to `?dl=1`).

Log in to `rosalind` (the large Mac in the lab), and open the terminal app.
Change the directory to the echo sequencing folder.

```sh
$ cd /Volumes/G-DRIVE\ USB-C/ECHO/sequencing/
$ cd mgx # (or 16S)
```

To download, use the `curl` program.
Replace the url below with the one provided by the sequencing facility.

NOTE: The file name below is just an example -
use something more specific to prevent overwriting previous sequences.

```sh
$ curl -L -o nov2018.zip https://www.dropbox.com/s/52468hvj9cmcizq/RowlandMetaG.zip?dl=1
```

You will see the download status displayed below the command.

## Document the contents of the `zip` file

Once the zip file has downloaded, check the contents of the file.
The `unzip -l` command lists the contents of a zip archive,
including files, their size/time of creation etc.

```sh
$ unzip -l nov2018.zip
Archive:  nov2018.zip
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
there should be 8 samples per sample (4 lanes each of forward and reverse).
In this case, there were 66 samples, plus one "file" for the folder itself

`66 samples x 2 x 4 lanes + 1 == 529`

### Create `batch###_details.txt`

Save the information from this output into a file called `batch###_details.txt`,
where `###` is the batch number (eg `006`), and put it in the `details/` folder.

```sh
$ unzip -l nov2018.zip > details/batch006_details.txt
```

### Create `batch###_files.txt`

We also want a file that contains only the file names,
so if we ever need the files from a single batch in the future,
we can easily get them.
To do this, we'll use `sed` and some regular expressions.

```sh
$ cat details/batch006_details.txt | grep fastq | sed -E "s/.+RowlandMetaG\/([^\]+\.fastq.gz)/\1/"
C0047-7F-1A_S9_L001_R1_001.fastq.gz
C0047-7F-1A_S9_L001_R2_001.fastq.gz
C0047-7F-1A_S9_L002_R1_001.fastq.gz
C0047-7F-1A_S9_L002_R2_001.fastq.gz
C0047-7F-1A_S9_L003_R1_001.fastq.gz
# ...
```

If the folder inside the `zip` file is something other than `RowlandMetaG`,
be sure to replace that part of the regular expression.

Now pipe the results of this command to a file called `batch###_files.txt`.

```sh
$ cat details/batch006_details.txt | grep fastq | sed -E "s/.+RowlandMetaG\/([^\]+\.fastq.gz)/\1/" > batch006_files.txt
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
$ ls RowlandMetaG | wc -l
    528
```

### Create batch###.md5

To reduce the overhead of checking the integrity of these files in the future,
we'll use `md5sum` which creates a hash of the file contents
(note - this may also take a while).

```sh
$ cd RowlandMetaG
$ md5sum * > ../details/batch006.md5
```

Which looks like this:

```sh
$ head -5 ../details/batch006.md5
66f8abc4a71d7ed945ae6a487629816c  C0047-7F-1A_S9_L001_R1_001.fastq.gz
68801cafb15d7993e7542e1c7e152147  C0047-7F-1A_S9_L001_R2_001.fastq.gz
292b2ed5a70872b536997bace2ed4b89  C0047-7F-1A_S9_L002_R1_001.fastq.gz
700784ef5544c13e5752cc4fdc8fb1cd  C0047-7F-1A_S9_L002_R2_001.fastq.gz
a98e4c376a5b5bcbaadb8a7fb5f3c3f5  C0047-7F-1A_S9_L003_R1_001.fastq.gz
```

## Verify that there are no conflicting file names in `ECHO/sequencing/mgx/rawfastq`

```sh
$ cd ..
$ for f in $(ls RowlandMetaG); do if [ -f rawfastq/$f ]; then echo "$f already exists!"; fi; done
```

## Move files into `ECHO/sequencing/mgx/rawfastq`

```sh
$ mv -nv RowlandMetaG/* rawfastq/
```
