# Processing fecal samples

## Picking up/receiving shipments of samples

Samples should be picked up from the Advanced Baby Imaging Lab in Providence, RI
(or be delivered by someone from the ABI lab) about once a month.

*Note*: From now on, 'fecal' = OMNIgene Gut tubes for DNA/RNA,
'ethanol' = collection tubes filled with pure ethanol for metabolimics

##### Advanced Baby Imaging Lab address:

_555 Prospect St

Pawtucket, RI 02860_

~1 hr drive from Wellesley College

#### What to bring there
[]Large styrofoam box
- []Dry ice (available in blue cooler on 2nd floor of L-wing)
[]USB drive

#### What to bring back
[]Large styrofoam box
- []Dry ice
- []Fecal samples
- []Ethanol samples
[]USB drive
- []Latest deidentified version of FIleMaker Pro database (.fmp12 file)
- []Electronic copy of sample spreadsheets
[]Fecal sample spreadsheet (with all samples included in this shipment)
[]Ethanol sample spreadsheet (with all samples included in this shipment)

## Storing samples

All samples are stored in -80C freezer. When unloading a new shipment, all
samples should be taken out of any biomedical bags unless the sample tube
has broken. Any broken collection tubes should be disposed of in biohazardous
waste. To stay organized, place all samples in test tube racks within the -80C
freezer as temporary storage.

**Input new samples into database**

(see Updating databases section below)

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
each page one-by-one. Label each printed page of labels with Sharpie with date
and #/total # pages.

```
# relevant file paths on Google Drive
My Drive/VKC LAB/Project & Personal Folders/ECHO Resonance/ECHO Sample Labels/
...
samplelabel_drafts.xlsx
samplelabel_template.docx
YYYY-MM-DD_fecal_#.docx
YYYY-MM-DD_fecal_#.pdf
```

#### Aliquoting Protocol
##### Materials
[]1000 ul pipette
[]Wide orifice, filtered 1000 ul pipette tips
[]Cryovials (1-2 ml)
[]Cryobox
[]Test tube rack
[]Expandable small tube rack
[]Sharpie
[]Sample labels (see above)
[]Biosafety cabinet
[]70% ethanol (for sterilization)
[]Paper towels
[]Small biohazardous waste bag
[]Ice bucket
[]Ice

1. Turn on blower and light in biosafety cabinet and open sash. Let air flow for
3-15 mins, depending on particular model.
2. Go to 'master_fecal_samples' base on AirTable. Open 'Master' table, 'Aliquoting' view.
Filter


### Storing ethanol samples

##  Extracting DNA/RNA

## Sending for sequencing

## Updating databases

The relevant database for fecal sample processing information is the
'master_fecal_samples' base on AirTable. The 'Master' table, 'Everything'
view is version controlled as 'master_fecal_samples.csv' in the git repo
'echo_metadata'. 'master_fecal_samples.csv' is also saved in the
`echo/databases/` folder on `rosalind:/Volumes/vkclab/`,
`rosalind:/Volumes/franklin/`, and `ada:/lovelace/`
(see below for how to sync these and other files stored on these drives).

### master_fecal_samples base overview
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
