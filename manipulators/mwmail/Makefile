# Makefile
#
# $Id: $
#
# Xplico System
# By Gianluca Costa <g.costa@xplico.org>
# Copyright 2007-2011 Gianluca Costa & Andrea de Franceschi. Web: www.xplico.org
#
#
# This program is free software; you can redistribute it and/or
# modify it under the terms of the GNU General Public License
# as published by the Free Software Foundation; either version 2
# of the License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 59 Temple Place - Suite 330, Boston, MA  02111-1307, USA.
#

# sub directory
SUBDIRS = 

# manipulator name
MANIPULA_NAME = mwmail

# xplico library
XPL_LIB = $(ROOT_DIR)/common/libxplico_core.a $(ROOT_DIR)/dispatch/libxplico_dispatch.a $(ROOT_DIR)/dissectors/libxplico_dissector.a

# src files
SRC = mmain.c mpei.c analyse.c
PYSRC = wbm_aol.py wbm_live.py wbm_yahoo.py
PYSRC += wbm_libero.py wbm_yahoo_android.py wbm_rediff.py wbm_aol_v2.py wbm_gmail.py wbm_yahoo_v2.py wbm_rossoalice.py

# obj files
OBJ = $(SRC:.c=.o)

# dep 
DEP = ../core/mmain.c mpei.c analyse.c

# compilation
LDFLAGS =  -L$(ROOT_DIR) -ldl -lpthread -lz -lssl -lcrypto -ltcmalloc

# cflags
CFLAGS += -I./include

# python complier
PYCMPL = python3 -m compileall

# verify GeoIP library source code
LDFLAGS += $(GEOIP_LIB)

# main cflags
MCFLAGS = $(CFLAGS) -DLOG_COMPONENT=-1

# To make it visible
export CC CCPP ROOT_DIR CFLAGS LDFLAGS INCLUDE_DIR


all: subdir $(MANIPULA_NAME) pycmpl pth


$(MANIPULA_NAME): $(OBJ) $(XPL_LIB)
	$(CC) $(MCFLAGS) -o $@ $(OBJ) $(XPL_LIB) $(LDFLAGS)
	rm -f modules config tmp
	cp $(MANIPULA_NAME) $(ROOT_DIR)

pth:
	@cp wbm_*.py $(ROOT_DIR)
	@cp wbm_*.pyc $(ROOT_DIR)

pycmpl: $(PYSRC:.py=.pyc)
	chmod +x *.pyc
	if [ -d "__pycache__" ]; then rm -rf __pycache__; fi

subdir:
	@for dir in $(SUBDIRS) ; \
	   do $(MAKE) -C $$dir || exit 1; \
	 done


clean:
	@for dir in $(SUBDIRS) ; do $(MAKE) -C $$dir clean || exit 1; done
	rm -f $(MANIPULA_NAME) $(ROOT_DIR)/$(MANIPULA_NAME) *.o *.so *~ include/*~ .depend *.expand
	rm -f $(ROOT_DIR)/wbm_*.pyc $(ROOT_DIR)/wbm_*.py
	rm -f modules config tmp xdecode
	rm -rf __pycache__
	rm wbm_*.pyc
	

install:
	cp $(MANIPULA_NAME) $(INSTALL_DIR)/bin
	cp wbm_*.py $(INSTALL_DIR)/bin
	cp wbm_*.pyc $(INSTALL_DIR)/bin

mmain.o: ../core/mmain.c
	$(CC) $(MCFLAGS) -c -o $@ $<

%.o: %.c
	$(CC) $(MCFLAGS) -c -o $@ $<

%.pyc: %.py
	rm -f $@
	$(PYCMPL) $< 
	if [ -d "__pycache__" ]; then mv __pycache__/*.pyc $@; fi

.depend: $(DEP)
	$(CC) -M $(MCFLAGS) $(DEP) > $@


sinclude .depend
