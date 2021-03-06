CC = gcc
CFLAGS = -Wall -g -pedantic -std=gnu99 -pthread
IFLAGS = -Iinclude

SPATH = src
OPATH = obj
BPATH = bin
IPATH = include
DPATH = doc

vpath %.c $(SPATH)/
vpath %.h $(IPATH)/
vpath %.o $(OPATH)/

all : main monsem_test

main : main.o commun.o societe.o tampon.o limite.o monsem.o | bin
	$(CC) $(CFLAGS) -o $(BPATH)/$@ $(OPATH)/main.o $(OPATH)/commun.o $(OPATH)/societe.o $(OPATH)/tampon.o $(OPATH)/limite.o $(OPATH)/monsem.o

monsem_test : monsem_test_binaire.o monsem_test_bloquant.o monsem_test_off.o monsem.o | bin
	$(CC) $(CFLAGS) -o $(BPATH)/$@_binaire $(OPATH)/monsem.o $(OPATH)/monsem_test_binaire.o
	$(CC) $(CFLAGS) -o $(BPATH)/$@_bloquant $(OPATH)/monsem.o $(OPATH)/monsem_test_bloquant.o
	$(CC) $(CFLAGS) -o $(BPATH)/$@_off $(OPATH)/monsem.o $(OPATH)/monsem_test_off.o

monsem_test_binaire.o : monsem_test.c monsem.h | obj
	gcc $(CFLAGS) -D _TEST_MONSEM_BINAIRE -U _TEST_MONSEM_BLOQUANT -o $(OPATH)/$@ -c $< $(IFLAGS)
monsem_test_bloquant.o : monsem_test.c monsem.h | obj
	gcc $(CFLAGS) -U _TEST_MONSEM_BINAIRE -D _TEST_MONSEM_BLOQUANT -o $(OPATH)/$@ -c $< $(IFLAGS)
monsem_test_off.o : monsem_test.c monsem.h | obj
	gcc $(CFLAGS) -U _TEST_MONSEM_BLOQUANT -U _TEST_MONSEM_BINAIRE -o $(OPATH)/$@ -c $< $(IFLAGS)

%.o : %.c | obj
	gcc $(CFLAGS) -o $(OPATH)/$@ -c $< $(IFLAGS)

monsem.o : monsem.c monsem.h
limite.o : limite.c limite.h commun.h
tampon.o : tampon.c tampon.h commun.h limite.h
societe.o : societe.c societe.h commun.h limite.h tampon.h
commun.o : commun.c commun.h limite.h
main.o : main.c commun.h limite.h societe.h tampon.h

obj :
	@mkdir -p $(OPATH)

bin :
	@mkdir -p $(BPATH)
	
doc :
	doxygen

clean :
	@rm -rf $(OPATH) $(BPATH)
	
cleandoc :
	@rm -rf $(DPATH)
	
cleanall : clean cleandoc
	@echo "Everything cleaned up"

archive :
	tar -cvzf $(shell basename `pwd`).tar.gz src include makefile Doxyfile
