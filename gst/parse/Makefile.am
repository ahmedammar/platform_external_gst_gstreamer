# libgstparse.la is an optionally built helper library linked into core
noinst_LTLIBRARIES = libgstparse.la

CLEANFILES = grammar.tab.h grammar.output
EXTRA_DIST = \
	grammar.y \
	parse.l

# uncomment these lines to dist the generated sources
#BUILT_SOURCES = grammar.tab.h grammar.tab.c lex._gst_parse_yy.c
#libgstparse_la_SOURCES = lex._gst_parse_yy.c grammar.tab.c

# uncomment these lines to _NOT_ dist the generated sources
nodist_libgstparse_la_SOURCES = lex._gst_parse_yy.c grammar.tab.c
CLEANFILES += grammar.tab.c lex._gst_parse_yy.c

# can't use GST_ALL_CFLAGS here because that'd pull in -Werror
libgstparse_la_CFLAGS = -I$(top_srcdir) -I$(top_builddir) \
	$(GLIB_CFLAGS) $(XML_CFLAGS) $(GST_OPTION_CFLAGS)
libgstparse_la_LIBADD = $(GST_ALL_LIBS)

noinst_HEADERS = grammar.tab.h types.h

Android.mk: Makefile.am
	androgenizer -:PROJECT gstreamer -:STATIC libgstparse -:TAGS eng debug \
	 -:REL_TOP $(top_srcdir) -:ABS_TOP $(abs_top_srcdir) \
	 -:SOURCES $(libgstparse_la_SOURCES) $(nodist_libgstparse_la_SOURCES) \
	 -:CFLAGS $(DEFS) $(libgstparse_la_CFLAGS) \
	 -:LDFLAGS $(libgstparse_la_LIBADD) \
	> $@

grammar.tab.c grammar.tab.h: grammar.y
	$(AM_V_GEN)$(BISON_PATH) -d -v -p_gst_parse_yy $(srcdir)/grammar.y -o grammar.tab.c && \
	mv grammar.tab.c grammar.tab_tmp.c && \
	echo '#ifdef HAVE_CONFIG_H' > grammar.tab_tmp2.c && \
	echo '#include <config.h>' >> grammar.tab_tmp2.c && \
	echo '#endif' >> grammar.tab_tmp2.c && \
	cat grammar.tab_tmp.c >> grammar.tab_tmp2.c && \
	rm grammar.tab_tmp.c && \
	mv grammar.tab_tmp2.c grammar.tab.c

lex._gst_parse_yy.c: parse.l grammar.tab.h
	$(AM_V_GEN)$(FLEX_PATH) -P_gst_parse_yy $^ && \
	mv lex._gst_parse_yy.c lex._gst_parse_yy_tmp.c && \
	echo '#ifdef HAVE_CONFIG_H' > lex._gst_parse_yy_tmp2.c && \
	echo '#include <config.h>' >> lex._gst_parse_yy_tmp2.c && \
	echo '#endif' >> lex._gst_parse_yy_tmp2.c && \
	echo 'static inline int _gst_parse_yyget_column  (void * yyscanner);' >> lex._gst_parse_yy_tmp2.c && \
	echo 'static inline void _gst_parse_yyset_column (int  column_no , void * yyscanner);' >> lex._gst_parse_yy_tmp2.c && \
	cat lex._gst_parse_yy_tmp.c >> lex._gst_parse_yy_tmp2.c && \
	rm lex._gst_parse_yy_tmp.c && \
	mv lex._gst_parse_yy_tmp2.c lex._gst_parse_yy.c

.NOTPARALLEL:

