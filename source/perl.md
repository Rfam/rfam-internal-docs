# Perl 

## Installing Modules 

- create an autobundle of the current perl modules 

```perl -MCPAN -eautobundle```

  - when finished you will see a message like 

    ```Wrote bundle file /homes/user/.cpan/Bundle/Snapshot_2022_03_03_00.pm```

  - copy this line, you will need the path later on

- now run

```perl -MCPAN -e ‘install Bundle::Snapshot_2022_03_03_00’```

  - this will read and install what is listed in the snapshot file 


- may need to change the lib directory in .cpan/CPAN/MyConfig.pm so that ‘my’ perl modules are installed where wanted (in this case `/hps/software/users/agb/rfam/perl/lib`)

- needed to manually install some modules along the way with CPAN install, e.g.

```console 
cpan -f install File::ShareDir::Install 
cpan -f install Inline::C
cpan -f install Data::Printer 
cpan -f install Config::General 
cpan -f install DBIx::Class::Schema 
cpan -f install DateTime 
cpan -f install DateTime::Format::MySQL  
cpan -f install MooseX::NonMoose 
cpan -f install Bio::Annotation::Reference 
cpan -f install File::Touch 
cpan -f install IPC::Run
cpan -f install Term::ReadPassword
cpan -f install JSON 
cpan -f install JSON::XS

cpan -f install DBIx::Class
cpan -f install Catalyst::Devel
cpan -f install Catalyst::Utils
cpan -f install DBIx::Class
cpan -f install Catalyst::Model
cpan -f install DBIx::Class::ResultSet

cpanm -l /hps/software/users/agb/pfam/perl/lib/perl5 --force My::Package
```

## Bio-Easel 

- installing Bio-Easel following [these instructions](https://github.com/nawrockie/Bio-Easel)
  - used 0.15
- need to install with perl 5.26 (not 5.34 as had be done - needed to reinstall)
- had issues with paths installing in `/hps/software/users/agb/rfam`, currently installed in `/homes/rfamprod`
  