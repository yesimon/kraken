CXX = g++
CXXFLAGS = -Wall -O3

uname_S := $(shell sh -c 'uname -s 2>/dev/null || echo not')
ifeq ($(uname_S),Linux)
  CXXFLAGS += -fopenmp
endif
ifeq ($(uname_S),Darwin)
	ifeq (1,$(WITH_OMP))
		CXXFLAGS += -fopenmp
	endif
endif

PROGS = db_sort set_lcas classify make_seqid_to_taxid_map db_shrink

.PHONY: all install clean

all: $(PROGS)

install: $(PROGS)
	cp $(PROGS) $(KRAKEN_DIR)/libexec

clean:
	rm -f $(PROGS) *.o

db_shrink: krakendb.o quickfile.o

db_sort: krakendb.o quickfile.o

set_lcas: krakendb.o quickfile.o krakenutil.o seqreader.o

classify: krakendb.o quickfile.o krakenutil.o seqreader.o

make_seqid_to_taxid_map: quickfile.o

krakenutil.o: krakenutil.cpp krakenutil.hpp
	$(CXX) $(CXXFLAGS) -c krakenutil.cpp

krakendb.o: krakendb.cpp krakendb.hpp quickfile.hpp
	$(CXX) $(CXXFLAGS) -c krakendb.cpp

seqreader.o: seqreader.cpp seqreader.hpp quickfile.hpp
	$(CXX) $(CXXFLAGS) -c seqreader.cpp

quickfile.o: quickfile.cpp quickfile.hpp
	$(CXX) $(CXXFLAGS) -c quickfile.cpp
