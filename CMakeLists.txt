cmake_minimum_required(VERSION 3.5)

include(android)
include(prebuilts)
# Make sure the standard set of windows libraries are available as targets
if(WINDOWS)
  include(emu-windows-libs)
endif()

android_add_library(TARGET cares
                    LIBNAME c-ares URL
                            "https://android.googlesource.com/platform/external/c-ares/+/refs/heads/emu-master-dev"
                    REPO "${ANDROID_QEMU2_TOP_DIR}/../cares/"
                    NOTICE "REPO/NOTICE"
                    LICENSE MIT
                    SRC ares__close_sockets.c
                        ares__get_hostent.c
                        ares__read_line.c
                        ares__timeval.c
                        ares_cancel.c
                        ares_create_query.c
                        ares_data.c
                        ares_destroy.c
                        ares_expand_name.c
                        ares_expand_string.c
                        ares_fds.c
                        ares_free_hostent.c
                        ares_free_string.c
                        ares_getenv.c
                        ares_gethostbyaddr.c
                        ares_gethostbyname.c
                        ares_getnameinfo.c
                        ares_getopt.c
                        ares_getsock.c
                        ares_init.c
                        ares_library_init.c
                        ares_llist.c
                        ares_mkquery.c
                        ares_nowarn.c
                        ares_options.c
                        ares_parse_a_reply.c
                        ares_parse_aaaa_reply.c
                        ares_parse_mx_reply.c
                        ares_parse_naptr_reply.c
                        ares_parse_ns_reply.c
                        ares_parse_ptr_reply.c
                        ares_parse_soa_reply.c
                        ares_parse_srv_reply.c
                        ares_parse_txt_reply.c
                        ares_platform.c
                        ares_process.c
                        ares_query.c
                        ares_search.c
                        ares_send.c
                        ares_strcasecmp.c
                        ares_strdup.c
                        ares_strerror.c
                        ares_timeout.c
                        ares_version.c
                        ares_writev.c
                        bitncmp.c
                        inet_net_pton.c
                        inet_ntop.c
                        windows_port.c)

target_compile_definitions(cares
                           PUBLIC
                           -DCARES_BUILDING_LIBRARY
                           -D_GNU_SOURCE
                           -D_HAS_EXCEPTIONS=0
                           -DHAVE_CONFIG_H
                           -DCARES_STATICLIB)
target_include_directories(cares PUBLIC .)
android_target_compile_definitions(cares
                                   windows
                                   PRIVATE
                                   -DNOMINMAX
                                   -D_CRT_SECURE_NO_DEPRECATE
                                   -D_CRT_NONSTDC_NO_DEPRECATE
                                   -D_WIN32_WINNT=0x0601)

android_target_link_libraries(cares windows PRIVATE ws2_32::ws2_32)
android_target_include_directories(cares linux-x86_64 PRIVATE config_linux)
android_target_include_directories(cares linux-aarch64 PRIVATE config_linux)
android_target_include_directories(cares darwin-x86_64 PRIVATE config_darwin)
android_target_include_directories(cares windows PRIVATE config_windows)
