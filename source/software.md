
# Software Installation

## Infernal 

- use the `—-prefix` option with configure to ensure software is installed to the correct location, for example:

```console
cd /hps/software/users/agb/rfam/
bsub -Is $SHELL
cd packages/
curl -OL http://eddylab.org/infernal/infernal-1.1.2.tar.gz
tar -xvzf infernal-1.1.2.tar.gz
cd infernal-1.1.2
./configure --help
cd infernal-1.1.2
./configure --prefix=/hps/software/users/agb/rfam
make
make install
```

## R-scape 

```console
mv rscape rscapev1.5.16
wget http://eddylab.org/software/rscape/rscape.tar.gz
tar -xvzf rscape.tar.gz
mv rscape_* rscape
rm rscape.tar.gz
cd rscape
./configure --help
./configure --prefix=/hps/software/users/agb/rfam
make
make install
```

- R-scape then needs to be added to the PATH variable
- As of July 2022, we are using R-scape version 2.0.0.g

## R 

```console
R-2.6.0/bin/R CMD BATCH --no-save  plot_outlist.R
```

## bedToBigBed

```console
 cd /hps/software/users/agb/rfam/
 cd bin/
 wget http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/bedToBigBed
 chmod +x ./bedToBigBed
 ```

## aspell

```console
wget https://ftp.gnu.org/gnu/aspell/aspell-0.60.8.tar.gz
cd ..
tar -xvzf packages/aspell-0.60.8.tar.gz 
ls bin/
cd aspell-0.60.8/
./configure --prefix=/hps/software/users/agb/rfam
make
make install
cd ..
aspell -v
```

## seqkit

```console
cd packages/
wget https://github.com/shenwei356/seqkit/releases/download/v2.2.0/seqkit_linux_amd64.tar.gz
cd ..
tar -xvzf packages/seqkit_linux_amd64.tar.gz
cp seqkit bin/
seqkit version
```

## misc.

```console
wget http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/hubCheck
wget http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/faSplit
wget http://hgdownload.cse.ucsc.edu/admin/exe/linux.x86_64/fetchChromSizes
chmod +x hubCheck
chmod +x faSplit 
chmod +x fetchChromSizes 
```

## netxflow 

- module available on codon, but installed a later version for our own use
- nextflow jobs will not run on codon from an interactive job, you must submit the jobs from a login node and monitor their progress with the output or the .nextflow.log
- getting this error on codon, as are other users, no solution from IT as of yet

> [rfamprod@hl-codon-30-03 rfam-production]$ nextflow run pdb_mapping/pdb_mapping.nf 

N E X T F L O W ~ version 20.07.1

Launching `pdb_mapping/pdb_mapping.nf` [backstabbing_stallman] - revision: e7ea86f012

Signal already used by VM: HUP