# Rfam sequence search - Infernal cmscan 


> **_NOTE:_** 
>Since May 2021, the Rfam.cm file from the CURRENT Rfam FTP folder is fetched automatically every night by both production and wwwdev cmscan services. If Rfam is silently updated 1 day before the public announcement, the cmscan service should already be updated and no email is necessary.

Note that there will be a delay between the public Rfam release and the cmscan service update. If this delay is not acceptable, follow the manual update procedure using email. 

The batch Rfam sequence search is powered by the Infernal cmscan program which is run by the EBI web production team. The Infernal service is available in several ways:

Web form: <http://www.ebi.ac.uk/Tools/rna/infernal_cmscan/>

REST API: <http://www.ebi.ac.uk/Tools/webservices/services/rna/infernal_cmscan_rest>

Every release Rfam updates the covariance models so we need to update the Rfam.cm file used by the Infernal service. When a new version of Infernal becomes available, we may also need to update the cmscan command, cmscan binary, or EBI help for cmscan. 

1. Email  <www-prod@ebi.ac.uk> using the template below:

> To: www-prod@ebi.ac.uk
>Subject: Infernal cmscan service update
> 
>Hello,
> 
>Could you please update the Infernal cmscan service to use a new set of
>covariance models from release 14.6 that are available here:
> 
><http://ftp.ebi.ac.uk/pub/databases/Rfam/.14.6/Rfam.cm.gz>
> 
>Please let me know if you need more information. Many thanks in advance!

2. Verify that the update worked. 

To test that the search works as expected, navigate to 

<http://www.ebi.ac.uk/Tools/rna/infernal_cmscan/> (production environment) 

or 

<http://wwwdev.ebi.ac.uk/Tools/rna/infernal_cmscan/> (testing environment) 

and try searching with a sequence from a new family that is only present in the new release. Make sure that the sequence is found by the correct family.