# `rfci.pl` notes

For large families, it is recommended you login to a node with at
least 64G of RAM (`bsub -M 64000M -q research -Is $SHELL`). The
overlap check requires a lot of RAM. There may be other steps that
require a lot of RAM as well.

Table of running times for commits (`rfci.pl`) (compiled 10/22:

| acc     | family id              | seed #seq | full #seq | `rfci.pl` flags*  | runtime |
| ------- | ---------------------- | --------- | --------- | ----------------- | ------- |
| RF00177 | SSU_rRNA_bacteria      |  99       | 37125     | `-i coding -i overlap` | 17 min |
| RF01960 | SSU_rRNA_eukarya       |  90       | 39618     | `-i coding -i overlap` |  9 min |
| RF02540 | LSU_rRNA_archaea       |  91       | 48683     | `-i coding -i overlap` | 23 min |
| RF02542 | SSU_rRNA_microsporidia |  46       | 35934     | `-i coding -i overlap` | 11 min |
| RF02543 | LSU_rRNA_eukarya       |  88       | 54013     | `-i coding -i overlap` |  7 min |
| RF03064 | RAGATH-18              | 1420      | 1771      |                        | 9 min  |

* `-i spell -i missing -preseed` also used for *all* above `rfci.pl` commands

---

Miscellaneous notes:

Jiffy script that loads info to RfamLive:
`/homes/nawrocki/git/Rfam/rfam-family-pipeline/Rfam/Scripts/jiffies/update_seed_dependent_tables_for_family.pl`

Note this is only a local file not in the git repo.

Code that updates the RfamLive tables is here:

Rfam/Schemata/{RfamLive,RfamDB}/ResultSet/Taxonomy.pm
Rfam/Schemata/{RfamLive,RfamDB}/ResultSet/SeedRegion.pm
Rfam/Schemata/{RfamLive,RfamDB}/ResultSet/Rfamseq.pm
Rfam/Schemata/{RfamLive,RfamDB}/ResultSet/RnacentralMatch.pm
