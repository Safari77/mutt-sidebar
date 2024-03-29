## Process this file with automake to produce Makefile.in
## Use aclocal -I m4; automake --foreign
include $(top_srcdir)/flymake.am

AUTOMAKE_OPTIONS = 1.6 foreign
EXTRA_PROGRAMS = mutt_dotlock mutt_pgpring pgpewrap

if BUILD_IMAP
IMAP_SUBDIR = imap
IMAP_INCLUDES = -I$(top_srcdir)/imap
endif

if BUILD_AUTOCRYPT
AUTOCRYPT_SUBDIR = autocrypt
AUTOCRYPT_INCLUDES = -I$(top_srcdir)/autocrypt
endif

SUBDIRS = m4 po doc contrib $(IMAP_SUBDIR) $(AUTOCRYPT_SUBDIR)

bin_SCRIPTS = muttbug flea $(SMIMEAUX_TARGET)

if BUILD_HCACHE
HCVERSION = hcversion.h
endif

BUILT_SOURCES = keymap_defs.h patchlist.c reldate.h conststrings.c version.h $(HCVERSION)

bin_PROGRAMS = mutt $(DOTLOCK_TARGET) $(PGPAUX_TARGET)
mutt_SOURCES = \
	addrbook.c alias.c attach.c background.c base64.c browser.c buffer.c \
	buffy.c color.c crypt.c cryptglue.c \
	commands.c complete.c compose.c copy.c curs_lib.c curs_main.c \
	curs_ti_lib.c date.c \
	edit.c enter.c flags.c init.c filter.c from.c \
	getdomain.c group.c \
	handler.c hash.c hdrline.c headers.c help.c hook.c keymap.c \
	main.c mbox.c menu.c mh.c mx.c pager.c parse.c pattern.c \
	postpone.c query.c recvattach.c recvcmd.c \
	rfc822.c rfc1524.c rfc2047.c rfc2231.c rfc3676.c \
	score.c send.c sendlib.c signal.c sort.c \
	status.c system.c thread.c charset.c history.c lib.c \
	mutt_lisp.c muttlib.c editmsg.c mbyte.c \
	url.c ascii.c crypt-mod.c crypt-mod.h safe_asprintf.c listmenu.c

nodist_mutt_SOURCES = $(BUILT_SOURCES)

mutt_LDADD = $(MUTT_LIB_OBJECTS) $(LIBOBJS) \
	$(LIBIMAP) $(LIBAUTOCRYPT) \
	$(MUTTLIBS) \
	$(LIBINTL) $(LIBICONV)  $(GPGME_LIBS) $(GPG_ERROR_LIBS)

mutt_DEPENDENCIES = $(MUTT_LIB_OBJECTS) $(LIBOBJS) $(LIBIMAPDEPS) \
	$(INTLDEPS) $(LIBAUTOCRYPTDEPS)

DEFS=-DPKGDATADIR=\"$(pkgdatadir)\" -DSYSCONFDIR=\"$(sysconfdir)\" \
	-DBINDIR=\"$(bindir)\" -DMUTTLOCALEDIR=\"$(datadir)/locale\" \
	-DHAVE_CONFIG_H=1

AM_CPPFLAGS=-I. -I$(top_srcdir) $(IMAP_INCLUDES) $(AUTOCRYPT_INCLUDES) $(GPGME_CFLAGS) $(GPG_ERROR_CFLAGS)

# This option allows `make distcheck` to run without encountering
# setgid errors.
AM_DISTCHECK_CONFIGURE_FLAGS = --with-homespool


EXTRA_mutt_SOURCES = account.c bcache.c compress.c crypt-gpgme.c crypt-mod-pgp-classic.c \
	crypt-mod-pgp-gpgme.c crypt-mod-smime-classic.c \
	crypt-mod-smime-gpgme.c dotlock.c gnupgparse.c hcache.c md5.c monitor.c \
	mutt_idna.c mutt_sasl.c mutt_sasl_gnu.c mutt_socket.c mutt_ssl.c \
	mutt_ssl_gnutls.c \
	mutt_tunnel.c pgp.c pgpinvoke.c pgpkey.c pgplib.c pgpmicalg.c \
	pgppacket.c pop.c pop_auth.c pop_lib.c remailer.c resize.c sha1.c \
	smime.c smtp.c utf8.c wcwidth.c sidebar.c mutt_zstrm.c \
	bcache.h browser.h hcache.h mbyte.h monitor.h mutt_idna.h remailer.h url.h \
  mutt_lisp.h

