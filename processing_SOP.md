# Processing fecal samples

## Picking up/receiving shipments of samples

Samples should be picked up from the Advanced Baby Imaging Lab in Providence, RI
(or be delivered by someone from the ABI lab) about once a month.

*Note*: From now on, 'fecal' = OMNIgene Gut tubes for DNA/RNA,
'ethanol' = collection tubes filled with pure ethanol for metabolimics

**Advanced Baby Imaging Lab address:** _555 Prospect St, Pawtucket, RI 02860_
(~1 hr drive from Wellesley College)

### What to bring there
- [ ] Large styrofoam box
  - [ ] Dry ice (available in blue cooler on 2nd floor of L-wing)
- [ ] USB drive

### What to bring back
- [ ] Large styrofoam box
  - [ ] Dry ice
  - [ ] Fecal samples
  - [ ] Ethanol samples
- [ ] USB drive
  - [ ] Latest deidentified version of FIleMaker Pro database (.fmp12 file)
  - [ ] Electronic copy of sample spreadsheets
- [ ] Fecal sample spreadsheet (with all samples included in this shipment)
- [ ] Ethanol sample spreadsheet (with all samples included in this shipment)

***

## Storing samples

All samples are stored in -80C freezer. When unloading a new shipment, all
samples should be taken out of any biomedical bags unless the sample tube
has broken. Any broken collection tubes should be disposed of in biohazardous
waste and noted in the database. To stay organized, place all samples in test
tube racks within the -80C freezer as temporary storage.

### Inputting new samples into database

(see **Updating databases** section below for more information about AirTable)

Open the digital copy of the sample spreadsheets (fecal and ethanol). Copy one
spreadsheet into a new spreadsheet to reformat the information to match what is
in AirTable.

#### What is given
**Fecal with Ethanol Shipment 6/14/2019**

| Participant ID | Date of Collection | Date in -80 | Date Shipped | Notes |
|----------------|--------------------|-------------|--------------|-------|
| SD34E          | 5/9/2019 CL        | 5/12/2019 CL| 6/14/2019    | Twins (32, 34) |
| SDM1044B       | 5/2/2019 LD        | 5/15/2019 CL| 6/14/2019    | 2 wks in fridge |

#### What you want

| SampleID | Mother_Child | SubjectID | MaternalID | TimePoint | Fecal_EtOH | DOC | RAInitials_DOC | DOF | RAInitials_DOF | Date_Brought_In | RAInitials_Brought | Date_Shipped | RAInitials_Shipped | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| C0034_5E_1A | C | 0034 | 0032_m | 5 | E | 2019-05-09 | CL | 2019-05-12 | CL |   |   | 2019-06-14 | SR | Twins (32, 34) |
| M1044_2E_1A | M | 1044 | 1044_m | 2 | E | 2019-05-02 | LD | 2019-05-15 | CL |   |   | 2019-06-14 | SR | 2 wks in fridge |

Use the `Inputting new samples` view to add new sample information into AirTable.

### Aliquoting fecal samples

All fecal samples (OMNIgene Gut tubes) should be aliquoted as these collection
tubes are not designed to be stored at -80C for long periods of time. These
samples are also aliquoted in order to set aside half of each sample for NIH.

#### Making sample labels

After inputting new samples into AirTable and generating sampleIDs
(ex: `M1020_2F_1A`), copy sampleIDs into `samplelabel_drafts.xlsx`. Drag all
sampleIDs across to fill 3 columns. Edit one column by find and replacing
"A" with "B".

You should now have 3 sample labels for each sample:
- `M1020_2F_1A`, `M1020_2F_1B` -> labels for aliquots
- `M1020_2F_1A` -> label for extracted DNA/RNA

Copy these rows of sample labels into the second sheet of the excel workbook,
formatted to fit multiple pages of labels. Each sample label page fits
5 columns x 17 rows of labels.

Then open `samplelabel_template.docx` and copy each page individually into the
template. Save each page of sample labels as `.docx` and `.pdf` file in the
format `YYYY-MM-DD_fecal_#`. Once ready, print each **pdf** file of labels onto
Tough Tags (Cat. No. ). Pages tend to stick to each other so load and print
each page one-by-one. Label each printed page of labels with marker with date
and #/total # pages.

