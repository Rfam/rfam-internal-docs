# `rfsearch.pl` notes

Families which require special `rfsearch.pl` flags:

| accession | recommended command              | reason |
| --------- | -------------------------------- | ------ |
| RF00017   |  `rfsearch.pl -ignoresm -cut_ga` | too many hits otherwise, and it takes more than 1 week |
| RF02543   |  `rfsearch.pl -cgtailn 200`      | allows calibration to finish successfully, otherwise you get this error 'Error: --gtailn <n>=250 cannot be used, there's only 215.000 hits per Mb in the histogram! Lower <n> or use --tailp.' |

---

Possible `rfsearch.pl` error: 
```
ERROR unable to fetch description for JZJH011 at /homes/nawrocki/git/Rfam/rfam-family-pipeline/Rfam/Lib/Bio/Rfam/FamilyIO.pm line 2070, <RTBL> line 48.
```

If you see an error like this: rerun with `-scpu 0`. This error is
due to a bug in infernal 1.1.4 (actually in easel) that was fixed in
commit 8d300ef, so should be fixed in next release of infernal.
https://github.com/EddyRivasLab/easel/pull/66/commits/8d300ef3899f69365be02635416333d0f30ccbe9


