EXTRA_DIST =	perl_wrap.c xwiimote.pm
noinst_DATA =	xwiimote.so

perl_wrap.c: $(srcdir)/../xwiimote.i
	$(SWIG) -I$(top_srcdir)/lib -perl -module xwiimote -o $@ $(srcdir)/../xwiimote.i

MOSTLYCLEANFILES=perl_wrap.c xwiimote.pm xwiimote.bs

Makefile.perl: Makefile.PL
	$(PERL) $< PREFIX=$(prefix) MAKEFILE=$@

xwiimote.so: perl_wrap.c Makefile.perl
	if test ! -f perl_wrap.c; then cp $(srcdir)/perl_wrap.c . ; fi
	$(MAKE) -fMakefile.perl
	if test $(top_srcdir) != $(top_builddir) ; then rm -f perl_wrap.c ; fi

install-data-local: Makefile.perl
	$(MAKE) -fMakefile.perl install

clean-local:
	if test -f Makefile.perl; then $(MAKE) -fMakefile.perl realclean; fi
	rm -f *.so

