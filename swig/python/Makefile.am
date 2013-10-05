
all-local: _xwiimote.so

$(builddir)/%_wrap.c %.py: ../xwiimote.i
	$(SWIG) -I$(top_srcdir)/lib -python -o $(builddir)/xwiimote_wrap.c $(srcdir)/../xwiimote.i

_%.so: $(builddir)/%_wrap.c
	env CFLAGS="" python setup.py build_ext

clean-local:
	-@rm -vf xwiimote.py xwiimote_wrap.c

MOSTLYCLEANFILES = installed_files.log

install-exec-hook: $(builddir)/xwiimote_wrap.c
	if test -n "$(DESTDIR)"; then\
		python setup.py install --root=$(DESTDIR) --prefix=$(prefix) --record=@top_builddir@/swig/python/installed_files.log; \
	else \
		python setup.py install --prefix=$(prefix) --record=@top_builddir@/swig/python/installed_files.log; \
	fi

uninstall-hook:
	cat @top_builddir@/swig/python/installed_files.log \
	  | awk '{print "$(DESTDIR)"$$1}' | xargs rm -vf
