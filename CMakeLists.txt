# Makefile for ROPTLIB. To be tested on ubuntu 16.04 LTS

cmake_minimum_required(VERSION 3.5)
cmake_policy(VERSION 3.5)

project(ROPTLIB LANGUAGES CXX VERSION 0.6)

set(CMAKE_CXX_STANDARD 11)

set(CMAKE_BUILD_TYPE Release CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel.")

# ${CMAKE_CURRENT_SOURCE_DIR}: /home/robin/Libraries/ROPTLIB
# ${CMAKE_CURRENT_BINARY_DIR}: /home/robin/Libraries/ROPTLIB/build

# ROPTLIB requires lapack, blas, and m libraries
find_library(LAPACK lapack DOC "LAPACK library to link against")
find_library(BLAS blas DOC "BLAS library to link against")
find_library(M m "M library to link against")

# Build type

# Directory for built libraries (local build)
# /home/robin/Libraries/ROPTLIB/build/lib
set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/lib CACHE PATH 
	"The directory in which to place libraries built by this project")

# Directory for built executables (local build)
# /home/robin/Libraries/ROPTLIB/build/bin
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/bin CACHE PATH 
	"The directory in which to place executables built by this project")

# Set standard installation directories
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR})
set(LIBRARY_DESTINATION ${CMAKE_INSTALL_PREFIX}/lib) # /usr/local/lib
set(SHARED_LIBS FALSE CACHE BOOL 
	"Set to TRUE to build shared library")

