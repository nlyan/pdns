AM_CPPFLAGS = $(LUA_CFLAGS) $(LIBEDIT_CFLAGS) $(YAHTTP_CFLAGS) $(BOOST_CPPFLAGS) $(SANITIZER_FLAGS) -O3 -Wall -pthread -DSYSCONFDIR=\"${sysconfdir}\"

ACLOCAL_AMFLAGS = -I m4

SUBDIRS=ext/yahttp

dnslabeltext.cc: dnslabeltext.rl
	$(AM_V_GEN)$(RAGEL) $< -o dnslabeltext.cc

BUILT_SOURCES=htmlfiles.h

htmlfiles.h: html/*
	./incfiles > $@

SRC_JS_FILES := $(wildcard src_js/*.js)
MIN_JS_FILES := $(patsubst src_js/%.js,html/js/%.min.js,$(SRC_JS_FILES))

html/js/%.min.js: src_js/%.js
	uglifyjs $< > $@

min_js: $(MIN_JS_FILES)

if HAVE_RE2
AM_CPPFLAGS += $(RE2_CFLAGS)
endif


EXTRA_DIST=dnslabeltext.rl \
	   dnsdistconf.lua \
	   README.md \
	   delaypipe.cc delaypipe.hh \
	   html \
	   dnsdist.1.md \
	   .version \
	   contrib \
	   build-aux/gen-version \
	   ext/incbin/UNLICENSE \
	   incfiles \
	   src_js

if UNIT_TESTS
bin_PROGRAMS = dnsdist testrunner
else
bin_PROGRAMS = dnsdist
endif

dnsdist_SOURCES = \
	base64.hh \
	dns.cc dns.hh \
	dnscrypt.cc dnscrypt.hh \
	dnsdist.cc dnsdist.hh \
	dnsdist-cache.cc dnsdist-cache.hh \
	dnsdist-carbon.cc \
	dnsdist-console.cc \
	dnsdist-dnscrypt.cc \
	dnsdist-ecs.cc dnsdist-ecs.hh \
	dnsdist-lua.cc \
	dnsdist-lua2.cc \
	dnsdist-rings.cc \
	dnsdist-tcp.cc \
	dnsdist-web.cc \
	dnslabeltext.cc \
	dnsname.cc dnsname.hh \
	dnsparser.hh dnsparser.cc \
	dnsrulactions.hh \
	dnswriter.cc dnswriter.hh \
	dolog.hh \
	ednssubnet.cc ednssubnet.hh \
	iputils.cc iputils.hh \
	lock.hh \
	misc.cc misc.hh \
	htmlfiles.h \
	namespaces.hh \
	pdnsexception.hh \
	qtype.cc qtype.hh \
	sholder.hh \
	sodcrypto.cc sodcrypto.hh \
	sstuff.hh \
	ext/luawrapper/include/LuaContext.hpp \
	ext/json11/json11.cpp \
	ext/json11/json11.hpp \
	ext/incbin/incbin.h

dnsdist_LDFLAGS = \
	$(AM_LDFLAGS) \
	$(PROGRAM_LDFLAGS) \
	-pthread 

dnsdist_LDADD = \
	$(LUA_LIBS) \
	$(LIBEDIT_LIBS) \
	$(RT_LIBS) \
	$(YAHTTP_LIBS) \
	$(LIBSODIUM_LIBS) \
	$(SANITIZER_FLAGS)

if HAVE_RE2
dnsdist_LDADD += $(RE2_LIBS)
endif


testrunner_SOURCES = \
	base64.hh \
	dns.hh \
	test-base64_cc.cc \
	test-dnsdist_cc.cc \
	test-dnsdistpacketcache_cc.cc \
	test-dnscrypt_cc.cc \
	dnsdist.hh \
	dnsdist-cache.cc dnsdist-cache.hh \
	dnsdist-ecs.cc dnsdist-ecs.hh \
	dnscrypt.cc dnscrypt.hh \
	dnslabeltext.cc \
	dnsname.cc dnsname.hh \
	dnsparser.hh dnsparser.cc \
	dnswriter.cc dnswriter.hh \
	dolog.hh \
	ednssubnet.cc ednssubnet.hh \
	iputils.cc iputils.hh \
	misc.cc misc.hh \
	namespaces.hh \
	pdnsexception.hh \
	qtype.cc qtype.hh \
	sholder.hh \
	sodcrypto.cc \
	sstuff.hh \
	testrunner.cc

testrunner_LDFLAGS = \
	$(AM_LDFLAGS) \
	$(PROGRAM_LDFLAGS) \
	$(BOOST_UNIT_TEST_FRAMEWORK_LDFLAGS) \
	-pthread

testrunner_LDADD = \
	$(BOOST_UNIT_TEST_FRAMEWORK_LIBS) \
	$(LIBSODIUM_LIBS) \
	$(RT_LIBS) \
	$(SANITIZER_FLAGS)

MANPAGES=dnsdist.1

dist_man_MANS=$(MANPAGES)

if HAVE_PANDOC
$(MANPAGES): %: %.md
	$(AM_V_GEN)$(PANDOC) -s -t man $< -o $@
else
if HAVE_MANPAGES
#nothing
else
$(MANPAGES):
	echo "You need pandoc to generate the manpages"
	exit 1
endif
endif
