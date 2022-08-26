# Release Process

The release process is carried out through the running of these steps: https://github.com/Rfam/rfam-production/tree/master/docs/release

The process is mostly automated, however some steps still require manual updates. Until the automation process is running smoothly, you may find it easier to run the workflows in the given order, to ease potential troubleshooting. 

1. Generate annotated files

2. Update PDB mapping

3. Run view process

4. Run clan competition

5. Prepare RfamLive

6. Generate FTP files

    a. manual step to generate database_files folder

7. Generate rfam2go

8. Stage RfamLive

9. Update text search

10. Copy from production folder to release ftp 

