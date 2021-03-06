cmake_minimum_required(VERSION 2.6)
set(CMAKE_C_COMPILER "mpicc")
set(CMAKE_CXX_COMPILER "mpicxx")


PROJECT (Recon)

#build the XML parser
ADD_SUBDIRECTORY(ticpp)
#link_directories("ticpp")

#includes
include_directories("$ENV{FFTW_DIR}/include")
include_directories("$ENV{PETSC_DIR}/include")
include_directories("$ENV{GSL_DIR}/include")
#Link directories
link_directories("$ENV{FFTW_DIR}/lib")
set(FFTW_LIBS drfftw_mpi dfftw_mpi drfftw dfftw m)
link_directories("$ENV{PETSC_DIR}/lib")
set(GSL_LIBS gsl gslcblas)  

link_directories("$ENV{GSL_DIR}/lib")

ADD_LIBRARY(Recon SHARED Recon.h Misc.cpp Params.cpp Particle.cpp DensityGrid.cpp PotentialSolve.cpp Delta.cpp)  
TARGET_LINK_LIBRARIES(Recon ${FFTW_LIBS})
TARGET_LINK_LIBRARIES(Recon petsc)
TARGET_LINK_LIBRARIES(Recon ${GSL_LIBS}) 
TARGET_LINK_LIBRARIES(Recon ticpp) 


ADD_SUBDIRECTORY(tests)
ADD_SUBDIRECTORY(progs)
