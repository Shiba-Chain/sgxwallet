## This line must come first when building an Intel SGX enclave.

include $(top_srcdir)/build-aux/sgx_enclave.am

## It sets the following Automake variables:
##
##   EXEEXT=.so
##   AM_CPPFLAGS = -I$(SGXSDK_INCDIR)
##   AM_LDFLAGS = -L$(SGXSDK_LIBDIR)
##   libexec_PROGRAMS = $(ENCLAVE)
##   CLEANFILES = $(ENCLAVE).signed.so
##
## and places required compiler flags in:
##
##   AM_CFLAGS
##   AM_CXXFLAGS
##
## It adds a pattern rule for building proxy functions from EDL files:
##
##   %_u.h %_u.c: %.edl
##
## And creates build targets for a signed enclave, generating a
## temporary private signing key, and a basic enclave config file:
##
##   $(ENCLAVE_CONFIG):
##   $(ENCLAVE_KEY):
##   $(ENCLAVE).signed$(EXEEXT): $(ENCLAVE)$(EXEEXT)
##
## And sets these Makefile variables:
##
##   SGXSDK
##   SGXSDK_BINDIR
##   SGXSDK_INCDIR
##   SGXSDK_LIBDIR
##   SGXSSL
##   SGXSSL_BINDIR
##   SGXSSL_INCDIR
##   SGXSSL_LIBDIR
##   SGX_TRTS_LIB
##   SGX_TSERVICE_LIB
##   SGX_EDGER8R
##   SGX_SIGN


## The name of your enclave, enclave config file, and private key
## file go in these variables. The ENCLAVE variable creates the
## following automake target defn:
##
##    libexec_PROGRAMS=$(ENCLAVE)

ENCLAVE=secure_enclave
ENCLAVE_CONFIG=$(ENCLAVE).config.xml
ENCLAVE_KEY=test_insecure_private_key.pem       #$(ENCLAVE)_private.pem


## Provide additional flags to sgx_sign when signing the enclave.
## This is almost never necessary. If you don't know if you need
## this, you probably don't.

## SGX_SIGN_FLAGS =


## Additional Automake flags needed to build the enclave.

AM_CPPFLAGS += -O2 -Wall -Wno-implicit-function-declaration $(TGMP_CPPFLAGS) -I./third_party/SCIPR -I../third_party/SCIPR -I../sgx-sdk-build/sgxsdk/include/libcxx 
AM_CXXFLAGS += -fno-builtin -fstack-protector-strong


## Additional files to remove with 'make clean'. This list needs
## to include your edger8r genreated files.

CLEANFILES+= secure_enclave_t.c secure_enclave_t.h


## Supply additional flags to edger8r here.
##
## SGX_EDGER8R_FLAGS=


## Put your sources here. Don't forget to list the _t.c and _t.h
## files. You can't use the $(ENCLAVE) variable in the build
## target name (i.e., $(ENCLAVE)_SOURCES will not work).

secure_enclave_SOURCES = secure_enclave_t.c secure_enclave_t.h \
	secure_enclave.c \
	Curves.c  NumberTheory.c Point.c Signature.c DHDkg.c AESUtils.c \
    DKGUtils.cpp  TEUtils.cpp EnclaveCommon.cpp  DomainParameters.cpp ../third_party/SCIPR/libff/algebra/curves/alt_bn128/alt_bn128_init.cpp \
                ../third_party/SCIPR/libff/algebra/curves/alt_bn128/alt_bn128_g2.cpp \
                ../third_party/SCIPR/libff/algebra/curves/alt_bn128/alt_bn128_g1.cpp $(ENCLAVE_KEY) $(ENCLAVE_CONFIG)


## Add additional linker flags to AM_LDFLAGS here. Don't put
## libraries flags here (see below).
##
## Be sure to use += to add to, and not replace, the default
## AM_LDFLAGS.

AM_LDFLAGS += $(TGMP_LDFLAGS) -L./tgmp-build/lib -L../tgmp-build/lib


## This line is REQUIRED. It can't be generically defined for
## automake, so you must specify it for your enclave. Note that you
## can't say $(ENCLAVE)_LDADD here: you must spell out the enclave name.
## If you add flags to it, you MUST include @SGX_ENCLAVE_LDADD@ as part
## of the definition to make sure you pick up the right linker flags
## and SGX trusted libraries.

secure_enclave_LDADD = @SGX_ENCLAVE_LDADD@


## Place any additional trusted libraries that your enclave may need in
## SGX_EXTRA_TLIBS. This will ensure they get place inside the
## --startgroup and --endgroup flags. (This would be where you'd add
## SGXSSL libraries, and your trusted c++ library

SGX_EXTRA_TLIBS=-lsgx_tgmp -lsgx_tservice -lsgx_urts -lsgx_tcxx 



## This line is OPTIONAL, and comes with a WARNING.
##
## In general, you shouldn't need to use the program-specific LDFLAGS
## instead of AM_LDFLAGS. But, if you need to, then you'll need to ensure
## @SGX_ENCLAVE_LDFLAGS@ is included in the definition as this will
## override AM_LDFLAGS.
##
## secure_enclave_LDFLAGS = @SGX_ENCLAVE_LDFLAGS@
##
