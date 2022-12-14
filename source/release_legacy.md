
# Release Legacy Steps 

These steps may be useful for reference. 

## Generate annotated SEED files

Export SEED files from SVN using generate_ftp_files.py:

  - to export all files (recommended 16GB RAM to checkout large families):
```console
python generate_ftp_files.py --acc all --seed --dest-dir /path/to/seed/files/dest/dir
```
  - to export accessions listed in a file

```console
python generate_ftp_files.py -f /path/to/rfam_accession_list.txt --seed --dest-dir /path/to/seed/files/dest/dir
```

- Alternatively, use writeAnnotatedSeed.pl:

```console
perl writeAnnotatedSeed.pl RFXXXXX
```

## Generate annotated CM files

Export plain CM files using generate_ftp_files.py:

- to export all files (recommended 16GB RAM to checkout large families) 
```console
python generate_ftp_files.py --acc all --cm --dest-dir /path/to/seed/files/dest/dir
```

- to export accessions listed in a file python 
```console
generate_ftp_files.py -f /path/to/rfam_accession_list.txt --cm --dest-dir /path/to/CM/files/dest/dir
```

- alternatively use writeAnnotatedCM.pl:

```console
perl writeAnnotatedCM.pl RFXXXXX 
```   

- Rewrite CM file and descriptions from SEED using seed-desc-to-cm.pl:

Required files:
- $CM_no_desc - a CM file to add DESC to (could be 1 CM or all CMs in a single file)
- $SEED_with_DESC - a seed file with DESC lines (could be 1 seed or all seeds in a single file)


```console
filter out DESC lines to avoid duplicates as some CMs already have DESC lines
grep -v DESC $CM_no_desc > Rfam.nodesc.cm
perl seed-desc-to-cm.pl $SEED_with_DESC Rfam.nodesc.cm > Rfam.cm

check that Rfam.cm contains the correct number of families
cmstat Rfam.cm | grep -v '#' | wc -l

check the number of DESC lines - should be 2 * number of families
grep DESC Rfam.cm | wc -l

generate the final archive
gzip -c Rfam.cm > Rfam.cm.gz 

split annotated Rfam.cm into individual .cm files and create Rfam.tar.gz:

delete any existing files
rm -f RF0*.cm

get a list of Rfam IDs found in Rfam.cm
grep ACC Rfam.cm | sed -e 's/ACC\s+//g' | sort | uniq > list.txt

create an index file to speed up cm retrieval
cmfetch --index Rfam.cm

create individual cm files
while read p; do echo $p; cmfetch Rfam.cm $p > "$p.cm" ; done <list.txt

create an archive
tar -czvf Rfam.tar.gz RF0*.cm

remove temporary files
rm RF0*.cm 
```

The CM file is used for PDB mapping. The SEED and CM files are stored in FTP archive and are also available from the Rfam website.

## Load annotated SEED and CM files into rfam_live

This enables the SEED and CM download directly from the Rfam website.
Requires new Rfam.cm and Rfam.seed files.

Use load_cm_seed_in_db.py:

```console
python load_cm_seed_in_db.py /path/to/Rfam.seed /path/to/Rfam.cm
```

Alternatively, use populateAnnotatedFiles.pl to process families one by one:

```console
perl populateAnnotatedFiles.pl RFXXXXX /path/to/RFXXXXX.cm /path/to/RFXXXXX.seed
```

## Clan competition

Clan competition is a quality assurance measure ran as a pre-processing release step aiming to reduce redundant hits of families belonging to the same clan.

1. Create a destination directory for clan competition required files

```console
mkdir ~/releaseX/clan_competition     mkdir ~/releaseX/clan_competition/sorted 
```       
 
2. Generate clan files using clan_file_generator.py:

-  recommended 16GB RAM     
```console
python clan_file_generator.py --dest-dir ~/releaseX/clan_competition --clan-acc CL00001 --cc-type FULL
```  

--cc-type: Clan competition type FULL/PDB

--clan-acc: Clan accession to compete

--all: Compete all Rfam clans

-f: Only compete clans in file

3. Sort clan files in dest-dir based on rfamseq_acc (col2) using linux sort command and store then in the sorted directory:

```console
sort -k2 -t $'\t' clan_file.txt > ~/releaseX/clan_competition/sorted/clan_file_sorted.txt
```  

or for multiple files cd ~/releaseX/clan_competition and run:

```console
for file in ./CL*; do sort -k2 -t $'\t' ${file:2:7}.txt > sorted/${file:2:7}_s.txt; done 
```

