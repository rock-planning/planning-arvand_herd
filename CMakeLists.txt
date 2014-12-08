cmake_minimum_required(VERSION 2.6)

find_package(Rock)
rock_init(arvand_herd 0.1)

execute_process(COMMAND ${CMAKE_COMMAND} -E chdir ../ git init)
execute_process(COMMAND ${CMAKE_COMMAND} -E chdir ../ chmod u+x plan_all)
execute_process(COMMAND ${CMAKE_COMMAND} -E chdir ../ chmod u+x build_all)
execute_process(COMMAND ${CMAKE_COMMAND} -E chdir ../ ./build_all)
execute_process(COMMAND ${CMAKE_COMMAND} -E chdir ../ ln -f -s ${CMAKE_CURRENT_SOURCE_DIR}/plan arvand-herd-planner-script)
execute_process(COMMAND ${CMAKE_COMMAND} -E chdir ../ ln -f -s ${CMAKE_CURRENT_SOURCE_DIR}/plan_all arvand-herd-planner)

install(FILES arvand-herd-planner arvand-herd-planner-script
    PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
    DESTINATION bin
    )