```
# relevant file paths on Google Drive
My Drive/VKC LAB/Project & Personal Folders/ECHO Resonance/ECHO Sample Labels/
...
samplelabel_drafts.xlsx
samplelabel_template.docx
YYYYMMDD_fecal_#.docx
YYYYMMDD_fecal_#.pdf
```
***

#### Aliquoting Protocol
##### Materials
- [ ] 1000 ul pipette
- [ ] Wide orifice, filtered 1000 ul pipette tips
- [ ] Cryovials (1-2 ml)
- [ ] Cryoboxes
- [ ] Test tube rack
- [ ] Expandable small tube rack
- [ ] Marker
- [ ] Sample labels (see above)
- [ ] Biosafety cabinet
- [ ] 70% ethanol (for sterilization)
- [ ] Paper towels
- [ ] Small biohazardous waste bag
- [ ] Ice bucket
- [ ] Ice

1. Turn on blower and light in biosafety cabinet and open sash. Let air flow for
3-15 mins, depending on particular model.
2. Go to 'master_fecal_samples' base on AirTable. Open 'Master' table,
'Aliquoting' view. Filter by DOC such that only 16-32 samples are shown. Print
this page for keeping track of samples and for recording all the information in
your lab notebook.
3. With full ice bucket, take selected samples out of -80C freezer. Arrange in
same order as printed database page. Let defrost on ice.
4. Clean inside of biosafety cabinet with 70% ethanol. Place pipette, tips, bag of
cryovials, test tube rack, small tube rack, marker, and biohazardous waste bag in
biosafety cabinet, wiping each item with ethanol in order to sterilize.
5. Arrange two cryovials per sample in small tube rack. Label first of each pair
with SampleID.
6. Take samples out of ice and arrange in test tube rack.
7. Pipette 1000 ul of each sample into each cryovial.  
  - *Note:* this does not have to be exact. It is recommended to set pipette to
  900 ul to avoid fecal sample touching filter at top of pipette tip.
  - Make any notes about unusual samples or inconsistencies between the fecal
  sample database and the label on the OMNIgene Gut tube.
8. When all samples are aliquoted, put aliquots on ice. If all sample tube labels
have been double checked, they can be thrown in the biohazardous waste bag.
  - It is recommended to dispose of the biohazardous waste bag into a larger
  collection bag as it can be very smelly if left open on the bench top, even
  after being emptied.)
9. Remove all items from biosafety cabinet. Wipe down with 70% ethanol. Let vent
for 5 mins. Then close sash and turn off light and blower.
10. Label all aliquots. Put in appropriate cryoboxes, noting which boxes samples
are stored in. Store cryoboxes in the appropriate shelf in -80C freezer.
11. Update AirTable in 'Aliquoting' view with Date_Aliquoted, Number_Replicates, RAInitials_Aliquoted, StorageBox, and any Notes.

***

### Storing ethanol samples

Ethanol samples will be processed later for metabolimics. Until that processing
begins, samples should be stored in 4" tall cryoboxes with 16 dividers. Record
StorageBox of these ethanol samples so it is easy to find them for later analysis.
The 'Ethanol storage' view is useful for this task.

***

##  Extracting DNA/RNA

***

## Sending for sequencing

***

## Updating databases

The relevant database for fecal sample processing information is the
`master_fecal_samples` base on AirTable. The `Master` table, `Everything`
view is version controlled as `master_fecal_samples.csv` in the git repo
`echo_metadata` and synced across other lab storage drives
(see below for how to sync these and other files stored on these drives).

```
# relevant paths for database storage
# on personal computer, remote git repo
~/gitrepos/echo_metadata/
master_fecal_samples.csv

# lab storage drives
# "NTM"
rosalind:/Volumes/vkclab/echo/databases/
# "Franklin"
rosalind:/Volumes/franklin/ECHO/Databases/
# "Lovelace"
ada:/lovelace/echo/databases/
...
master_fecal_samples.csv
YYYYMMDD_deidentified.fmp12
# Note: identifiable version of FileMaker Pro database CANNOT be put on NTM
YYYYMMDD_identifiable.fmp12
```

***

### master_fecal_samples AirTable base overview
#### Tables
##### Master
###### Everything
###### Aliquoting
###### Extracting
###### Sequencing
###### Inputting new samples
###### Samples by SubjectID

##### Batch 1 Mapping
##### Mgx_batch Mapping
##### 16S_batch Mapping
##### Mother_Child Dyads

This table is to record the number of mother child pairs (or trios, in the case
of twins). This table is populated by using the

#### Blocks

###
