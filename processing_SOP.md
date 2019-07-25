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

All samples are stored in -80C freezer. When unloading a new shipment, all samples
should be taken out of any biomedical bags unless the sample tube has broken.
Any broken collection tubes should be disposed of in biohazardous waste. To stay
organized, place all samples in test tube racks within the -80C freezer as temporary storage.

### Aliquoting fecal samples

All fecal samples (OMNIgene Gut tubes) should be aliquoted as these collection tubes
are not designed to be stored at -80C for long periods of time. These samples are also
aliquoted in order to set aside half of each sample for NIH.

#### Making sample labels


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

The relevant database for fecal sample processing information is the 'master_fecal_samples'
base on AirTable. The 'Master' table, 'Everything' view is version controlled as 'master_fecal_samples.csv'
in the git repo 'echo_metadata'. 'master_fecal_samples.csv' is also saved in the `echo/databases/` folder on
`rosalind:/Volumes/vkclab/`, `rosalind:/Volumes/franklin/`, and `ada:/lovelace/`
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

This table is to record the number of mother child pairs (or trios, in the case of
twins). This table is populated by using the

#### Blocks

### 
