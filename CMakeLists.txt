# PROJECT CONFIGURATION
cmake_minimum_required(VERSION 2.8)

project(ROPTLIB C CXX)
set (CMAKE_CXX_FLAGS "-std=c++11 ${CMAKE_CXX_FLAGS}")
add_compile_options(-march=native) # Enable faster instruction sets

include(GNUInstallDirs)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
set(ROPTLIB_INSTALL_INCLUDEDIR ${CMAKE_INSTALL_INCLUDEDIR}/ROPTLIB)

# Directory for built libraries
set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/lib CACHE PATH "The directory in which to place libraries built by this project")
# Directory for built executables
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/bin CACHE PATH "The directory in which to place executables built by this project")

# Directory for built libraries
set(LIBRARY_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/lib CACHE PATH "The directory in which to place libraries built by this project")
# Directory for built executables
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_CURRENT_BINARY_DIR}/bin CACHE PATH "The directory in which to place executables built by this project")

# BUILD CONFIGURATIONS
option(CMAKE_VERBOSE_MAKEFILE "Generate verbose makefiles?" OFF)

set(CODE_PROFILING OFF CACHE BOOL "Turn on code profiling?")
if(${CODE_PROFILING})
message(STATUS "Turning on code profiling for ROPTLIB")
endif()


# ROPTLIB requires lapack, blas, and m libraries
find_library(LAPACK lapack DOC "LAPACK library to link against")
find_library(BLAS blas DOC "BLAS library to link against")
find_library(M m "M library to link against")

# COMPILE THE ROPTLIB LIBRARY
add_library(roptlib SHARED
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
#Problems/juliaProblem.cpp 
#Problems/mexProblem.cpp 
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
Solvers/LRBFGS.cpp Solvers/LRBFGSLPSub.cpp Solvers/LRTRSR1.cpp Solvers/MRankAdaptive.cpp Solvers/QuasiNewton.cpp Solvers/RBFGS.cpp Solvers/RBFGSLPSub.cpp Solvers/RBroydenFamily.cpp Solvers/RCG.cpp Solvers/RGS.cpp Solvers/RNewton.cpp Solvers/RSD.cpp Solvers/RTRNewton.cpp Solvers/RTRSD.cpp Solvers/RTRSR1.cpp Solvers/RWRBFGS.cpp Solvers/Solvers.cpp Solvers/SolversLS.cpp Solvers/SolversLSLPSub.cpp Solvers/SolversTR.cpp)

target_include_directories(roptlib PUBLIC 
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/CpxNStQOrth>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/EucPositive>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Euclidean>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Grassmann>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/L2Sphere>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/LowRank>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Oblique>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/OrthGroup>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/SPDManifold>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Sphere>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Manifolds/Stiefel>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Others>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Others/SparseBLAS>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Problems>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Solvers>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/cwrapper>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/cwrapper/blas>
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/cwrapper/lapack>
	# Install
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/CpxNStQOrth>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/EucPositive>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/Euclidean>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/Grassmann>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/L2Sphere>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/LowRank>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/Oblique>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/OrthGroup>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/SPDManifold>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/Sphere>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Manifolds/Stiefel>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Others>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Others/SparseBLAS>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Problems>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/Solvers>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/cwrapper>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/cwrapper/blas>
	$<INSTALL_INTERFACE:${ROPTLIB_INSTALL_INCLUDEDIR}/cwrapper/lapack>
)

target_link_libraries(roptlib ${LAPACK} ${BLAS} ${M})
set_target_properties(roptlib PROPERTIES INSTALL_RPATH_USE_LINK_PATH TRUE)

if(${CODE_PROFILING})
	set_target_properties(roptlib PROPERTIES COMPILE_FLAGS "-pg -g" LINK_FLAGS "-pg -g")
endif()

# COMPILE EXECUTABLES

# Compile the simple test program from ROPTLIB
# add_executable(TestSimpleExample test/TestSimpleExample.cpp)
# target_link_libraries(TestSimpleExample roptlib)

# Compile the simple product test program from ROPTLIB
# add_executable(TestProduct test/TestProduct.h test/TestProduct.cpp)
# target_link_libraries(TestProduct roptlib)

# Compile the simple product test program from ROPTLIB
# add_executable(TestProductBrockett test/TestProductExample.cpp test/TestProductExample.cpp)
# target_link_libraries(TestProductBrockett roptlib)

################################ INSTALL ##########################################
include(CMakePackageConfigHelpers)
set(INSTALL_CONFIGDIR ${CMAKE_INSTALL_LIBDIR}/cmake/roptlib)

write_basic_package_version_file(${CMAKE_CURRENT_BINARY_DIR}/roptlibConfigVersion.cmake
  VERSION 0.7
  COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(${CMAKE_CURRENT_LIST_DIR}/cmake/roptlibConfig.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/roptlibConfig.cmake
  INSTALL_DESTINATION ${INSTALL_CONFIGDIR}
)

# install the configuration file
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/roptlibConfig.cmake
  DESTINATION ${INSTALL_CONFIGDIR}
)


install(
		TARGETS roptlib
		EXPORT roptlibTargets
		LIBRARY  DESTINATION ${CMAKE_INSTALL_LIBDIR}
)

install(
		EXPORT roptlibTargets
		FILE roptlibTargets.cmake
		DESTINATION ${INSTALL_CONFIGDIR}
)

# install header files
install(DIRECTORY ${CMAKE_SOURCE_DIR}/
		DESTINATION ${ROPTLIB_INSTALL_INCLUDEDIR}
		FILES_MATCHING PATTERN "*.h"
		)


export(TARGETS roptlib FILE roptlibTargets.cmake)

############################### UNINSTALL ##########################################
if(NOT TARGET uninstall)
	configure_file(
			"${CMAKE_CURRENT_SOURCE_DIR}/cmake/cmake_uninstall.cmake.in"
			"${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake"
			IMMEDIATE @ONLY)

	add_custom_target(uninstall
			COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
endif()
