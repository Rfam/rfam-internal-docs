
# Codon Migration 

This collection has notes and details on the steps taken to migrate from noah (old EBI cluster) to codon. All of the data needed to be copied from several different paths. We had to make updates to hardcoded paths in the code, and ensure all the configs were updated. Should a migration need to be repeated, the following checklist may act as a start:

- autobundle the Perl code and re-install the modules
- manual (cpan) install of Perl modules
- copy all data, files, folders