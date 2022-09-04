AUTOMAKE_OPTIONS = foreign subdir-objects
AM_CFLAGS = -Wall -Werror -std=c11
AM_CXXFLAGS = -Wall -Werror -std=c++11 -I./linux -I$(srcdir)/Common -I$(srcdir)/Common/pbkdf2 -I$(srcdir)/linux -I$(srcdir)/LinuxPBA
SEDUTIL_COMMON_CODE = \
	Common/DtaAnnotatedDump.cpp Common/DtaAnnotatedDump.h \
	Common/DtaCommand.cpp Common/DtaCommand.h \
	Common/DtaConstants.h Common/DtaDev.cpp \
	Common/DtaDev.h Common/DtaDevEnterprise.cpp \
	Common/DtaDevEnterprise.h Common/DtaDevGeneric.cpp \
	Common/DtaDevGeneric.h Common/DtaDevOpal.cpp \
	Common/DtaDevOpal.h Common/DtaDevOpal1.cpp \
	Common/DtaDevOpal1.h Common/DtaDevOpal2.cpp \
	Common/DtaDevOpal2.h Common/DtaDevOpalite.cpp \
	Common/DtaDevOpalite.h Common/DtaDevPyrite1.cpp \
	Common/DtaDevPyrite1.h Common/DtaDevPyrite2.cpp \
	Common/DtaDevPyrite2.h Common/DtaDevRuby1.cpp \
	Common/DtaDevRuby1.h Common/DtaDiskType.cpp \
	Common/DtaDiskType.h Common/DtaEndianFixup.h \
	Common/DtaHashPwd.cpp Common/DtaHashPwd.h \
	Common/DtaHexDump.cpp Common/DtaHexDump.h \
	Common/DtaLexicon.h Common/DtaResponse.cpp \
	Common/DtaResponse.h Common/DtaSession.cpp \
	Common/DtaSession.h Common/DtaStructures.h \
	Common/log.h Common/pbkdf2/bitops.h \
	Common/pbkdf2/blockwise.c Common/pbkdf2/blockwise.h \
	Common/pbkdf2/chash.c Common/pbkdf2/chash.h \
	Common/pbkdf2/handy.h Common/pbkdf2/hmac.c \
	Common/pbkdf2/hmac.h Common/pbkdf2/pbkdf2.c \
	Common/pbkdf2/pbkdf2.h Common/pbkdf2/sha2.h \
	Common/pbkdf2/sha512.c Common/pbkdf2/tassert.h
SEDUTIL_LINUX_CODE = \
	linux/Version.h linux/os.h linux/DtaDevLinuxDrive.h \
	linux/DtaDevLinuxNvme.cpp linux/DtaDevLinuxSata.cpp \
	linux/DtaDevLinuxNvme.h linux/DtaDevLinuxSata.h \
	linux/DtaDevOS.cpp linux/DtaDevOS.h 
sbin_PROGRAMS = sedutil-cli linuxpba
sedutil_cli_SOURCES = Common/sedutil.cpp Common/DtaOptions.cpp \
	Common/DtaOptions.h \
	$(SEDUTIL_LINUX_CODE) \
	$(SEDUTIL_COMMON_CODE)
CLEANFILES = linux/Version.h
BUILT_SOURCES = linux/Version.h
#
linuxpba_SOURCES = LinuxPBA/LinuxPBA.cpp LinuxPBA/GetPassPhrase.cpp LinuxPBA/UnlockSEDs.cpp \
	LinuxPBA/GetPassPhrase.h LinuxPBA/UnlockSEDs.h \
	$(SEDUTIL_LINUX_CODE) \
	$(SEDUTIL_COMMON_CODE)
EXTRA_DIST = linux/GitVersion.sh linux/PSIDRevert_LINUX.txt linux/TestSuite.sh README.md docs/sedutil-cli.8
man_MANS = docs/sedutil-cli.8
linux/Version.h:
	$(srcdir)/linux/GitVersion.sh > linux/Version.h
.INTERMEDIATE: linux/Version.h
maintainer-clean-local:
	rm config.status configure config.log
	rm Makefile
	rm -r autom4te.cache/
	rm aclocal.m4
	rm compile install-sh missing Makefile.in
	rm -rf depcomp
