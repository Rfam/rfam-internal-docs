
# Websites

There are two sets of machines that serve the live websites. PG virtual machines are in Hemel Hempstead (HH) and the OY VMs are located in Hinxton (HX). The load balancers ensure that most requests are handled by the PG machines with the OY machines serving as backup. The Rfam PG and OY VMs are run from separate directories and the update mechanism for each data-centre is different. 
The test website, `preview.rfam.org`, is served by a HX web VM. 

The preview website runs directly from the **rfam-live** database. The OY VMs use the **fb1-mysql-rfam-rel.ebi.ac.uk** database while the PG VMs use the **pg-mysql-rfam-rel.ebi.ac.uk** database.

## Servers

- ves-pg-b7.ebi.ac.uk
- ves-pg-b9.ebi.ac.uk
- ves-oy-b7.ebi.ac.uk
- ves-oy-b9.ebi.ac.uk
- ves-hx-b7.ebi.ac.uk

## Local development

The source code of the Rfam website is maintained on GitHub: [rfam-website](https://github.com/Rfam/rfam-website). Follow the Readme instructions to run the website locally using Docker.

## Updating test website

The test website runs on ves-hx-b7.ebi.ac.uk and is available off-campus at <https://preview.rfam.org>.

```
become xfm_adm

ssh ves-hx-b7

cd /nfs/public/rw/xfam/rfam/test/rfam-website
# optional: check settings in config/rfamweb_local.conf

git pull

# clear cache 
echo flush_all | nc ves-hx-b7.ebi.ac.uk 11211

# Optional: restart Apache to apply Perl code changes (not needed for JS or HTML template changes)
sudo /etc/init.d/httpd restart
```