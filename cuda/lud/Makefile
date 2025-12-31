# ===== Target =====
EXE := lud

# ===== Compiler =====
CXX := clang++
CC  := clang

# ===== CUDA =====
CUDA_PATH ?= /usr/local/cuda-12
CUDA_ARCH := sm_89

# ===== Flags =====
CXXFLAGS := -O2 -fno-PIE
CFLAGS   := -O2 -fno-PIE
CUDAFLAGS := \
	--cuda-gpu-arch=$(CUDA_ARCH) \
	--cuda-path=$(CUDA_PATH) \

INCLUDES := -I$(CUDA_PATH)/include
LDFLAGS := \
	-L$(CUDA_PATH)/lib64 \
	-no-pie \
	-lcudart -ldl -lrt -pthread \

# ===== Source =====
SRCS := \
	common/common.c 

CU_SRCS := \
	lud_kernel.cu \
	lud.cu

# ===== Object =====
OBJS := $(SRCS:.c=.o) $(CU_SRCS:.cu=.o)

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