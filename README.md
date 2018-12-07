# echo_metadata

## Notes

- editing on AirTable
- export as UTF8 CSV
  - replace previous csv file in repo
- in atom, find/replace `\r\n` with `\n` (using regular expressions - the `.*` button)
  - doesn't seem to be a problem with AirTable
- sort by DOC, then SubjectID
- commit to GitHub at least once a week
- upload copy to ntm at least once a month

## Column Headers

- SampleID: unique ID associated with that specific sample, created by Sophie in the following form (N.B. may begin using alternate format when submitting to IMR in order to not include any metadata in sample name)
    - Mother_Child: M or C (M = mother, C = child)
    - SubjectID: #### specific to patient (see also MaternalID)
    - TimePoint: # converted from alphabetical (i.e. 1=A, 2=B, 3=C) to match main FileMakerPro database and not fecal sample label
    - Fecal_EtOH: F or E (F = fecal or Genotek collection tubes, E = ethanol collection tubes)
- MaternalID: usually identical to SubjectID, except in case of multiple births (i.e. twins, triplets) where the numerically first ID is chosen
- CollectionRep: 1 or 2, replicates of one stool sample collected in a separate collection tubes
- DOC: Date of collection
- DOF: Date of freeze, when moved to -80 freezer in Providence
- Date_Shipped: Date picked up from Providence
- Date_Aliquoted: Date when fecal samples were split into 2 1000 ul aliquots (A,B) or 4 500 ul aliquots (A,B,C,D) in cryovials and original Genotek tube was discarded
- StorageBox: (in -80)
    - For F, what box aliquots are in
    - For E, what box sample tubes are in
- DOE: Date of DNA extraction
- Extract_number: # of #, sample # of round # on that date
- AliquotRep: which aliquot was used for the extraction, default is "A" (even for E samples which aren't aliquoted)
- DNABox: (in -80) what box extracted DNA is in
- DNAConc: (if measured) in ng/ul, measured using Qubit
- DOM: Date of metagenome, when samples were sent to IMR for sequencing, corresponds to date on IMR sample submission sheet  
- DO16S: Date of 16S, same as DOM except for 16S sequencing
- Mgx_processed and 16S_processed:
    - Y = sent and sequenced
    - N = sent, not yet sequenced
    - changed to "Sequenced" and "Sent" in AirTable
- Mgx_batch: dictates which batch sample was sequenced in, batch is defined by separate 96-well plates
- Notes: miscellaneous comments on that sample
- Discrepancies: connects to separate table on AirTable, documents samples with identical SampleIDs or other questions/mistakes in the record accumulated through processing
