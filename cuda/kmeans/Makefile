# ===== Target =====
EXE := kmeans

# ===== Compiler =====
CXX := clang++
CC  := clang

# ===== CUDA =====
CUDA_PATH ?= /usr/local/cuda-12
CUDA_ARCH := sm_89

# ===== OpenMP =====
OMP_PATH := $(HOME)/llvm-omp-19

# ===== Flags =====
CXXFLAGS := -O2 -fopenmp -I$(OMP_PATH)/include -fno-PIE
CFLAGS   := -O2 -fopenmp -I$(OMP_PATH)/include -fno-PIE
CUDAFLAGS := \
	--cuda-gpu-arch=$(CUDA_ARCH) \
	--cuda-path=$(CUDA_PATH) \

INCLUDES := -I$(CUDA_PATH)/include
LDFLAGS := \
	-no-pie \
	-L$(CUDA_PATH)/lib64 \
	-lcudart -ldl -lrt -pthread \
	-L$(OMP_PATH)/lib -lomp

# ===== Source =====
CU_SRCS := \
	kmeans_cuda.cu

SRCS := \
	cluster.c \
	kmeans.c \
	kmeans_clustering.c \
	rmse.c

# ===== Object =====
CU_OBJS := $(CU_SRCS:.cu=.o)	
SR_OBJS := $(SRCS:.c=.o)
OBJS := $(CU_OBJS) $(SR_OBJS)


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
	rm -f $(EXE) $(OBJS) output.txt