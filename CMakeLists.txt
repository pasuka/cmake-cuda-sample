cmake_minimum_required(VERSION 3.14 FATAL_ERROR)
project(cuda_sample LANGUAGES CUDA CXX)

## Print CUDA related variable information ##
message(STATUS "CUDA_TOOLKIT_ROOT_DIR: ${CUDA_TOOLKIT_ROOT_DIR}")
message(STATUS "CUDA_SDK_ROOT_DIR: ${CUDA_SDK_ROOT_DIR}")
message(STATUS "CUDA LIBRARY: ${CUDA_LIBRARIES}  ")
message(STATUS "CUBLAS: ${CUDA_CUBLAS_LIBRARIES}")
message(STATUS "CUDART: ${CUDA_cudart_static_LIBRARY}")

###########################################
## User reqiured to update this variable ##
###########################################
set(SAMPLE_ROOT_DIR "C:/ProgramData/NVIDIA Corporation/CUDA Samples/v10.1")
set(TOOLKIT_ROOT_DIR "C:/Program Files/NVIDIA GPU Computing Toolkit/CUDA/v10.1")

## Set for CXX standard ##
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
## Set for CUDA standar ##
set(CMAKE_CUDA_STANDARD 14)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)
set(CMAKE_CUDA_SEPARABLE_COMPILATION ON)

##################################################
## Special treatment for current graphical card ## 
##################################################
set(CUDA_ARCH "-arch=sm_30")
set(CMAKE_CUDA_FLAGS "${CMAKE_CUDA_FLAGS} ${CUDA_ARCH}")

## Set required libraries for compile examples ##
set(requiredLibs
  cublas.lib
  cudart_static.lib
  nvrtc.lib
  cuda.lib
)

## Begin compile samples ##
set(single_src OFF)
set(multi_src ON)
set(folder0 "${SAMPLE_ROOT_DIR}/0_Simple")
set(folder7 "${SAMPLE_ROOT_DIR}/7_CUDALibraries")

set(cppMixed 
  ${folder0}/cppIntegration/cppIntegration.cu
  ${folder0}/cppIntegration/cppIntegration_gold.cpp
  ${folder0}/cppIntegration/main.cpp
)
set(matmulNVRTC 
  ${folder0}/matrixMul_nvrtc/matrixMul_kernel.cu
  ${folder0}/matrixMul_nvrtc/matrixMul.cpp
)
set(vectorAdd_nvrtc
  ${folder0}/vectorAdd_nvrtc/vectorAdd_kernel.cu
  ${folder0}/vectorAdd_nvrtc/vectorAdd.cpp
)
set(vectorAddDrv
  ${folder0}/vectorAddDrv/vectorAdd_kernel.cu
  ${folder0}/vectorAddDrv/vectorAddDrv.cpp
)

set(sample_tuple
  "test01\;${folder0}/matrixMul/matrixMul.cu\;${single_src}"
  "test02\;${folder0}/cudaOpenMP/cudaOpenMP.cu\;${single_src}"
  "test03\;cppMixed\;${multi_src}"
  "test04\;${folder0}/matrixMulCUBLAS/matrixMulCUBLAS.cpp\;${single_src}"
  "test05\;${folder0}/simplePrintf/simplePrintf.cu\;${single_src}"
  "test06\;${folder0}/UnifiedMemoryStreams/UnifiedMemoryStreams.cu\;${single_src}"
  "test07\;${folder0}/vectorAdd/vectorAdd.cu\;${single_src}"
  # "test08\;vectorAdd_nvrtc\;${multi_src}"
  # "test09\;matmulNVRTC\;${multi_src}"
  "test10\;vectorAddDrv\;${multi_src}"
  "test71\;${folder7}/simpleCUBLAS/simpleCUBLAS.cpp\;${single_src}"
)

foreach(iter IN LISTS sample_tuple)
  list(GET iter 0 tname)
  list(GET iter 1 src)
  list(GET iter 2 multiple)
  if(${multiple})
    add_executable(${tname} ${${src}})
  else()
    add_executable(${tname} ${src})
  endif()
  target_include_directories(${tname} PRIVATE ${SAMPLE_ROOT_DIR}/common/inc)
  foreach(lib IN LISTS requiredLibs)
    target_link_libraries(${tname} PRIVATE ${TOOLKIT_ROOT_DIR}/lib/x64/${lib})
  endforeach()
endforeach()
  

