cmake_minimum_required(VERSION 3.0)

project(basisu_transcoder_js)

if (EMSCRIPTEN)
  set(CMAKE_CXX_STANDARD 11)

  add_executable(basis_transcoder.js
    ../../transcoder/basisu_transcoder.cpp
    basis_wrappers.cpp
  )

  target_compile_definitions(basis_transcoder.js PRIVATE NDEBUG BASISD_SUPPORT_UASTC=1 BASISD_SUPPORT_BC7=1 BASISD_SUPPORT_ATC=0 BASISD_SUPPORT_ASTC_HIGHER_OPAQUE_QUALITY=0 BASISD_SUPPORT_PVRTC2=0 BASISD_SUPPORT_FXT1=0 BASISD_SUPPORT_ETC2_EAC_RG11=0 BASISU_SUPPORT_ENCODING=0)
  target_compile_options(basis_transcoder.js PRIVATE -O3 -fno-strict-aliasing)
  target_include_directories(basis_transcoder.js PRIVATE ../../transcoder)

  string(CONCAT LINK_FLAGS_STR 
    "--bind "
    "-s EXPORTED_FUNCTIONS='["
      "\"_hehe\","
      "\"_haha\","
      "\"_basis_init\","
      "\"_is_format_supported\","
      "\"_get_bytes_per_block_or_pixel\","
      "\"_get_format_block_width\","
      "\"_get_format_block_height\","
      "\"_format_is_uncompressed\","
      "\"_init_transcoder\","
      "\"_decode_palettes\","
      "\"_decode_tables\","
      "\"_transcode_image\","
      "\"_deinit_transcoder\""
      # "\"_init_etc1s_transcoder\","
      # "\"_etc1s_transcoder_decode_palettes\","
      # "\"_etc1s_transcoder_decode_tables\","
      # "\"_etc1s_transcoder_transcode_image\","
      # "\"_deinit_etc1s_transcoder\""
    "]' "
    "-s EXPORTED_RUNTIME_METHODS='[\"ccall\",\"cwrap\"]' "
    "-s ALLOW_MEMORY_GROWTH=1 "
    "-O2 "
    "-s ASSERTIONS=0 "
    "-s MALLOC=emmalloc "
    "-s MODULARIZE=1 "
    "-s EXPORT_NAME=BASIS "
    # "-s WASM=1 "
    # "-s RESERVED_FUNCTION_POINTERS=1 "
    # "-s FORCE_FILESYSTEM=1 "
    # "-s LLD_REPORT_UNDEFINED "
  )
  
  set_target_properties(basis_transcoder.js PROPERTIES
      OUTPUT_NAME "basis_transcoder"
      SUFFIX ".js"
      LINK_FLAGS ${LINK_FLAGS_STR})

  message("\nLINK_FLAGS: \n")
  message(${LINK_FLAGS_STR})
  message("\n")
endif()
