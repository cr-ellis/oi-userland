#
# This file and its contents are supplied under the terms of the
# Common Development and Distribution License ("CDDL)". You may
# only use this file in accordance with the terms of the CDDL.
#
# A full copy of the text of the CDDL should have accompanied this
# source. A copy of the CDDL is also available via the Internet at
# http://www.illumos.org/license/CDDL.
#

#
# Copyright 2011-2013, EveryCity Ltd. All rights reserved.
# 

include ../../make-rules/shared-macros.mk

CMAKE= cmake

COMPONENT_NAME=		percona-server
COMPONENT_VERSION=	5.5.32-31.0
IPS_COMPONENT_VERSION=	5.5.32.31.0
COMPONENT_SRC=		Percona-Server-5.5.32-rel31.0
COMPONENT_ARCHIVE=	$(COMPONENT_SRC).tar.gz
COMPONENT_PROJECT_URL=	http://www.percona.com/
COMPONENT_ARCHIVE_HASH=	sha256:e34676cea5ceb398258408e31e8527724845f83bbbf1f22308f5069304118e8f
COMPONENT_ARCHIVE_URL=	http://www.percona.com/redir/downloads/Percona-Server-5.5/Percona-Server-$(COMPONENT_VERSION)/source/$(COMPONENT_ARCHIVE)

include ../../make-rules/prep.mk
include ../../make-rules/configure.mk
include ../../make-rules/ips.mk


CONFIGURE_PREFIX=/usr/percona-server/5.5
CONFDIR=/etc/percona-server/5.5
DATA_PREFIX=/var/percona-server/5.5

CMAKE_BINDIR.32 =	bin
CMAKE_BINDIR.64 =	bin/$(MACH64)
CMAKE_LIBDIR.32 =	lib
CMAKE_LIBDIR.64 =	lib/$(MACH64)
CMAKE_PLUGINDIR.32 =	lib/plugin
CMAKE_PLUGINDIR.64 =	lib/$(MACH64)/plugin

CMAKE_OPTIONS+= -DCMAKE_C_COMPILER=$(CC)
CMAKE_OPTIONS+= -DCMAKE_CXX_COMPILER=$(CXX)
CMAKE_OPTIONS+= -DCMAKE_C_FLAGS=$(CFLAGS)
CMAKE_OPTIONS+= -DCMAKE_CXX_FLAGS=$(CFLAGS)

CMAKE_OPTIONS+=	-DWITH_SSL=system

CMAKE_OPTIONS+= -DINSTALL_LAYOUT=SVR4
CMAKE_OPTIONS+= -DCMAKE_INSTALL_PREFIX=$(CONFIGURE_PREFIX)
CMAKE_OPTIONS+= -DINSTALL_BINDIR=$(CMAKE_BINDIR.$(BITS))
CMAKE_OPTIONS+= -DINSTALL_INFODIR=bin
CMAKE_OPTIONS+= -DINSTALL_INCLUDEDIR=include
CMAKE_OPTIONS+= -DINSTALL_LIBDIR=$(CMAKE_LIBDIR.$(BITS))
CMAKE_OPTIONS+= -DINSTALL_PLUGINDIR=$(CMAKE_PLUGINDIR.$(BITS))
CMAKE_OPTIONS+= -DINSTALL_MANDIR=man
CMAKE_OPTIONS+= -DINSTALL_SHAREDIR=share
CMAKE_OPTIONS+= -DINSTALL_MYSQLSHAREDIR=share
CMAKE_OPTIONS+= -DINSTALL_SBINDIR=$(CMAKE_BINDIR.$(BITS))
CMAKE_OPTIONS+= -DINSTALL_SCRIPTDIR=$(CMAKE_BINDIR.$(BITS))
CMAKE_OPTIONS+= -DMYSQL_DATADIR=$(DATA_PREFIX)/data
CMAKE_OPTIONS+= -DSYSCONFDIR=$(CONFDIR)
CMAKE_OPTIONS+= -DENABLE_DTRACE=on
CMAKE_OPTIONS+= -DINSTALL_SUPPORTFILESDIR=share/mysql

#LDFLAGS.32	+=	-L/usr/lib/mysql/5.5/lib -R/usr/lib/mysql/5.5/lib
#LDFLAGS.64	+=	-L/usr/lib/mysql/5.5/lib/$(MACH64) -R/usr/lib/mysql/5.5/lib/$(MACH64)

# gcc invocation in some link.txt file is missing -m$(BITS) parameter
# and it makes 64-bit build to fail, this is a quick hack to make it build
# for now, cmake build system needs a proper fix

COMPONENT_POST_CONFIGURE_ACTION.32=
COMPONENT_POST_CONFIGURE_ACTION.64= find $(@D) -type f -name link.txt \
    -exec perl -pi -e 's/gcc\ \ -fPIC/\/gcc\ -m64\ -fPIC/' {} \;
COMPONENT_POST_CONFIGURE_ACTION=$(COMPONENT_POST_CONFIGURE_ACTION.$(BITS))

$(BUILD_DIR)/%/.configured:	$(SOURCE_DIR)/.prep
	($(RM) -rf $(@D) ; $(MKDIR) $(@D))
	$(COMPONENT_PRE_CONFIGURE_ACTION)
	(cd $(@D) ; $(ENV) $(CONFIGURE_ENV) $(CMAKE) $(CMAKE_OPTIONS) $(SOURCE_DIR))
	$(COMPONENT_POST_CONFIGURE_ACTION)
	$(TOUCH) $@

build:		$(BUILD_32_and_64)

install:	$(INSTALL_32_and_64)
