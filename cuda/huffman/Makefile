# ===== Target =====
EXE := pavle

# ===== Compiler =====
CXX := clang++
CC  := clang

# ===== CUDA =====
CUDA_PATH ?= /usr/local/cuda-12
CUDA_ARCH := sm_89

# ===== Flags =====
CXXFLAGS := -O2 
CFLAGS   := -O2 
CUDAFLAGS := \
	--cuda-gpu-arch=$(CUDA_ARCH) \
	--cuda-path=$(CUDA_PATH) \

INCLUDES := -I$(CUDA_PATH)/include
LDFLAGS := \
	-L$(CUDA_PATH)/lib64 \
	-lcudart -ldl -lrt -pthread \

# ===== Source =====
CU_SRCS := \
	pavle.cu \

SRCS := \
	cpuencode.cpp

# ===== Object =====
CU_OBJS := $(CU_SRCS:.cu=.o)
SR_OBJS := $(SRCS:.cpp=.o)
OBJS := $(CU_OBJS) $(SR_OBJS)

# ===== Rules =====
$(EXE): $(OBJS)
	$(CXX) $^ $(LDFLAGS) -o $@

# ---- CUDA compile ----
%.o: %.cu
	$(CXX) $(CXXFLAGS) $(CUDAFLAGS) $(INCLUDES) -c $< -o $@

.PHONY: clean
clean:
	rm -f $(EXE) $(OBJS)