4. Run clan competition using clan_competition.py:

```console
python clan_competition.py --input ~/releaseX/clan_competition/sorted --full
```    

--input: Path to ~/releaseX/clan_competition/sorted

--pdb: Type of hits to compete PDB (pdb_full_region table)

--full: Type of hits to compete FULL (full_region table)

## Prepare rfam_live for a new release

1. Populate rfam_live tables using populate_rfamlive_for_release.py:

python populate_rfamlive_for_release.py --all    

2. Make keywords using make_rfam_keywords_table.pl:

perl make_rfam_keywords_table.pl    

3. Update taxonomy_websearch table using updateTaxonomyWebsearch.pl (:warning: requires cluster access):

perl updateTaxonomyWebsearch.pl    

## Running view processes

:warning: Requires cluster access and updating the PDB fasta file. Note that using PDB view processes plugin will be discontinued in favour of regular updates of the entire pdb_full_region table.

1. Create a list of tab separated family accessions and their corresponding uuids using the following query:
```console

select rfam_acc, uuid from _post_process where status='PEND';  
```  

2. Update the PDB sequence file and the path in the PDB plugin

3. Launch view processes on the EBI cluster:

```console
python job_dequeuer.py --view-list /path/to/rfam_uuid_pairs.tsv --dest-dir /path/to/destination/directory 
```

--view-list: A list with tab separated rfam_acc, uuid pairs to run view processes on
--dest-dir: The path to the destination directory to generate shell scripts and log to

## Update PDB mapping

This step requires a finalised Rfam.cm file with the latest families, including descriptions (see FTP section for instructions).

```console
1. Get PDB sequences in FASTA format

wget ftp://ftp.wwpdb.org/pub/pdb/derived_data/pdb_seqres.txt.gz     

gunzip pdb_seqres.txt.gz    

2. Split into individual sequences

perl -pe "s/\>/\/\/\n\>/g" < pdb_seqres.txt > pdb_seqres_sep.txt    

3. Remove protein sequences using collateSeq.pl:

perl collateSeq.pl pdb_seqres_sep.txt     

mv pdb_seqres_sep.txt.noprot pdb_seqres_sep_noprot.fa    

4. Remove extra text from FASTA descriptor line

awk '{print $1}' pdb_seqres_sep_noprot.fa > pdb_trimmed_noprot.fa    

5. Replace any characters that are not recognised by Infernal

sed -e '/^[^>]/s/[^ATGCURYMKSWHBVDatgcurymkswhbvd]/N/g' pdb_trimmed_noprot.fa > pdb_trimmed_noillegals.fa    

6. Split into 100 files

mkdir files

seqkit split -O files -p 100 pdb_trimmed_noillegals.fa    

7. Run cmscan in parallel

cd files     
cmpress Rfam.cm     
bsub -o part_001.lsf.out -e part_001.lsf.err -M 16000 "cmscan -o part_001.output --tblout part_001.tbl --cut_ga Rfam.cm pdb_trimmed_noillegals.part_001.fa"     
...     
bsub -o part_100.lsf.out -e part_100.lsf.err -M 16000 "cmscan -o part_100.output --tblout part_100.tbl --cut_ga Rfam.cm pdb_trimmed_noillegals.part_001.fa"  

To check that all commands completed, check that every log file contains Successfully completed:

cat part_*lsf.out | grep Success | wc -l    

8. Combine results

cat *.tbl | sort | grep -v '#' > PDB_RFAM_X_Y.tbl    

9. Convert Infernal output to pdb_full_region table using infernal_2_pdb_full_region.py:

python infernal_2_pdb_full_region.py --tblout /path/to/pdb_full_region.tbl --dest-dir /path/to/dest/dir    

The script will generate a file like pdb_full_region_YYYY-MM-DD.txt.

10. Create a new temporary table in rfam_live

create table pdb_full_region_temp like pdb_full_region; 
```   

11. Manually import the data in the .txt dump into rfam_live using Sequel Ace or similar

:warning: mysqlimport or LOAD DATA INFILE do not work with rfam_live because of secure-file-priv MySQL setting.

11. Examine the newly imported data and compare with pdb_full_region

12. Rename pdb_full_region to pdb_full_region_old and rename pdb_full_region_temp to pdb_full_region

13. Clan compete the hits as described under Clan competition section using the --PDB option

14. List new families with 3D structures 

> -  number of families with 3D before
select count(distinct rfam_acc) from pdb_full_region_old where is_significant = 1;