EXTRA_DIST = COPYRIGHT GPL OPS OPS.PGP OPS.CRYPT OPS.SMIME TODO UPDATING \
	configure account.h \
	attach.h buffer.h buffy.h charset.h color.h compress.h copy.h crypthash.h \
	dotlock.h functions.h gen_defs gettext.h \
	globals.h hash.h history.h init.h keymap.h mutt_crypt.h \
	mailbox.h mapping.h md5.h mime.h mutt.h mutt_curses.h mutt_menu.h \
	mutt_regex.h mutt_sasl.h mutt_sasl_gnu.h mutt_socket.h mutt_ssl.h \
	mutt_tunnel.h \
	mx.h pager.h pgp.h pop.h protos.h rfc1524.h rfc2047.h \
	rfc2231.h rfc822.h rfc3676.h sha1.h sort.h mime.types VERSION prepare \
	_mutt_regex.h OPS.MIX README.SECURITY remailer.c remailer.h browser.h \
	mbyte.h lib.h extlib.c pgpewrap.c smime_keys.pl pgplib.h \
	README.SSL smime.h group.h mutt_zstrm.h send.h background.h \
	muttbug pgppacket.h depcomp ascii.h PATCHES patchlist.sh \
	ChangeLog mkchangelog.sh mkreldate.sh mutt_idna.h sidebar.h OPS.SIDEBAR \
	regex.c crypt-gpgme.h hcachever.pl \
	txt2c.c txt2c.sh version.sh check_sec.sh

EXTRA_SCRIPTS = smime_keys

mutt_dotlock_SOURCES = mutt_dotlock.c
mutt_dotlock_LDADD = $(LIBOBJS)
mutt_dotlock_DEPENDENCIES = $(LIBOBJS)

mutt_pgpring_SOURCES = pgppubring.c pgplib.c lib.c extlib.c sha1.c md5.c pgppacket.c ascii.c
mutt_pgpring_LDADD = $(LIBOBJS) $(LIBINTL)
mutt_pgpring_DEPENDENCIES = $(LIBOBJS) $(INTLDEPS)

txt2c_SOURCES = txt2c.c
txt2c_LDADD =

noinst_PROGRAMS = txt2c

mutt_dotlock.c: dotlock.c
	cp $(srcdir)/dotlock.c mutt_dotlock.c

# With autoconf 2.71, the "ac_cs_config=" line in config.status may contain
# sequences like '\'' when using a configure argument with spaces in it,
# for instance CFLAGS with several options. Thus it is not sufficient to
# get this line with grep; one needs to eval it and output the result.
conststrings.c: txt2c config.status
	( \
		$(CC) -v || \
		$(CC) --version || \
		$(CC) -V || \
		echo "unknown compiler"; \
	) 2>&1 | ${srcdir}/txt2c.sh cc_version >conststrings_c
	echo "$(CFLAGS)" | ${srcdir}/txt2c.sh cc_cflags >>conststrings_c
	( eval "`grep '^ac_cs_config=' config.status`" && echo $$ac_cs_config; ) | ${srcdir}/txt2c.sh configure_options >>conststrings_c
	mv -f conststrings_c conststrings.c

CLEANFILES = mutt_dotlock.c $(BUILT_SOURCES)

DISTCLEANFILES= flea smime_keys txt2c po/$(PACKAGE).pot

ACLOCAL_AMFLAGS = -I m4

LDADD = $(LIBOBJS) $(LIBINTL)

flea: $(srcdir)/muttbug
	cp $(srcdir)/muttbug flea
	chmod +x flea

smime_keys: $(srcdir)/smime_keys.pl
	cp $(srcdir)/smime_keys.pl smime_keys
	chmod +x smime_keys

keymap_defs.h: $(OPS) config.h $(srcdir)/gen_defs
	$(srcdir)/gen_defs $(OPS) > keymap_defs.h

# If we have GNU make, we can use the FORCE target to enable
# automatic rebuilding of version.h after a commit.
if GNU_MAKE
version.h: FORCE
	echo '#define MUTT_VERSION "'`sh "$(srcdir)/version.sh"`'"' > $@.tmp
	cmp -s $@ $@.tmp && rm -f $@.tmp || mv $@.tmp $@
