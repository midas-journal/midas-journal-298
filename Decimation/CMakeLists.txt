CMAKE_MINIMUM_REQUIRED(VERSION 2.4)

PROJECT( Decimation )

# On Visual Studio 8 MS deprecated C. This removes all 1.276E1265 security
# warnings
IF(NOT BORLAND)
 IF(NOT CYGWIN)
   IF(NOT MINGW)
     IF(NOT ITK_ENABLE_VISUAL_STUDIO_DEPRECATED_C_WARNINGS)
       ADD_DEFINITIONS(
         -D_CRT_NONSTDC_NO_DEPRECATE
         -D_CRT_SECURE_NO_DEPRECATE
         -D_SCL_SECURE_NO_DEPRECATE
         )
     ENDIF(NOT ITK_ENABLE_VISUAL_STUDIO_DEPRECATED_C_WARNINGS)
   ENDIF(NOT MINGW)
 ENDIF(NOT CYGWIN)
ENDIF(NOT BORLAND)

FIND_PACKAGE(ITK)

IF(ITK_FOUND)
    INCLUDE(${ITK_USE_FILE})
ENDIF(ITK_FOUND)

INCLUDE_DIRECTORIES( BEFORE ${ITK_INCLUDE_DIR} )

INCLUDE( CTest )

FIND_PATH( DATAROOT wave.vtk
  ${Decimation_SOURCE_DIR}/data
  $ENV{DATAROOT} )

ADD_EXECUTABLE( SquaredEdgeLengthDecimation
  itkQuadEdgeMeshSquaredEdgeLengthDecimationTest.cxx )
TARGET_LINK_LIBRARIES( SquaredEdgeLengthDecimation 
  ITKCommon ITKQuadEdgeMesh )
ADD_TEST( SquaredEdgeLengthDecimationTest SquaredEdgeLengthDecimation 
${DATAROOT}/wave.vtk 100 SqauredEdgeRes.vtk )

ADD_EXECUTABLE( QuadricDecimation
  itkQuadEdgeMeshQuadricDecimationTest.cxx )
TARGET_LINK_LIBRARIES( QuadricDecimation 
  ITKCommon ITKQuadEdgeMesh )
ADD_TEST( QuadricDecimationTest QuadricDecimation
${DATAROOT}/wave.vtk 100 SqauredEdgeRes.vtk )

