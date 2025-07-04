find_package(Torch REQUIRED)
find_package(Python3 COMPONENTS Interpreter Development)

add_subdirectory(proto)


add_library(grpc_ps_client
    grpc_ps_client.cpp grpc_ps_client.h
)

target_link_libraries(
    grpc_ps_client PUBLIC
    base
    base_ps
    ps_proto
    gRPC::grpc++_reflection
    gRPC::grpc++
    protobuf::libprotobuf
)

add_executable(grpc_ps_client_test
    grpc_ps_client_test.cpp
)
target_link_libraries(grpc_ps_client_test PUBLIC
    grpc_ps_client
)


add_executable(grpc_ps_server
    grpc_ps_server.cpp
    grpc_ps_server.h
)

target_link_libraries(grpc_ps_server PUBLIC
    base
    base_ps
    ps_proto
    gRPC::grpc++_reflection
    gRPC::grpc++
    protobuf::libprotobuf
    stdc++fs
    kv_engine
)

# add_executable(grpc_ps_server_async
#     grpc_ps_server_async.cpp 
#     grpc_ps_server_async.h 
#     cache_ps_impl.h
#     ${common}
# )

# target_link_libraries(grpc_ps_server_async PUBLIC
#     base
#     ps_proto
#     gRPC::grpc++_reflection
#     gRPC::grpc++
#     protobuf::libprotobuf
#     stdc++fs
#     kv_engine
# )


add_library(grpc_ps_client_python MODULE
    grpc_ps_client_pytorch.cpp
)

target_include_directories(grpc_ps_client_python PUBLIC ${TORCH_INCLUDE_DIRS})

target_link_libraries(grpc_ps_client_python PUBLIC
    grpc_ps_client
    ${TORCH_LIBRARIES}
    Python3::Python
)
