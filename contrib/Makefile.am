# $Id$

subdir = contrib

SAMPLES = Mush.rc Pine.rc gpg.rc pgp2.rc pgp5.rc pgp6.rc Tin.rc \
	sample.mailcap sample.muttrc sample.muttrc-sidebar sample.muttrc-tlr \
	sample.muttrc-compress sample.muttrc-starter \
  sample.vimrc-sidebar colors.default colors.linux smime.rc \
	smime_keys_test.pl mutt_xtitle markdown2html \
  bgedit-detectgui.sh bgedit-screen-tmux.sh \
 	mutt_oauth2.py mutt_oauth2.py.README

EXTRA_DIST = language.txt language50.txt  \
	patch.slang-1.2.2.keypad.1	\
	$(SAMPLES) \
	iconv/README \
	iconv/make.sh

install-data-local:
	$(MKDIR_P) $(DESTDIR)$(docdir)/samples $(DESTDIR)$(docdir)/samples/iconv
	for f in $(SAMPLES) ; do \
		$(INSTALL) -m 644 $(srcdir)/$$f $(DESTDIR)$(docdir)/samples ;	\
	done
	for f in $(srcdir)/iconv/*.rc ; do					\
		$(INSTALL) -m 644 $$f $(DESTDIR)$(docdir)/samples/iconv	  ;	\
	done

uninstall-local:
	for f in $(SAMPLES) ; do \
		rm -f $(DESTDIR)$(docdir)/samples/$$f ; \
	done
	-rm -rf $(DESTDIR)$(docdir)/samples/iconv
	-rmdir $(DESTDIR)$(docdir)/samples
	-rmdir $(DESTDIR)$(docdir)

distdir = ../$(PACKAGE)-$(VERSION)/$(subdir)
dist-hook: Makefile $(DISTFILES)
	mkdir -p $(distdir)/iconv
	for file in $(srcdir)/iconv/*.rc ; do					\
		ln $$file $(distdir)/iconv 2> /dev/null				\
			|| cp -p $$file $(distdir)/iconv ;			\
	done
