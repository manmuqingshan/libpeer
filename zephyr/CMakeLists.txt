if(CONFIG_LIBPEER)
  zephyr_library_named(libpeer)

  # Align with upstream libpeer build style: compile core C files.
  file(GLOB LIBPEER_CORE_SRCS ${CMAKE_CURRENT_LIST_DIR}/../src/*.c)

  include(${CMAKE_CURRENT_LIST_DIR}/../third_party/coreHTTP/httpFilePaths.cmake)
  include(${CMAKE_CURRENT_LIST_DIR}/../third_party/coreMQTT/mqttFilePaths.cmake)

  set(LIBPEER_SRCS
    ${LIBPEER_CORE_SRCS}
    ${HTTP_SOURCES}
    ${MQTT_SOURCES}
    ${MQTT_SERIALIZER_SOURCES}
  )

  zephyr_library_sources(${LIBPEER_SRCS})

  target_compile_definitions(libpeer PRIVATE
    HTTP_DO_NOT_USE_CUSTOM_CONFIG
    MQTT_DO_NOT_USE_CUSTOM_CONFIG
    CONFIG_USE_USRSCTP=0
  )

  zephyr_library_include_directories(
    ${CMAKE_CURRENT_LIST_DIR}/../src
    ${HTTP_INCLUDE_PUBLIC_DIRS}
    ${MQTT_INCLUDE_PUBLIC_DIRS}
  )

  zephyr_include_directories(
    ${CMAKE_CURRENT_LIST_DIR}/../src
    ${HTTP_INCLUDE_PUBLIC_DIRS}
    ${MQTT_INCLUDE_PUBLIC_DIRS}
  )

  zephyr_library_link_libraries(cjson)
  zephyr_library_link_libraries(srtp)
endif()
