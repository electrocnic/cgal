# Created by the script cgal_create_CMakeLists
# This is the CMake script for compiling a set of CGAL applications.

cmake_minimum_required(VERSION 3.1...3.15)
project( Polygon_mesh_processing_Tests )

# CGAL and its components
find_package( CGAL QUIET COMPONENTS  )

if ( NOT CGAL_FOUND )

  message(STATUS "This project requires the CGAL library, and will not be compiled.")
  return()  

endif()

# Boost and its components
find_package( Boost REQUIRED )

if ( NOT Boost_FOUND )

  message(STATUS "This project requires the Boost library, and will not be compiled.")

  return()  

endif()

# include for local directory

# include for local package
find_package(Eigen3 3.2.0) #(requires 3.2.0 or greater)

find_package( TBB )

if (EIGEN3_FOUND)
  include( ${EIGEN3_USE_FILE} )

  # Creating entries for all .cpp/.C files with "main" routine
  # ##########################################################
  create_single_source_cgal_program("fairing_test.cpp")
  create_single_source_cgal_program("triangulate_hole_Polyhedron_3_no_delaunay_test.cpp" )
  create_single_source_cgal_program("triangulate_hole_Polyhedron_3_test.cpp")
  create_single_source_cgal_program("test_shape_smoothing.cpp")

endif(EIGEN3_FOUND)

find_package( OpenMesh QUIET )
if ( OpenMesh_FOUND )
  include( UseOpenMesh )
else()
  message(STATUS "Examples that use OpenMesh will not be compiled.")
endif()

  create_single_source_cgal_program("connected_component_polyhedron.cpp")
  create_single_source_cgal_program("connected_component_surface_mesh.cpp")
  create_single_source_cgal_program("test_detect_features.cpp")
  create_single_source_cgal_program("pmp_compute_normals_test.cpp")
  create_single_source_cgal_program("orient_polygon_mesh_test.cpp" )
  create_single_source_cgal_program("orient_polygon_soup_test.cpp")
  create_single_source_cgal_program("point_inside_polyhedron_boundary_test.cpp")
  create_single_source_cgal_program("point_inside_surface_mesh_test.cpp" )
  create_single_source_cgal_program("polygon_mesh_slicer_test.cpp" )
  create_single_source_cgal_program("self_intersection_polyhedron_test.cpp" )
  create_single_source_cgal_program("self_intersection_surface_mesh_test.cpp" )
  create_single_source_cgal_program("pmp_do_intersect_test.cpp" )
  create_single_source_cgal_program("test_is_polygon_soup_a_polygon_mesh.cpp")
  create_single_source_cgal_program("test_stitching.cpp")
  create_single_source_cgal_program("remeshing_test.cpp" )
  create_single_source_cgal_program("measures_test.cpp")
  create_single_source_cgal_program("triangulate_faces_test.cpp")
  create_single_source_cgal_program("triangulate_faces_hole_filling_dt3_test.cpp")
  create_single_source_cgal_program("triangulate_faces_hole_filling_all_search_test.cpp")
  create_single_source_cgal_program("test_pmp_remove_border_edge.cpp")
  create_single_source_cgal_program("test_pmp_distance.cpp")
  create_single_source_cgal_program("test_corefinement_and_constraints.cpp")
  create_single_source_cgal_program("test_corefinement_bool_op.cpp")
  create_single_source_cgal_program("test_corefine.cpp")
  create_single_source_cgal_program("test_coref_epic_points_identity.cpp")
  create_single_source_cgal_program("test_does_bound_a_volume.cpp")
  create_single_source_cgal_program("test_pmp_clip.cpp")
  create_single_source_cgal_program("test_autorefinement.cpp")
  create_single_source_cgal_program("autorefinement_sm.cpp")
  create_single_source_cgal_program("triangulate_hole_polyline_test.cpp")
  create_single_source_cgal_program("surface_intersection_sm_poly.cpp" )
  create_single_source_cgal_program("test_orient_cc.cpp")
  create_single_source_cgal_program("test_pmp_transform.cpp")
  create_single_source_cgal_program("extrude_test.cpp")
  create_single_source_cgal_program("test_merging_border_vertices.cpp")
  create_single_source_cgal_program("test_repair_polygon_soup.cpp")
  create_single_source_cgal_program("test_shape_predicates.cpp")
  create_single_source_cgal_program("test_pmp_collision_detection.cpp")
  create_single_source_cgal_program("test_pmp_locate.cpp")
  create_single_source_cgal_program("test_pmp_snapping.cpp")
  create_single_source_cgal_program("test_pmp_non_conforming_snapping.cpp")
  create_single_source_cgal_program("remove_degeneracies_test.cpp")
  create_single_source_cgal_program("test_pmp_manifoldness.cpp")
  create_single_source_cgal_program("test_mesh_smoothing.cpp")

  if( TBB_FOUND )
    CGAL_target_use_TBB(test_pmp_distance)
  else()
    message( STATUS "NOTICE: Intel TBB was not found. test_pmp_distance will use sequential code." )
  endif()

if(OpenMesh_FOUND)
  create_single_source_cgal_program("remeshing_test_P_SM_OM.cpp" )
  target_link_libraries( remeshing_test_P_SM_OM PRIVATE ${OPENMESH_LIBRARIES} )
endif()

find_package(Ceres QUIET)
if(TARGET ceres)
  target_compile_definitions( test_mesh_smoothing PRIVATE CGAL_PMP_USE_CERES_SOLVER )
  target_link_libraries( test_mesh_smoothing PRIVATE ceres )
endif(TARGET ceres)

if(BUILD_TESTING)
  set_tests_properties(
      execution___of__triangulate_hole_Polyhedron_3_no_delaunay_test
      execution___of__triangulate_hole_Polyhedron_3_test
    PROPERTIES RESOURCE_LOCK Triangulation_with_hole)
endif()
