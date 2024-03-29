# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

add_library(
  file_channel
  STATIC
    file_channel.cc
)
target_link_libraries(
  file_channel
    file_ops
    logging
)

add_library(
  double_write_channel
  STATIC
    double_write_channel.cc
)
target_link_libraries(
  double_write_channel
    logging
)

add_library(
  buffered_writer
  STATIC
    buffered_writer.cc
)
target_link_libraries(
  buffered_writer
    logging
)

add_library(
  tcp_channel
  STATIC
    tcp_channel.cc
)
target_link_libraries(
  tcp_channel
    error_handling
    uv_helpers
    libuv-interface
    logging
)

add_library(
  lz4_channel
  STATIC
    lz4_channel.cc
)
target_link_libraries(
  lz4_channel
    logging
    lz4
)

add_library(
  upstream_connection
  STATIC
    upstream_connection.cc
)
target_link_libraries(
  upstream_connection
    double_write_channel
    lz4_channel
    buffered_writer
    logging
)

add_library(
  reconnecting_channel
  STATIC
    reconnecting_channel.cc
)

target_link_libraries(
  reconnecting_channel
    upstream_connection
    spdlog
    libuv-interface
    render_ebpf_net_artifacts
)

add_library(
  connection_caretaker
  STATIC
    connection_caretaker.cc
)
target_link_libraries(
  connection_caretaker
    agent_id
    aws_instance_metadata
    gcp_instance_metadata
    intake_config
    element_queue_writer
    fastpass_util
    render_ebpf_net_artifacts
    logging
    tcp_channel
    absl::strings
)

add_library(
  test_channel
  STATIC
    test_channel.cc
)
target_link_libraries(
  test_channel
    render_pipeline
    wire_msg_to_json
    llvm
)

add_unit_test(buffered_writer LIBS buffered_writer element_queue_writer llvm)
