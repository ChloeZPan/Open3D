set(KERNEL_SRC
    kernel/IndexGetSet.cpp
    kernel/IndexGetSetCPU.cpp
    kernel/NonZero.cpp
    kernel/NonZeroCPU.cpp
    kernel/UnaryEW.cpp
    kernel/UnaryEWCPU.cpp
    kernel/BinaryEW.cpp
    kernel/BinaryEWCPU.cpp
    kernel/Reduction.cpp
    kernel/ReductionCPU.cpp
    kernel/Kernel.cpp
)

set(KERNEL_CUDA_SRC
    kernel/IndexGetSetCUDA.cu
    kernel/NonZeroCUDA.cu
    kernel/UnaryEWCUDA.cu
    kernel/BinaryEWCUDA.cu
    kernel/ReductionCUDA.cu
)

set(LINALG_SRC
    linalg/Matmul.cpp
    linalg/MatmulCPU.cpp
    linalg/LeastSquares.cpp
    linalg/LeastSquaresCPU.cpp
    linalg/Solve.cpp
    linalg/SolveCPU.cpp
    linalg/Inverse.cpp
    linalg/InverseCPU.cpp
    linalg/SVD.cpp
    linalg/SVDCPU.cpp
)

set(LINALG_CUDA_SRC
    linalg/LinalgUtils.cpp
    linalg/MatmulCUDA.cpp
    linalg/LeastSquaresCUDA.cpp
    linalg/SolveCUDA.cpp
    linalg/InverseCUDA.cpp
    linalg/SVDCUDA.cpp
)

set(CORE_SRC
    AdvancedIndexing.cpp
    ShapeUtil.cpp
    CUDAUtils.cpp
    Dtype.cpp
    EigenConverter.cpp
    Indexer.cpp
    MemoryManager.cpp
    MemoryManagerCPU.cpp
    Tensor.cpp
    TensorKey.cpp
    TensorList.cpp
)

set(CORE_CUDA_SRC
    MemoryManagerCUDACached.cu
    MemoryManagerCUDASimple.cu
)

set(HASHMAP_SRC
  hashmap/Hashmap.cpp
  hashmap/DeviceHashmap.cpp
  hashmap/CPU/DefaultHashmapCPU.cpp
)

set(HASHMAP_CUDA_SRC
  hashmap/CUDA/DefaultHashmapCUDA.cu
  )

set(CORE_NNS_SRC
    nns/NanoFlannIndex.cpp
    nns/NearestNeighborSearch.cpp
)

if(BUILD_CUDA_MODULE)
    set(ALL_CORE_SRC
        ${CORE_SRC}
        ${CORE_CUDA_SRC}
        ${LINALG_SRC}
        ${LINALG_CUDA_SRC}
        ${KERNEL_SRC}
        ${KERNEL_CUDA_SRC}
        ${HASHMAP_SRC}
        ${HASHMAP_CUDA_SRC}
        ${CORE_NNS_SRC}
    )
else()
  set(ALL_CORE_SRC
        ${CORE_SRC}
        ${LINALG_SRC}
        ${KERNEL_SRC}
        ${HASHMAP_SRC}
        ${CORE_NNS_SRC}
    )
endif()

# Create object library
add_library(core OBJECT ${ALL_CORE_SRC})
open3d_set_global_properties(core)
open3d_link_3rdparty_libraries(core)
open3d_set_open3d_lib_properties(core)
open3d_show_and_abort_on_warning(core)

if(BUILD_CUDA_MODULE)
    target_include_directories(core SYSTEM PRIVATE ${CMAKE_CUDA_TOOLKIT_INCLUDE_DIRECTORIES})
endif()