FORCE:
# On some other versions of make, such as OpenBSD, invoking the
# version.h target always retriggers targets with that prerequisite, which
# causes installation issues.
else
version.h: $(srcdir)/version.sh
	echo '#define MUTT_VERSION "'`sh "$(srcdir)/version.sh"`'"' > version.h
endif

reldate.h: $(srcdir)/mkreldate.sh $(srcdir)/ChangeLog
	echo 'const char *ReleaseDate = "'`(cd $(srcdir) && ./mkreldate.sh)`'";' > reldate.h

# The '#undef ENABLE_NLS' is to work around an automake ordering issue:
# BUILT_SOURCES are processed before SUBDIRS.
# If configured with --with-included-gettext this means that intl will
# not have generated libintl.h yet, and mutt.h -> lib.h will generate
# an error.
hcversion.h: $(srcdir)/mutt.h $(srcdir)/rfc822.h $(srcdir)/buffer.h config.h $(srcdir)/hcachever.pl $(srcdir)/color.h
	( echo '#include "config.h"'; echo '#undef ENABLE_NLS'; echo '#include "mutt.h"'; ) \
	| $(CPP) $(DEFS) $(DEFAULT_INCLUDES) $(INCLUDES) $(AM_CPPFLAGS) $(CPPFLAGS) - | perl $(srcdir)/hcachever.pl > hcversion.h

patchlist.c: $(srcdir)/PATCHES $(srcdir)/patchlist.sh
	$(srcdir)/patchlist.sh < $(srcdir)/PATCHES > patchlist.c

install-exec-hook:
	if test -f $(DESTDIR)$(bindir)/mutt.dotlock && test -f $(DESTDIR)$(bindir)/mutt_dotlock ; then	\
		rm -f $(DESTDIR)$(bindir)/mutt.dotlock ;		\
		ln -sf $(DESTDIR)$(bindir)/mutt_dotlock $(DESTDIR)$(bindir)/mutt.dotlock ; \
	fi
	if test -f $(DESTDIR)$(bindir)/mutt_dotlock && test x$(DOTLOCK_GROUP) != x ; then \
		chgrp $(DOTLOCK_GROUP) $(DESTDIR)$(bindir)/mutt_dotlock && \
		chmod $(DOTLOCK_PERMISSION) $(DESTDIR)$(bindir)/mutt_dotlock || \
		{ echo "Can't fix mutt_dotlock's permissions!  This is required to lock mailboxes in the mail spool directory." >&2 ; exit 1 ; } \
	fi

install-data-local:
	$(MKDIR_P) $(DESTDIR)$(sysconfdir)
	$(INSTALL) -m 644 $(srcdir)/mime.types $(DESTDIR)$(sysconfdir)/mime.types.dist
	-if [ ! -f $(DESTDIR)$(sysconfdir)/mime.types ]; then \
		$(INSTALL) -m 644 $(srcdir)/mime.types $(DESTDIR)$(sysconfdir); \
	fi

uninstall-local:
	for i in mime.types ; do \
		if cmp -s $(DESTDIR)$(sysconfdir)/$$i.dist $(DESTDIR)$(sysconfdir)/$$i ; then \
			rm $(DESTDIR)$(sysconfdir)/$$i ; \
		fi ; \
		rm $(DESTDIR)$(sysconfdir)/$${i}.dist ; \
	done

pclean:
	cat /dev/null > $(top_srcdir)/PATCHES

check-security:
	(cd $(top_srcdir) && ./check_sec.sh)

update-changelog:
	(cd $(top_srcdir); \
	(sh ./mkchangelog.sh; echo; cat ChangeLog) > ChangeLog.$$$$ && mv ChangeLog.$$$$ ChangeLog; \
	$${VISUAL:-vi} ChangeLog)

mutt-dist:
	(cd $(srcdir) && ./build-release )

update-doc:
	(cd doc && $(MAKE) update-doc)

shellcheck:
	(find . -name \*.sh && echo "gen_defs muttbug prepare") | xargs shellcheck --exclude=SC2003,SC2006,SC2086,SC2162,SC2046

.PHONY: commit pclean check-security shellcheck