-  number of families with 3D after
select count(distinct rfam_acc) from pdb_full_region where is_significant = 1;

-  new families with 3D
select distinct rfam_acc
from pdb_full_region
where
is_significant = 1
and rfam_acc not in
(select distinct rfam_acc
from pdb_full_region_temp
where is_significant = 1); 

## Stage RfamLive for a new release

Create MySQL dumps using mysqldump:

1. Create a new MySQL dump to replicate the database on REL and PUBLIC servers:

```console
export MYSQL_PWD=rfam_live_password

bsub -o mysqldump.out -e mysqldump.err "mysqldump -u <user> -h <hostname> -P <port> --single-transaction --add-locks --lock-tables --add-drop-table --dump-date --comments --allow-keywords --max-allowed-packet=1G rfam_live > rfam_live_relX.sql"
```

To create a MySQL dump of a single table:

```console
mysqldump -u username  -h hostname -P port -p --single-transaction --add-locks --lock-tables --add-drop-table --dump-date --comments --allow-keywords --max-allowed-packet=1G database_name table_name > table_name.sql
```

Restore a MySQL database instance on a remote server
```console

2. Move to the directory where a new MySQL dump is located

    cd /path/to/rfam_live_relX.sql    

3. Connect to the MySQL server (e.g. PUBLIC)

    mysql -u username  -h hostname -P port -p    

4. Create a new MySQL Schema

    Create schema rfam_X_Y;    

5. Select schema to restore MySQL dump

    Use rfam_X_Y;    

6. Restore the database

    source rfam_live_relX.sql  
```  

## Generate FTP files

1. Generate annotated tree files

Generate new tree files for the release using generate_ftp_files.py:

-  to export all files (recommended 16GB RAM to checkout large families)
python generate_ftp_files.py --acc all --tree --dest-dir /path/to/tree/files/dest/dir

-  to export accessions listed in a file
python generate_ftp_files.py -f /path/to/rfam_accession_list.txt --tree --dest-dir /path/to/tree/files/dest/dir

alternatively use writeAnnotatedTree.pl:

perl writeAnnotatedTree.pl RFXXXXX

2. Generate rfam2go files

Create a new rfam2go export by running rfam2go.pl:

rfam2go.pl > /path/to/dest/dir/rfam2go    

Add a header line to rfam2go with release and date.

> 
!version date: <YYYY/MM/DD>     

!description: A mapping of GO terms to Rfam release <XX.X>.     

!external resource: https://rfam.org/     

!citation: Kalvari et al. (2020) Nucl. Acids Res. 49: D192-D200     

!contact: rfam-help@ebi.ac.uk      

Create md5 checksum of rfam2go file:

cd rfam2go && md5sum * > md5.txt    

Run GO validation and review warnings using validate_rfam2go.pl:

perl validate_rfam2go.pl goselect selectgo rfam2go    

## Generate Rfam.full_region file

Export ftp_Rfam_full_region.sql:

export MYSQL_PWD=rfam_live_password

mysql -u <user> -h <host> -P <port> --database rfam_live < sql/ftp_rfam_full_reqion.sql > /path/to/Rfam.full_region

gzip Rfam.full_region

## Generate Rfam.pdb file

Export ftp_rfam_pdb.sql:

```console
export MYSQL_PWD=rfam_live_password

mysql -u <user> -h <host> -P <port> --database rfam_live < sql/ftp_rfam_pdb.sql > /path/to/Rfam.pdb

gzip Rfam.pdb
```

## Generate Rfam.clanin file

Generate a new Rfam.clanin file using clanin_file_generator.py:

python clanin_file_generator.py /path/to/destination/directory

## Generate database_files folder

The option --tab of mysqldump is used to generate dumps in both .sql and .txt formats, where:

table.sql includes the MySQL query executed to create a table

table.txt includes the table contents in tabular format

The main rfam_live database is running with the secure-file-priv setting so it is not possible to export using the --tab option directly. A workaround is to copy dump files on a laptop, and use a local MySQL database for export.

Create a new database dump using mysqldump:
```console

    mysqldump -u <user> -h <host> -P <port> -p --single-transaction --add-locks --lock-tables --add-drop-table --dump-date --comments --allow-keywords --max-allowed-packet=1G --tab=/tmp/database_files rfam_local  
```  

Run the database_file_selector.py python script to create the subset of database_files available on the FTP:

```console
    cd /releaseX/database_files     
    gzip *.txt     
    python database_file_selector.py --source-dir /tmp --dest-dir /releaseX/database_files
```    