# Directories of ROPTLIB header files
set(ROPTLIB_INCLUDE_DIRECTORIES 
	${CMAKE_CURRENT_SOURCE_DIR}/BinaryFiles/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/CpxNStQOrth/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/ElasticShape/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/EucPositive/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Euclidean/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Grassmann/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/L2Sphere/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/LowRank/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Oblique/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/OrthGroup/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/PreShapeCurves/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/SPDManifold/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/SPDTensor/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Sphere/
	${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Stiefel/
	${CMAKE_CURRENT_SOURCE_DIR}/Others/
	${CMAKE_CURRENT_SOURCE_DIR}/Others/SparseBLAS/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/ElasticCurvesRO/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/EucFrechetMean/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/EucPosSpCd/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/EucQuadratic/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/GrassRQ/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/KarcherMean/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/LRMatrixCompletion/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/ObliqueSparsePCA/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/ObliqueTestSparsePCA/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/OrthBoundingBox/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/PreShapePathStraighten/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/SPDMean/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/SPDTensorDL/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/ShapePathStraighten/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/SphereConvexHull/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/StieBrockett/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/StieSoftICA/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/StieSparseBrockett/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/StieSumBrockett/
	${CMAKE_CURRENT_SOURCE_DIR}/Problems/WeightedLowrank/
	${CMAKE_CURRENT_SOURCE_DIR}/Solvers/
	${CMAKE_CURRENT_SOURCE_DIR}/cwrapper/
	${CMAKE_CURRENT_SOURCE_DIR}/cwrapper/blas/
	${CMAKE_CURRENT_SOURCE_DIR}/cwrapper/lapack/
	${CMAKE_CURRENT_SOURCE_DIR}/test/
	CACHE INTERNAL ""
)

set(MANIFOLD_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Manifolds)
set(PROBLEM_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Problems)
set(OTHERS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Others)
set(SOLVERS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Solvers)
set(WRAPPERS_DIR ${CMAKE_CURRENT_SOURCE_DIR}/cwrapper)

include_directories(${CMAKE_SOURCE_DIR} ${ROPTLIB_INCLUDE_DIRECTORIES})

# COMPILE THE ROPTLIB LIBRARY
set(ROPTLIB_SRC
	Manifolds/Element.cpp Manifolds/LinearOPE.cpp Manifolds/Manifold.cpp Manifolds/ProductElement.cpp Manifolds/ProductManifold.cpp Manifolds/SharedSpace.cpp Manifolds/SmartSpace.cpp 
	Manifolds/CpxNStQOrth/CSOVariable.cpp Manifolds/CpxNStQOrth/CSOVector.cpp Manifolds/CpxNStQOrth/CpxNStQOrth.cpp 
	Manifolds/ElasticShape/ElasticShape.cpp Manifolds/ElasticShape/ShapeVariable.cpp Manifolds/ElasticShape/ShapeVector.cpp 
	Manifolds/EucPositive/EucPosVariable.cpp Manifolds/EucPositive/EucPosVector.cpp Manifolds/EucPositive/EucPositive.cpp 
	Manifolds/Euclidean/EucVariable.cpp Manifolds/Euclidean/EucVector.cpp Manifolds/Euclidean/Euclidean.cpp 
	Manifolds/Grassmann/GrassVariable.cpp Manifolds/Grassmann/GrassVector.cpp Manifolds/Grassmann/Grassmann.cpp 
	Manifolds/L2Sphere/L2Sphere.cpp Manifolds/L2Sphere/L2SphereVariable.cpp Manifolds/L2Sphere/L2SphereVector.cpp 
	Manifolds/LowRank/LowRank.cpp Manifolds/LowRank/LowRankVariable.cpp Manifolds/LowRank/LowRankVector.cpp 
	Manifolds/Oblique/Oblique.cpp Manifolds/Oblique/ObliqueVariable.cpp Manifolds/Oblique/ObliqueVector.cpp 
	Manifolds/OrthGroup/OrthGroup.cpp Manifolds/OrthGroup/OrthGroupVariable.cpp Manifolds/OrthGroup/OrthGroupVector.cpp 
	Manifolds/PreShapeCurves/PSCVariable.cpp Manifolds/PreShapeCurves/PSCVector.cpp Manifolds/PreShapeCurves/PreShapeCurves.cpp 
	Manifolds/SPDManifold/SPDManifold.cpp Manifolds/SPDManifold/SPDVariable.cpp Manifolds/SPDManifold/SPDVector.cpp 
	Manifolds/SPDTensor/SPDTVariable.cpp Manifolds/SPDTensor/SPDTVector.cpp Manifolds/SPDTensor/SPDTensor.cpp 
	Manifolds/Sphere/Sphere.cpp Manifolds/Sphere/SphereVariable.cpp Manifolds/Sphere/SphereVector.cpp 
	Manifolds/Stiefel/StieVariable.cpp Manifolds/Stiefel/StieVector.cpp Manifolds/Stiefel/Stiefel.cpp 
	Others/ForDebug.cpp Others/MinPNormConHull.cpp Others/MyMatrix.cpp Others/Spline.cpp Others/Timer.cpp Others/randgen.cpp 
	Others/SparseBLAS/nist_spblas.cpp 
	Problems/Problem.cpp
	Problems/ElasticCurvesRO/DriverElasticCurvesRO.cpp Problems/ElasticCurvesRO/ElasticCurvesRO.cpp 
	Problems/EucFrechetMean/EucFrechetMean.cpp 
	Problems/EucPosSpCd/EucPosSpCd.cpp 
	Problems/EucQuadratic/EucQuadratic.cpp 
	Problems/GrassRQ/GrassRQ.cpp 
	Problems/KarcherMean/KarcherMean.cpp 
	Problems/LRMatrixCompletion/LRMatrixCompletion.cpp 
	Problems/ObliqueSparsePCA/ObliqueSparsePCA.cpp 
	Problems/ObliqueTestSparsePCA/ObliqueTestSparsePCA.cpp 
	Problems/OrthBoundingBox/OrthBoundingBox.cpp 
	Problems/PreShapePathStraighten/PreShapePathStraighten.cpp 
	Problems/SPDMean/SPDMean.cpp 
	Problems/SPDTensorDL/SPDTensorDL.cpp 
	Problems/ShapePathStraighten/ShapePathStraighten.cpp 
	Problems/SphereConvexHull/SphereConvexHull.cpp 
	Problems/StieBrockett/StieBrockett.cpp 
	Problems/StieSoftICA/StieSoftICA.cpp 
	Problems/StieSparseBrockett/StieSparseBrockett.cpp 
	Problems/StieSumBrockett/StieSumBrockett.cpp 
	Problems/WeightedLowrank/WeightedLowRank.cpp 
	Solvers/LRBFGS.cpp Solvers/LRBFGSLPSub.cpp Solvers/LRTRSR1.cpp Solvers/MRankAdaptive.cpp Solvers/QuasiNewton.cpp Solvers/RBFGS.cpp Solvers/RBFGSLPSub.cpp Solvers/RBroydenFamily.cpp Solvers/RCG.cpp Solvers/RGS.cpp Solvers/RNewton.cpp Solvers/RSD.cpp Solvers/RTRNewton.cpp Solvers/RTRSD.cpp Solvers/RTRSR1.cpp Solvers/RWRBFGS.cpp Solvers/Solvers.cpp Solvers/SolversLS.cpp Solvers/SolversLSLPSub.cpp Solvers/SolversTR.cpp
)

if (SHARED_LIBS)
	add_library(roptlib SHARED ${ROPTLIB_SRC})
	install(TARGETS roptlib LIBRARY DESTINATION lib)
else (SHARED_LIBS)
	add_library(roptlib ${ROPTLIB_SRC})
	add_definitions(-fPIC)
	install(TARGETS roptlib ARCHIVE DESTINATION lib)
endif (SHARED_LIBS)

target_link_libraries(roptlib ${LAPACK} ${BLAS} ${M})

install(DIRECTORY ${MANIFOLD_DIR} ${PROBLEM_DIR} ${OTHERS_DIR} ${SOLVERS_DIR} ${WRAPPERS_DIR}
		DESTINATION include/roptlib
		FILES_MATCHING PATTERN "*.h")

# Compile the test programs from ROPTLIB
add_executable(TestSimpleExample test/TestSimpleExample.cpp)
target_link_libraries(TestSimpleExample roptlib)

add_executable(TestProductExample test/TestProductExample.cpp)
target_link_libraries(TestProductExample roptlib)

# 1- local build #

# Register the local build in case one doesn't use "make install"
export(PACKAGE roptlib)

# Create variable with the library location
set(libroptlib_library $<TARGET_FILE:roptlib>)

# Create variable for the local build tree
get_property(libroptlib_include_dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)

# Configure & generate config file for local build tree
configure_file(libroptlibConfig.cmake.in
	"${PROJECT_BINARY_DIR}/libroptlibConfig.cmake.conf" @ONLY)
file(GENERATE
	OUTPUT "${PROJECT_BINARY_DIR}/libroptlibConfig.cmake"
	INPUT "${PROJECT_BINARY_DIR}/libroptlibConfig.cmake.conf")

# 2- installation build #

# Change the library location for an install location
set(libroptlib_library ${CMAKE_INSTALL_PREFIX}/lib/$<TARGET_FILE_NAME:roptlib>)

# Change the include location for the case of an install location
set(libroptlib_include_dirs ${CMAKE_INSTALL_PREFIX}/include/roptlib ${CMAKE_INSTALL_PREFIX}/include/roptlib/cwrapper/blas ${CMAKE_INSTALL_PREFIX}/include/roptlib/cwrapper/lapack)

# We put the generated file for installation in a different repository (i.e., ./CMakeFiles/)
configure_file(libroptlibConfig.cmake.in
	"${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/libroptlibConfig.cmake.conf" @ONLY)
file(GENERATE
	OUTPUT "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/libroptlibConfig.cmake"
	INPUT "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/libroptlibConfig.cmake.conf")

install(FILES
	"${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/libroptlibConfig.cmake"
	DESTINATION share/libroptlib/cmake COMPONENT dev)

#====================== uninstall target ===============================
configure_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake_uninstall.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
    IMMEDIATE @ONLY)

add_custom_target(uninstall
    COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)