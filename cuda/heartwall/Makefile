# ===== Target =====
EXE := heartwall

# ===== Submodule =====
AVI_DIR := ../../common/avi

# ===== Compiler =====
CXX := clang++
CC  := clang

# ===== CUDA =====
CUDA_PATH ?= /usr/local/cuda-12
CUDA_ARCH := sm_89

# ===== Flags =====
CXXFLAGS := -O2 -I$(AVI_DIR)
CFLAGS   := -O2 -I$(AVI_DIR)
CUDAFLAGS := \
	--cuda-gpu-arch=$(CUDA_ARCH) \
	--cuda-path=$(CUDA_PATH) \

INCLUDES := -I$(CUDA_PATH)/include -I$(CUDA_PATH)/samples/common/inc
LDFLAGS := \
	-L$(CUDA_PATH)/lib64 \
	-lcudart -ldl -lrt -pthread \

# ===== Source =====
SRCS := heartwall.cu

# ===== Object =====
OBJS := \
	$(AVI_DIR)/avilib.o \
	$(AVI_DIR)/avimod.o \
	$(SRCS:.cu=.o)


# ---- build AVI objects ----
.PHONY: all
all: $(EXE)

$(AVI_DIR)/avilib.o $(AVI_DIR)/avimod.o:
	$(MAKE) -C $(AVI_DIR)

$(EXE): $(OBJS)
	$(CXX) $^ $(LDFLAGS) -o $@

# ---- CUDA compile ----
%.o: %.cu
	$(CXX) $(CXXFLAGS) $(CUDAFLAGS) $(INCLUDES) -c $< -o $@

.PHONY: clean
clean:
	rm -f $(EXE) $(OBJS)


# include ../common.mk

# EXE  = heartwall
# OBJS = $(AVI_DIR)/avilib.o $(AVI_DIR)/avimod.o

# .PHONY: all
# all: $(EXE)

# # we call make recursively, so need to export variables
# export

# AVI_DIR=../../common/avi
# $(AVI_DIR)/avilib.o $(AVI_DIR)/avimod.o:
# 	$(MAKE) -C $(AVI_DIR)
# CPPFLAGS += -I$(AVI_DIR)

# $(EXE): $(OBJS)

# .PHONY: clean
# clean:
# 	$(RM) $(EXE) $(OBJS) output.txt
# 	$(MAKE) -C $(AVI_DIR) clean
