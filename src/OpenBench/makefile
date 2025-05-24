EVALFILE = ../../data/quantised.bin
BUILD_FLAGS = -O3 -std=c++20 -march=native -Wall -g -flto -DEVALFILE=\"$(EVALFILE)\"
EXE = bbd
SRC = $(wildcard ../*.cpp)
OBJ = $(SRC:.cpp=.o)

%.o: %.cpp
	clang++ -fuse-ld=lld $(BUILD_FLAGS) $(RFLAGS) -o $@ -c $<

native: $(OBJ)
	clang++ -fuse-ld=lld $(OBJ) $(BUILD_FLAGS) -o $(EXE)
