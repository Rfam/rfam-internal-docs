
# Websites

There are two sets of machines that serve the live websites. PG virtual machines are in Harlow 1 (HL1) and the OY VMs are located in Harlow 2 (HL2). The load balancers ensure that most requests are handled by the PG machines with the OY machines serving as backup. The Rfam PG and OY VMs are run from separate directories and the update mechanism for each data-centre is different. 

The test website: `preview.rfam.org`, and dev are served by HL1 web VM. Fallback and disaster recovery sites are in HL2.

The preview website runs directly from the **rfam-live** database. The OY VMs use the **fb1-mysql-rfam-rel.ebi.ac.uk** database while the PG VMs use the **pg-mysql-rfam-rel.ebi.ac.uk** database.

## Servers

- ves-pg-b7.ebi.ac.uk
- ves-pg-b9.ebi.ac.uk
- ves-oy-b7.ebi.ac.uk
- ves-oy-b9.ebi.ac.uk
- ves-hx-b7.ebi.ac.uk

## Local development

The source code of the Rfam website is maintained on GitHub: [rfam-website](https://github.com/Rfam/rfam-website). Follow the Readme instructions to run the website locally using Docker.

## Updating the website

The websites can be updated using **Jenkins** - simply choose the correct site (HX, OY, PG) from the dropdown website for the build `update_rfam_website`. 
