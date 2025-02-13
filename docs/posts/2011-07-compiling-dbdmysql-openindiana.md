---
date: 2011-07-11T23:50:00.000+02:00
description: ''
published: true
slug: 2011-07-compiling-dbdmysql-openindiana
tags:
- CPAN
- Perl
- OpenIndiana
- legacy-blogger
readtime: 5
title: Compiling DBD::mysql @OpenIndiana
---

*This was originally posted on blogger [here](https://snarkybrill.blogspot.com/2011/07/compiling-dbdmysql-openindiana.html)*.

I tried to compile <b>DBD::mysql</b> module from CPAN on <b>oi_148</b> by SunStudio cc using commands:<br />
<br />
<pre>export PATH="$PATH:/usr/mysql/5.1/bin/amd64"
perl -MCPAN -e shell
cpan[1]&gt; install DBD::mysql
</pre><br />
I got:<br />
<br />
<pre>cpan[1]&gt; install DBD::mysql
</pre><pre>(...)</pre><pre>I will use the following settings for compiling and testing:

  cflags        (mysql_config) = -I/usr/mysql/5.1/include/mysql  -xprefetch=auto -xprefetch_level=3 -mt -fns=no -fsimple=1 -xbuiltin=%all -xlibmil -xlibmopt -xnorunpath -m64   -DHAVE_RWLOCK_T -DUNIV_SOLARIS
  embedded      (mysql_config) = 
  libs          (mysql_config) = -lrt -L/usr/mysql/5.1/lib/amd64/mysql -R/usr/mysql/5.1/lib/amd64/mysql -lmysqlclient -lz -lsocket -lnsl -lm
  mysql_config  (guessed     ) = mysql_config
  nocatchstderr (default     ) = 0
  nofoundrows   (default     ) = 0
  ssl           (guessed     ) = 0
  testdb        (default     ) = test
  testhost      (default     ) = 
  testpassword  (default     ) = 
  testsocket    (default     ) = 
  testuser      (guessed     ) = root

To change these settings, see 'perl Makefile.PL --help' and
'perldoc INSTALL'.

Checking if your kit is complete...
Looks good
Using DBI 1.616 (for perl 5.008004 on i86pc-solaris-64int) installed in /usr/perl5/site_perl/5.8.4/i86pc-solaris-64int/auto/DBI/
Writing Makefile for DBD::mysql
Writing MYMETA.yml and MYMETA.json
cp lib/DBD/mysql.pm blib/lib/DBD/mysql.pm
cp lib/DBD/mysql/GetInfo.pm blib/lib/DBD/mysql/GetInfo.pm
cp lib/DBD/mysql/INSTALL.pod blib/lib/DBD/mysql/INSTALL.pod
cp lib/Bundle/DBD/mysql.pm blib/lib/Bundle/DBD/mysql.pm
cc -c  -I/usr/perl5/site_perl/5.8.4/i86pc-solaris-64int/auto/DBI -I/usr/mysql/5.1/include/mysql  -xprefetch=auto -xprefetch_level=3 -mt -fns=no -fsimple=1 -xbuiltin=%all -xlibmil -xlibmopt -xnorunpath -m64   -DHAVE_RWLOCK_T -DUNIV_SOLARIS -DDBD_MYSQL_INSERT_ID_IS_GOOD -g  -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -D_TS_ERRNO -xO3 -xspace -xildoff   -DVERSION=\"4.019\" -DXS_VERSION=\"4.019\" -KPIC "-I/usr/perl5/5.8.4/lib/i86pc-solaris-64int/CORE"   dbdimp.c
/usr/perl5/5.8.4/bin/perl -p -e "s/~DRIVER~/mysql/g" /usr/perl5/site_perl/5.8.4/i86pc-solaris-64int/auto/DBI/Driver.xst &gt; mysql.xsi
/usr/perl5/5.8.4/bin/perl /usr/perl5/5.8.4/lib/ExtUtils/xsubpp  -typemap /usr/perl5/5.8.4/lib/ExtUtils/typemap  mysql.xs &gt; mysql.xsc &amp;&amp; mv mysql.xsc mysql.c
Warning: duplicate function definition 'do' detected in mysql.xs, line 242
Warning: duplicate function definition 'rows' detected in mysql.xs, line 749
cc -c  -I/usr/perl5/site_perl/5.8.4/i86pc-solaris-64int/auto/DBI -I/usr/mysql/5.1/include/mysql  -xprefetch=auto -xprefetch_level=3 -mt -fns=no -fsimple=1 -xbuiltin=%all -xlibmil -xlibmopt -xnorunpath -m64   -DHAVE_RWLOCK_T -DUNIV_SOLARIS -DDBD_MYSQL_INSERT_ID_IS_GOOD -g  -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64 -D_TS_ERRNO -xO3 -xspace -xildoff   -DVERSION=\"4.019\" -DXS_VERSION=\"4.019\" -KPIC "-I/usr/perl5/5.8.4/lib/i86pc-solaris-64int/CORE"   mysql.c
"mysql.xs", line 687: warning: implicit function declaration: mysql_st_next_results
"mysql.xs", line 897: warning: implicit function declaration: is_prefix
Running Mkbootstrap for DBD::mysql ()
chmod 644 mysql.bs
rm -f blib/arch/auto/DBD/mysql/mysql.so
LD_RUN_PATH="/lib:/usr/mysql/5.1/lib/amd64/mysql" /usr/perl5/5.8.4/bin/perl myld cc  -G dbdimp.o mysql.o  -o blib/arch/auto/DBD/mysql/mysql.so  \
           -lrt -L/usr/mysql/5.1/lib/amd64/mysql -R/usr/mysql/5.1/lib/amd64/mysql -lmysqlclient -lz -lsocket -lnsl -lm          \
          
ld: fatal: file dbdimp.o: wrong ELF class: ELFCLASS64
ld: fatal: file processing errors. No output written to blib/arch/auto/DBD/mysql/mysql.so
make: *** [blib/arch/auto/DBD/mysql/mysql.so] Error 1
  CAPTTOFU/DBD-mysql-4.019.tar.gz
  /usr/gnu/bin/make -- NOT OK
'YAML' not installed, will not store persistent state
Running make test
  Can't test without successful make
Running make install
  Make had returned bad status, install seems impossible
Failed during this command:
 CAPTTOFU/DBD-mysql-4.019.tar.gz              : make NO
</pre><br />
The problem was that <b>I mixed up 64-bit AMD64 MySQL libraries with stock 32-bit Perl binaries</b>. Solution was to omit amd64 from path:<br />
<br />
<pre>export PATH="$PATH:/usr/mysql/5.1/bin"
perl -MCPAN -e shell
cpan[1]&gt; install DBD::mysql
</pre>