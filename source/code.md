# Code Changes  

## PDB Pipeline

- Change all hardcoded paths to params - in config file
- Update paths in config file 
- Update PYTHONPATH variable in config 
- Need to copy over search dumps 
- Check slack config
- Will need to update database configs on codon

## To submit jobs 

- had to change the queue names in several scripts in the family pipeline repo (https://github.com/Rfam/rfam-family-pipeline/)
- run rfmake with -local
- run rfsearch with -scpu 0 