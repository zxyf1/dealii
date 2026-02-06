##
#  CMake script for the step-64_large tutorial program:
#
#  A variant of step-64 with larger mesh (subdivided_hyper_cube) and
#  CUDA event timing for stress-testing GPU memory usage (~35-40 GB).
##

# Set the name of the project and target:
set(TARGET "step-64_large")

# Declare all source files the target consists of.
set(TARGET_SRC
  ${TARGET}.cc
  )

# Usually, you will not need to modify anything beyond this point...

cmake_minimum_required(VERSION 3.13.4)

find_package(deal.II 9.8.0
  HINTS ${deal.II_DIR} ${DEAL_II_DIR} ../ ../../ $ENV{DEAL_II_DIR}
  )
if(NOT ${deal.II_FOUND})
  message(FATAL_ERROR "\n"
    "*** Could not locate a (sufficiently recent) version of deal.II. ***\n\n"
    "You may want to either pass a flag -DDEAL_II_DIR=/path/to/deal.II to cmake\n"
    "or set an environment variable \"DEAL_II_DIR\" that contains this path."
    )
endif()

#
# Are all dependencies fulfilled?
#
if(NOT DEAL_II_WITH_MPI OR NOT DEAL_II_WITH_P4EST OR NOT DEAL_II_WITH_LAPACK)
  message(FATAL_ERROR "
Error! This tutorial requires a deal.II library that was configured with the following options:
    DEAL_II_WITH_LAPACK = ON
    DEAL_II_WITH_MPI = ON
    DEAL_II_WITH_P4EST = ON
However, the deal.II library found at ${DEAL_II_PATH} was configured with these options:
    DEAL_II_WITH_LAPACK = ${DEAL_II_WITH_LAPACK}
    DEAL_II_WITH_MPI = ${DEAL_II_WITH_MPI}
    DEAL_II_WITH_P4EST = ${DEAL_II_WITH_P4EST}
This conflicts with the requirements."
    )
endif()


deal_ii_initialize_cached_variables()
project(${TARGET})
deal_ii_invoke_autopilot()
