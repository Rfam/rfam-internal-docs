
# Databases

## RfamLive

The main database used for ongoing curation. All new families are immediately visible in Rfam Live.

Host: mysql-rfam-live.ebi.ac.uk

There is also an rfamro user (no password required) with read-only access.

## RfamRel

A Gold Master copy of RfamLive generated at release time. Previous releases are available as separate schemas.

Host: mysql-rfam-rel.ebi.ac.uk

## Rfam Web FB1

A fallback database used for the fallback web VMs located in Hinxton.

Host: fb1-mysql-rfam-rel.ebi.ac.uk

## Rfam Web PG

Main database serving the main web VMs located in London.

Host: pg-mysql-rfam-rel.ebi.ac.uk

## Rfam Public 

A public database used by Rfam users (the users have read only access, see credentials on Rfam Docs).

Host: mysql-rfam-public.ebi.ac.uk