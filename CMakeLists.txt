cmake_minimum_required(VERSION 2.6)

find_package(Rock)
rock_init(arvand_herd 0.1)

add_definitions(-Wno-deprecated)
add_definitions(-Wextra -Wno-sign-compare)
add_definitions(-DUSE_LP=1)
add_definitions(-DCOIN_USE_CLP=1)
add_definitions(-DSTATE_VAR_BYTES=1)
rock_activate_cxx11()

rock_library(${PROJECT_NAME}-search
    SOURCES
        search/aras_state.cc
        search/axioms.cc
        search/blind_search_heuristic.cc
        search/closed_list.cc
        search/command_line_parsing.cc
        search/delayed_wa_star.cc
        search/domain_transition_graph.cc
        search/fd_ff_heuristic.cc
        search/ff_heuristic.cc
        search/globals.cc
        search/goal_count_heuristic.cc
        search/heuristic.cc
        search/lama_ffc_heuristic.cc
        search/lama_ffs_heuristic.cc
        search/landmarks_count_heuristic.cc
        search/landmarks_graph.cc
        search/landmarks_graph_rpg_sasp.cc
        search/memory_utils.cc
        search/mrw.cc
        search/mrw_parameters.cc
        search/mrw_runner.cc
        search/mtrand.cc
        search/open_list.cc
        search/operator.cc
        search/parameter_learner.cc
        search/plan_booster.cc
        search/planner.cc
        #search/planner_wrapper.cc
        search/predecessor_generator.cc
        search/search_engine.cc
        search/search_space.cc
        search/shared_mrw_parameters.cc
        search/smart_restarts.cc
        search/state.cc
        search/string_utils.cc
        search/successor_generator.cc
        search/timer.cc
        search/walker.cc
        search/wa_runner.cc
        search/wa_star_params.cc
    HEADERS
        search/aras_state.h
        search/axioms.h
        search/blind_search_heuristic.h
        search/closed_list.h
        search/command_line_parsing.h
        search/delayed_wa_star.h
        search/domain_transition_graph.h
        search/fd_ff_heuristic.h
        search/ff_heuristic.h
        search/globals.h
        search/goal_count_heuristic.h
        search/heuristic.h
        search/lama_ffc_heuristic.h
        search/lama_ffs_heuristic.h
        search/landmarks_count_heuristic.h
        search/landmarks_graph.h
        search/landmarks_graph_rpg_sasp.h
        search/landmarks_types.h
        search/memory_utils.h
        search/mrw.h
        search/mrw_parameters.h
        search/mrw_runner.h
        search/mtrand.h
        search/open_list.h
        search/operator.h
        search/parameter_learner.h
        search/plan_booster.h
        search/planner_wrapper.h
        search/predecessor_generator.h
        search/search_engine.h
        search/search_space.h
        search/shared_mrw_parameters.h
        search/smart_restarts.h
        search/state.h
        search/string_utils.h
        search/successor_generator.h
        search/timer.h
        search/walker.h
        search/wa_runner.h
        search/wa_star_params.h
)
target_link_libraries(${PROJECT_NAME}-search pthread)

rock_executable(${PROJECT_NAME}-planner-search
        search/planner.cc
        DEPS ${PROJECT_NAME}-search
        NOINSTALL)

rock_library(${PROJECT_NAME}-preprocess
    SOURCES
        preprocess/axiom.cc
        preprocess/causal_graph.cc
        preprocess/domain_transition_graph.cc
        preprocess/helper_functions.cc
        preprocess/max_dag.cc
        preprocess/operator.cc
        preprocess/planner.cc
        preprocess/scc.cc
        preprocess/state.cc
        preprocess/successor_generator.cc
        preprocess/variable.cc
    HEADERS
        preprocess/axiom.h
        preprocess/causal_graph.h
        preprocess/domain_transition_graph.h
        preprocess/helper_functions.h
        preprocess/max_dag.h
        preprocess/operator.h
        preprocess/scc.h
        preprocess/state.h
        preprocess/successor_generator.h
        preprocess/variable.h
)
rock_executable(${PROJECT_NAME}-planner-preprocess
        preprocess/planner.cc
        DEPS ${PROJECT_NAME}-preprocess
        NOINSTALL)

# Installation of the driver directory
# Python wrapper to facilitate running the system
install(CODE "execute_process(COMMAND ${CMAKE_COMMAND} -E copy_directory
\"${CMAKE_CURRENT_SOURCE_DIR}/translate\"
\"${CMAKE_INSTALL_PREFIX}/bin/${PROJECT_NAME}/translate\") ")

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-planner-preprocess
    PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
    DESTINATION bin/${PROJECT_NAME}/preprocess
    RENAME preprocess)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}-planner-search
    PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
    DESTINATION bin/${PROJECT_NAME}/search
    RENAME release-search)
install(FILES plan
    PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
    DESTINATION bin
    RENAME arvand_herd-planner)
install(FILES sat_conf
    PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
    DESTINATION bin/${PROJECT_NAME}
    RENAME arvand_herd-sat_conf)

