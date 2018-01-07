cmake_minimum_required(VERSION 2.8.8)

project(catalyst_adaptor)

set(USE_CATALYST ON CACHE BOOL "Link with Catalyst")
option(BUILD_SHARED_LIBS "Build shared library to simplify linking" ON)

if(USE_CATALYST)
  find_package(ParaView 4.1 REQUIRED COMPONENTS vtkPVCatalyst)
  include("${PARAVIEW_USE_FILE}")
  set(Adaptor_SRCS adaptor.cxx coprocessor.cxx vtk_pipeline.cxx)
  add_library(catalyst_adaptor ${Adaptor_SRCS})
  target_link_libraries(catalyst_adaptor vtkPVCatalyst)
  add_definitions("-DUSE_CATALYST")
  if(NOT PARAVIEW_USE_MPI)
    message(SEND_ERROR "ParaView must be built with MPI enabled")
  endif()
else()
  find_package(MPI REQUIRED)
  include_directories(${MPI_CXX_INCLUDE_PATH})
endif()