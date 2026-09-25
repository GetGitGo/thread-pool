# GNU make，在 MINGW64 终端、仓库根目录执行:
#   make
#   make STD=c++20
#   make clean
# nmake 不读本文件，仍用 Makefile。

CXX = c++
STD = c++17
OUTDIR = build
TARGET = $(OUTDIR)/GNU_BS_thread_pool_test.exe
SRC = tests/BS_thread_pool_test.cpp

CXXFLAGS = -std=$(STD) -Wall -Wextra -Wconversion -Wsign-conversion -Wpedantic -Wshadow \
	-Wuseless-cast -Wnrvo -march=native -fdiagnostics-color=always -s \
	-DBS_THREAD_POOL_NATIVE_EXTENSIONS -I include

.PHONY: all clean

all: $(TARGET)

$(TARGET): $(SRC)
	mkdir -p $(OUTDIR)
	$(CXX) $(CXXFLAGS) -o $@ $<

clean:
	rm -f $(TARGET)
