# Processing fecal samples

1. [Picking up samples] (#picking-up-shipments-of-samples)
2. [Storing samples] (#storing-samples)
  * [Aliquoting] (#aliquoting-fecal-samples)
3. [Extracting DNA/RNA] (#extracting-dna-rna)
4. [Sending for sequencing] (#sending-for-sequencing)
5. [Updating databases] (#updating-databases)

## Picking up shipments of samples

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

(see [Updating databases] (#updating-databases) section below for more information about AirTable)

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

| SampleID | Mother_Child | SubjectID | MaternalID | TimePoint | Fecal_EtOH | CollectionRep | DOC | RAInitials_DOC | DOF | RAInitials_DOF | Date_Brought_In | RAInitials_Brought | Date_Shipped | RAInitials_Shipped | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| C0034_5E_1A | C | 0034 | 0032_m | 5 | E | 1 | 2019-05-09 | CL | 2019-05-12 | CL |   |   | 2019-06-14 | SR | Twins (32, 34) |
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

### Materials
- [ ] RNeasy PowerMicrobiome Kit (*check expiration dates*)
  - [ ] PM1
  - [ ] IRS
  - [ ] PM3
  - [ ] PM4
  - [ ] PM5
  - [ ] Powerbead tubes
  - [ ] Spin filters
  - [ ] Elution tubes
- [ ] RB tubes
- [ ] Rotor adaptors
- [ ] Beta-mercaptoethanol (BME)
- [ ] 70% ethanol
- [ ] RNase Zap
- [ ] 15 ml conical tube
- [ ] 10 ml serological pipette
- [ ] Electronic pipettor
- [ ] 200 and 1000 ul pipettes
- [ ] Filtered 200 ul pipette tips
- [ ] Filtered 1000 ul pipette tips
- [ ] Wide orifice, filtered 1000 ul pipette tips
- [ ] Qiacube 1000 ul tips
- [ ] Qiacube reagent bottles
- [ ] Rotor adaptor tray
- [ ] Racks for tubes
  - [ ] Eppendorf tube racks (recommended: 4)
  - [ ] Expandable small tube rack
  - [ ] Rack for 15 ml tube
- [ ] Forceps
- [ ] Ice bucket
- [ ] Ice
- [ ] Biohazard bag
- [ ] Sharpie
- [ ] Bunsen burner
- [ ] Centrifuge
- [ ] Heat block
- [ ] PowerLyzer
- [ ] Centrifuge
- [ ] Qiacube Connect
- [ ] Chemical waste collection for BME

### Protocol
1. Turn on heat block (set to 55 degrees Celsius), centrifuge, PowerLyzer, and Qiacube
Connect. UV treat Qiacube Connect.
2. Sterilize work bench with ethanol and RNase Zap. Light bunsen burner. Remove samples from
-80 freezer and put on ice. Label top of aliquot tube with extraction number.
3. Make PM1 and beta-mercaptoethanol mixture in the 15 ml tube. Heat for 5-10 minutes
at 55 degrees Celsius.
 - *Note*: if running a full round (12 samples), use the following calculations.
 ```
 # n=12, 700 ul/n, 10 ul BME/990 ul PM1
 # BME: 84 ul
 # PM1: 8316 ul (8 ml + 316 ul)
 ```
4. While the mixture is heating, prep sample tubes (1 of each kind per sample).
Label top of tubes with number of the sample in the round (ie 1-12).
5. Load 650 ul of PM1/BME mixture into each Powerbead tube. Then add 200 ul of each sample using
wide orifice pipette tips to each respective Powerbead tube.
  - *Note*: if including a negative control, use 200 ul of PM1/BME instead of sample.
  - *Tip*: if fecal sample is stuck in pipette, pipette up and down a few times with
  PM1/BME mixture in individual Powerbead tube to loosen the sample.
6. Lyse samples on PowerLyzer 2500 speed for 45 seconds. Centrifuge for 1 minute at RT at
13,000 x g.
7. While samples are shaking and spinning, load rotor adaptors onto tray. Then place
spin columns and elution tubes into proper places on rotor adaptors (consult Qiacube for
  details).
8. Transfer 450 ul of supernatant to rotor adaptors as instructed by Qiacube.
9. Load samples and supplies into Qiacube as instructed on screen.
10. Run protocol on Qiacube Connect.
11. When round finished, empty waste drawer. Take out reagents and unused pipette tips.
Carefully remove elution tubes from rotor adaptors. Discard of other tubes and plastic ware
in chemical waste for beta-mercaptoethanol waste.
12. Label extracted DNA and RNA from samples and store in proper 2" cryobox in -80 freezer.
13. Record extraction progress and any notes from the protocol in Airtable (see [Updating databases] (#updating-databases)
below for more details).

***

## Sending for sequencing

### Materials
**For lab**
- [ ] 96-well plate (BioRad, HSP9601)
  - With diagram of sample location in wells
- [ ] Filtered 20 or 200 ul pipettes
- [ ] 20 or 200 ul pipette
- [ ] PCR hood
- [ ] Eppendorf tube rack
- [ ] Benchtop biohazard bag
- [ ] Ice bucket(s)
- [ ] Ice
- [ ] Eppendorf tube cap opener
- [ ] 96-well plate seal (BioRad, Microseal B, MSB-1001)
- [ ] Ziploc bag


**For shipping**
- [ ] Cardboard box
- [ ] Styrofoam box
- [ ] Ice pack(s)
- [ ] FedEx international waybill
- [ ] Commercial invoice
- [ ] Shipping label
- [ ] Sample submission sheet

### Loading samples
1. Fill out submission sheet.
  - *Note*: check Integrated Microbiome Resource's [submission guidelines] (https://imr.bio/submissions.html)
  for updated versions before filling out.
```$
# Submission sheet:
My Drive/VKC LAB/Sequencing Files/Sequencing Center Info/
...
IMRSampleSubmissionSheet_v11(LASTNAME_MMMDD).xlsx
```
2. Print out location of samples in the 96-well plate to refer to while loading.
3. Take out samples and put on ice in order of loading.
4. Turn on PCR hood and place new 96-well plate in work area. Put all other necessary
materials (pipette, tips, biohazard bag, rack, cap opener) in hood. UV for 10-20 mins.
5. Load samples 8 at a time, going down each column (ie A1, B1, C1, etc.). Check sampleIDs
and well location as you go.
  - *Note*: if you start with a new container of tips, you can double check your
  loading in the correct well because the tips come in the same arrangement of 8x12
  as a 96-well plate.
6. When finished, seal plate with clear adhesive seal. Run a thin tool (eg cap opener)
or fingernail between wells to ensure no cross-contamination occurs during travel.
7. Label side of plate with `VKC YourLastName MM/DD/YYYY Plate ##`
8. Put in plastic bag and label with the same information.
9. Put filled 96-well plate in -20 freezer until ready to ship.


### Shipping samples
1. Locate styrofoam box, a cardboard box the styrofoam fits into, and an ice
pack that fits into the styrofoam box with room for samples.
2. Fill out the Fed international waybill with the following address:
```
IMR (c/o Dr. André Comeau)
Dept. of Pharmacology, local 5-D
Tupper Medical Building
Dalhousie University
5850 College St.
Halifax NS B3H 4R2
```
3. Print 2 copies of address sheet and commercial invoice. Fill out date and sign
commercial invoice.
```
My Drive/VKC LAB/Sequencing Files/Sequencing Center Info/
...
IMR_commercialinvoice.pdf
IMR_ShippingLabel.pdf
```
4. Schedule FedEx pickup.
5. Pack ice pack and sample plate in styrofoam box. Place a copy of the shipping label
and commercial invoice inside cardboard box. Seal cardboard box with styrofoam
box inside. Tape shipping label to outside of box. Put international waybill and
commercial invoice in FedEx sleeve and attach to top of box.
6. Drop box off at Science Center FedEx pickup location.
  - *Note*: location is currently ground floor of Simpson East (as of Fall 2019).
7. Check that package is delivered successfully as it has sometimes gotten stuck at customs.


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
