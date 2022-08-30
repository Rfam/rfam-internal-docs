
# Copying Data

## paths

- grep /nfs/ on all code in Rfam to get a list of distinct paths in the code 
- Rfam.conf
- rfam-family-pipeline/Rfam/Conf/rfam_local.conf
- rfamprod .bashrc
- all aliases 
- rh74 all
- soft all
- rfamprod folder

## datamover queue

- need to use the datamover queue to transfer between noah and codon, e.g.

```
bsub -n 4 -q datamover "/hps/software/copytools/msrsync/msrsync 
hps/nobackup/production/xfam/rfam/RELEASES /nfs/production/agb/rfam/"
```

## folders to copy 

- folders to copy in to `/nfs/production/agb/rfam`

  - /hps/ebi/nobackup/production/xfam/rfam/RELEASES
  - /nfs/ebi/production/xfam/users/rfamprod 
  - /hps/ebi/nobackup/production/xfam/rfam/
  - /hps/ebi/nobackup2/production/xfam/rfam/
  - /nfs/ebi/production/xfam/rfam/rfamseq


- Copying data from all aliases
  - alias cd_prod='cd /nfs/production/xfam/users/rfamprod'
  - alias cd_rh74='cd /nfs/production/xfam/rfam/rfam_rh74'
  - /nfs/production/agb/rfam/software = rh74 stuff
  - alias cd_rfamp='cd /nfs/production/xfam/users/rfamprod/rfamp/'
  - alias cd_rfam='cd /nfs/production/xfam/rfam/rfam_rh74/production_software/rfam_production/rfam-family-pipeline/Rfam'
  - alias cd_soft='cd /nfs/production/xfam/rfam/rfam_rh74/software'
  - alias cd_lib='cd /nfs/production/xfam/rfam/rfam_rh74/production_software/rfam_production/rfam-family-pipeline/Rfam/Lib/Bio/Rfam'
  - alias cd_hps='cd /hps/nobackup/production/xfam/rfam'
  - alias cd_gpfs='cd /gpfs/nobackup/xfam/rfam' - no such file or directory
  - alias cd_code='cd /nfs/production/xfam/users/rfamprod/code'
  - alias cd_rel='cd /hps/nobackup/production/xfam/rfam/RELEASES'
  - alias cd_main='cd /nfs/production/xfam/rfam'
  - alias cd_gens='cd /hps/nobackup/production/xfam/rfam/genome_datasets'
  - alias cd_rfamseq='cd /nfs/production/xfam/rfam/rfamseq'
  - alias cd_megs='cd /hps/nobackup/production/xfam/rfam/METAGENOMICS'
  - alias cd_hps2='cd /hps/nobackup2/production/rfam'
  - alias cd_mir='cd /hps/nobackup/production/xfam/rfam/RELEASES/14.3'
  - alias cd_ftp='cd /ebi/ftp/pub/databases/Rfam'
  - alias search_dumps='cd /nfs/production/xfam/rfam/search_dumps'
  - /nfs/ftp/public/databases/Rfam/
  - /nfs/ftp/pub/databases/Rfam/.preview/