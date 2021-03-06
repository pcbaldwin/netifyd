# Netify Agent Source Automake File
# Copyright (C) 2016-2020 eGloo, Incorporated
#
# This is free software, licensed under the GNU General Public License v3.

AUTOMAKE_OPTIONS = foreign
ACLOCAL_AMFLAGS = -I m4
AM_CPPFLAGS = $(CPPFLAGS) -D_GNU_SOURCE -I $(top_srcdir)/include \
	-I $(top_srcdir)/libs/ndpi/src/include -I $(top_srcdir)/libs/inih/cpp \
	$(NLOHMANN_JSON_CPPFLAGS) $(LIBCURL_CFLAGS) $(ZLIB_CFLAGS) $(LIBTCMALLOC_CFLAGS) \
	-D_ND_INTERNAL=1 \
	-D'ND_CONF_FILE_NAME="$(sysconfdir)/$(PACKAGE_TARNAME).conf"' \
	-D'ND_DATADIR="$(datadir)/$(PACKAGE_TARNAME)"' \
	-D'ND_PERSISTENT_STATEDIR="$(persistentstatedir)"' \
	-D'ND_PID_FILE_NAME="$(volatilestatedir)/$(PACKAGE_TARNAME).pid"' \
	-D'ND_VOLATILE_STATEDIR="$(volatilestatedir)"' \
	-D NDPI_LIB_COMPILATION=1

if USE_CONNTRACK
AM_CPPFLAGS += $(LIBNETFILTER_CONNTRACK_CFLAGS) $(LIBMNL_CFLAGS)
endif

lib_LTLIBRARIES = libnetifyd.la
libnetifyd_la_SOURCES = nd-base64.cpp nd-capture.cpp nd-detection.cpp nd-dhc.cpp \
	nd-fhc.cpp nd-flow.cpp nd-json.cpp nd-ndpi.cpp nd-sha1.c nd-sink.cpp nd-socket.cpp \
	nd-thread.cpp nd-util.cpp

libnetifyd_la_LIBADD = $(top_srcdir)/libs/ndpi/src/lib/libndpi.la

if USE_INOTIFY
libnetifyd_la_SOURCES += nd-inotify.cpp
endif

if USE_NETLINK
libnetifyd_la_SOURCES += nd-netlink.cpp
endif

if USE_PLUGINS
libnetifyd_la_SOURCES += nd-plugin.cpp
endif

if USE_CONNTRACK
libnetifyd_la_SOURCES += nd-conntrack.cpp
libnetifyd_la_LIBADD += $(LIBNETFILTER_CONNTRACK_LIBS) $(LIBMNL_LIBS)
endif

sbin_PROGRAMS = netifyd
netifyd_SOURCES = netifyd.cpp
netifyd_LDFLAGS = $(LIBCURL_LIBS) $(ZLIB_LIBS)
netifyd_LDADD = ./libnetifyd.la $(top_srcdir)/libs/inih/libini.la

if USE_LIBTCMALLOC
# XXX: Recommended compiler flags
AM_CPPFLAGS += $(LIBTCMALLOC_CFLAGS) -fno-builtin-malloc -fno-builtin-calloc -fno-builtin-realloc -fno-builtin-free
# XXX: It's important that this library be linked after all others.
netifyd_LDADD += $(LIBTCMALLOC_LIBS)
endif

# For debugging ns_initparse
#netifyd_SOURCES += ns-parse.c
