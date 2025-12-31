# ===== Target =====
EXE := lavaMD

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
	-no-pie \
	-L$(CUDA_PATH)/lib64 \
	-lcudart -ldl -lrt -pthread \

# ===== Source =====
SRCS := \
	util/num/num.c \
	util/timer/timer.c 
CPP_SRCS := \
	lavaMD.cpp
CU_SRCS := \
	kernel/kernel_gpu_cuda_wrapper.cu \
	util/device/device.cu

# ===== Object =====
SRCS_OBJS := $(SRCS:.c=.o)
CU_SRCS_OBJS := $(CU_SRCS:.cu=.o)
CPP_SRCS_OBJS := $(CPP_SRCS:.cpp=.o)
OBJS := $(SRCS_OBJS) $(CU_SRCS_OBJS) $(CPP_SRCS_OBJS) 

# ===== Rules =====
$(EXE): $(OBJS)
	$(CXX) $^ $(LDFLAGS) -o $@

# ---- CUDA compile ----
%.o: %.cu
	$(CXX) $(CXXFLAGS) $(CUDAFLAGS) $(INCLUDES) -c $< -o $@

%.o: %.c
	$(CC) $(CFLAGS) $(INCLUDES) -c $< -o $@

%.o: %.cpp
	$(CXX) $(CXXFLAGS) $(INCLUDES) -c $< -o $@

.PHONY: clean
clean:
	rm -f $(EXE) $(OBJS)