Zip database_files directory and copy to remote server:

    tar -czvf database_files.tar.gz /releaseX/database_files     scp database_files.tar.gz username@remote.host:/some/location    

Restore database_files on FTP

    tar -xzvf /some/location/database_files.tar.gz .    

## Generate fasta_files folder

Export new fasta files for all families in Rfam using fasta_file_generator.py:

```console
bsub -M 10000 -o fasta_generator.lsf.out -e fasta_generator.lsf.err "python fasta_file_generator.py --seq-db /path/to/rfamseq.fa --rfam-seed /path/to/releaseX/Rfam.seed --all --outdir /path/to/ftp/fasta_files"
```

## Update Rfam text search

Description of indexed fields: https://www.ebi.ac.uk/ebisearch/metadata.ebi?db=rfam

## Generate new data dumps

Requirements:

The directory for the text search index must have the following structure:

release_note.txt

families

clans

genomes

motifs

full_region

Move to the main rfam-production repo and setup the django environment:

    source django_settings.sh    

Create an output directory for a new release index and all required subdirectories:

```console
cd /path/to/relX_text_search     

mkdir clans     

mkdir families     

mkdir full_region     

mkdir genomes     

mkdir motifs  
```  

Create new XML dumps:
```console

cd /path/to/relX_text_search     

python rfam_xml_dumper.py --type F --out families     

python rfam_xml_dumper.py --type C --out clans     

python rfam_xml_dumper.py --type M --out motifs     

python rfam_xml_dumper.py --type G --out genomes     

python rfam_xml_dumper.py --type R --out full_region   
``` 

For more information on launching XML dumps as LSF jobs see lsf_rfam_xml_dumper.sh.

Validate xml dumps using xml_validator.py:
```console

python xml_validator.py --input /path/to/relX_text_search/families --log     

python xml_validator.py --input /path/to/relX_text_search/clans --log     

python xml_validator.py --input /path/to/relX_text_search/motifs --log     

python xml_validator.py --input /path/to/relX_text_search/genomes --log     

python xml_validator.py --input /path/to/relX_text_search/full_region --log   
``` 

Check that error.log files in each folder is empty.

Get the total number of entries:

    grep -r 'entry id' . | wc -l    

Create release_note.txt file:

    release=14.5     release_date=15-Mar-2021     entries=2949678    

## Index data on dev and prod

Change directory to text_search on the cluster:

cd_main && cd search_dumps    

Index data on dev:

unlink rfam_dev     

ln -s /path/to/xml/data/dumps rfam_dev    

Index data on prod:

unlink current_release     

ln -s /path/to/xml/data/dumps current_release    

The files in rfam_dev and current_release folders are automatically indexed every night.

## Create new json dumps for RNAcentral

Create a new region export from Rfam using Rfam2RNAcentral.pl:

Extract SEED regions

    perl Rfam2RNAcentral.pl SEED > /path/to/relX/rnacentral/dir/rfamX_rnac_regions.txt    

Extract FULL region

    perl Rfam2RNAcentral.pl FULL >> /path/to/relX/rnacentral/dir/rfamX_rnac_regions.txt    

Split regions into smaller chunks using basic linux split comman

mkdir chunks &&     cd chunks &&     split -n 3000 /path/to/relX/rnacentral/dir/rfamX_rnac_regions.txt rnac_ --additional-suffix='.txt'    

-n: defines the number of chunks to generate (2000 limit on LSF)

Notes:
- This command will generate 3000 files named like rnac_zbss.txt
- Use -l 500 option for more efficient chink size

Create a copy of the fasta files directory

mkdir fasta_files && cd fasta_files &&     wget http://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/fasta_files/* .    
NOTE: Rfam2RNAcentral regions need to match the exact same release the fasta_files were created for.
Use the Public MySQL database if rfam-live have been updated

Unzip all fasta files and index using esl-sfetch:

gunzip * .gz for file in ./RF*.fa; do esl-sfetch --index $file; done    

Copy the Rfam.seed file from the FTP to the fasta_files directory and index using esl-sfetch`:

wget http://ftp.ebi.ac.uk/pub/databases/Rfam/CURRENT/Rfam.seed.gz && gunzip Rfam.seed.gz     esl-sfetch --index Rfam.seed    

Launch a new json dump using rnac2json.py:

- fasta file directory: Create a new fasta files directory
- json files directory: Create a new json files output directory

python rnac2json.py --input /path/to/chunks --rfam-fasta /path/to/fasta_files --outdir /path/to/json_files    