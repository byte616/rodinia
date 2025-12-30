# ===== Target =====
EXE := backprop_cuda

# ===== Compiler =====
CXX := clang++
CC  := clang

# ===== CUDA =====
CUDA_PATH ?= /usr/local/cuda-12
CUDA_ARCH := sm_89

# ===== OpenMP =====
OMP_PATH := $(HOME)/llvm-omp-19

# ===== Flags =====
CXXFLAGS := -O2 -fopenmp -I$(OMP_PATH)/include
CFLAGS   := -O2 -fopenmp -I$(OMP_PATH)/include
CUDAFLAGS := \
	--cuda-gpu-arch=$(CUDA_ARCH) \
	--cuda-path=$(CUDA_PATH) \
	

INCLUDES := -I$(CUDA_PATH)/include
LDFLAGS := \
	-no-pie \
	-L$(CUDA_PATH)/lib64 \
	-lcudart -ldl -lrt -pthread \
	-L$(OMP_PATH)/lib -lomp

# ===== Sources =====
CU_SRCS  := backprop_cuda.cu
C_SRCS   := backprop.c facetrain.c imagenet.c

OBJS := $(CU_SRCS:.cu=.o) $(C_SRCS:.c=.o)

# ===== Rules =====
all: $(EXE)

$(EXE): $(OBJS)
	$(CXX) $^ $(LDFLAGS) -o $@

# ---- CUDA compile ----
%.o: %.cu
	$(CXX) $(CXXFLAGS) $(CUDAFLAGS) $(INCLUDES) -c $< -o $@

# ---- C compile ----
%.o: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -c $< -o $@

.PHONY: clean
clean:
	rm -f $(EXE) $(OBJS)
