CXX	= clang++-devel

BIN     = ./lib/libthecore.a

IFLAGS = -Iinclude

CFLAGS = -Wall -Ofast -D_THREAD_SAFE -pipe -msse2 -mssse3 -m32 -fno-exceptions -std=c++1z -stdlib=libc++

LIBS =

CPP_FILES := $(wildcard src/*.cpp)

default: $(BIN)

$(BIN): $(CPP_FILES)
	ar cru $(BIN) $(CPP_FILES) $(LIBS)
	ranlib $(BIN)
	chmod 700 $(BIN)

release/%.o: src/%.cpp
	$(CXX) $(CFLAGS) $(IFLAGS) -c $< -o $@

clean:
	rm -f debug/*.o
	rm -f release/*.o
	rm -f $(BIN)
	rm -f $(DBGBIN)

dep:
	touch src/Depend
	$(CXX) $(CFLAGS) $(IFLAGS) -MM src/*.cpp > src/Depend