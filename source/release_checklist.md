# Release Checklist 

This is a template checklist. It can be copied to the GitHub issue for each release. 

## Data updates

- Export annotated CM and SEED files

- Update PDB mapping

- Run view processes

- Run clan competition

- Run make_rfam_keywords_table.pl

- Update family_ncbi

- Update the family table (number_of_species and num_full fields)

- Update the version table

- Update RNAcentral descriptions

## Update FTP archive

- Create .release FTP directory

- database_files

- fasta_files

- genome_browser_hub

- rfam2go

- COPYING

- USERMAN

- README including Section 5

- Rfam.clanin

- Rfam.cm.gz

- Rfam.full_region.gz

- Rfam.pdb.gz

- Rfam.seed.gz

- Rfam.seed_tree.gz

- Rfam.tar.gz

- .tsv release stats file 


## Pre Announce

- Update EBI text search on wwwdev

- Update the website

- Add release graphic

- Update pages with new data (e.g. microRNA or viral project pages)

- Merge new code into rfam-website master

- Update the REL MySQL database

- Stop Apache in OY

- Update the OY web production MySQL database

- Update rfamweb_local.conf (fields ebi_search and connect_info) in OY

- Deploy new website code in OY

- Start Apache in OY

- Verify OY VMs directly

- Repeat for PG

- Move.release FTP directory to release

- Update CURRENT FTP symlink

- Update Public MySQL database using Sequel Ace

- Load new data into a new database called rfam_xx_x

- Rename the Rfam database to rfam_xx_y

- Rename the rfam_xx_x database to Rfam

- Delete old databases to save space on PUB

- Update EBI cmscan search - check that it automatically picks up the new covariance models from the production FTP location

- Update RNAcentral sequence search

- Update rfam_local.py config to latest db

## Announce

- Publish blog post with the Rfam tag

- Post on Twitter

## Post Announce

- Close GitHub release project

- Review GitHub issues

- Backup old text search files - tar.gz folder in search_dumps

- Update EBI text search in production

- Make sure production website uses production search index

- Update Rfam models in R2DT

- Update Rfam taxonomy and create a new GitHub release

- Create a release checklist for the next release

