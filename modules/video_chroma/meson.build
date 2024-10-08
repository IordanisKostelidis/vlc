# chroma copy helper library
chroma_copy_lib_srcs = files('copy.c')
chroma_copy_lib = static_library(
    'chroma_copy',
    chroma_copy_lib_srcs,
    include_directories: [vlc_include_dirs],
    install: false,
    pic: true
)

vlc_modules += {
    'name' : 'chain',
    'sources' : files('chain.c')
}

swscale_dep = dependency('libswscale', version: '>= 0.5.0', required: get_option('swscale'))
vlc_modules += {
    'name' : 'swscale',
    'sources' : files(
      'swscale.c',
      '../codec/avcodec/chroma.c'
    ),
    'dependencies' : [swscale_dep, m_lib],
    'link_args' : symbolic_linkargs,
    'enabled' : swscale_dep.found(),
}

vlc_modules += {
    'name' : 'grey_yuv',
    'sources' : files('grey_yuv.c')
}

vlc_modules += {
    'name' : 'i420_rgb',
    'sources' : files(
        'i420_rgb.c',
        'i420_rgb8.c',
        'i420_rgb16.c',
    )
}

vlc_modules += {
    'name' : 'i420_yuy2',
    'sources' : files('i420_yuy2.c'),
}

vlc_modules += {
    'name' : 'i420_nv12',
    'sources' : files('i420_nv12.c'),
    'link_with' : [chroma_copy_lib],
}

vlc_modules += {
    'name' : 'i422_i420',
    'sources' : files('i422_i420.c')
}

vlc_modules += {
    'name' : 'i422_yuy2',
    'sources' : files('i422_yuy2.c'),
}

vlc_modules += {
    'name' : 'rv32',
    'sources' : files('rv32.c')
}

vlc_modules += {
    'name' : 'yuy2_i420',
    'sources' : files('yuy2_i420.c')
}

vlc_modules += {
    'name' : 'yuy2_i422',
    'sources' : files('yuy2_i422.c')
}

vlc_modules += {
    'name' : 'yuvp',
    'sources' : files('yuvp.c')
}

vlc_modules += {
    'name' : 'i420_rgb_sse2',
    'sources' : files(
        'i420_rgb.c',
        'i420_rgb16_x86.c'
    ),
    'c_args' : ['-DPLUGIN_SSE2'],
    'enabled' : have_sse2,
}

vlc_modules += {
    'name' : 'i420_yuy2_sse2',
    'sources' : files('i420_yuy2.c'),
    'c_args' : ['-DPLUGIN_SSE2'],
    'enabled' : have_sse2,
}

vlc_modules += {
    'name' : 'i422_yuy2_sse2',
    'sources' : files('i422_yuy2.c'),
    'c_args' : ['-DPLUGIN_SSE2'],
    'enabled' : have_sse2,
}

vlc_modules += {
    'name' : 'orient',
    'sources' : files('orient.c'),
}

# CVPX chroma converter
# TODO: Set minimum versions for tvOS and iOS
vlc_modules += {
    'name' : 'cvpx',
    'sources' : files(
        '../codec/vt_utils.c',
        'cvpx.c',
    ),
    'link_with' : [chroma_copy_lib],
    'dependencies' : [
        frameworks['VideoToolbox'],
        frameworks['Foundation'],
        frameworks['CoreMedia'],
        frameworks['CoreVideo'],
    ],
    'enabled' : host_system == 'darwin',
}

## Tests

if host_system != 'windows' # can't use alarm
# Chroma copy SSE test
chroma_copy_sse_test = executable(
    'chroma_copy_sse_test',
    chroma_copy_lib_srcs,
    c_args: ['-DCOPY_TEST'],
    dependencies: [libvlccore_dep],
    include_directories: [vlc_include_dirs]
)
test('chroma_copy_sse', chroma_copy_sse_test, suite: 'video_chroma')

# Chroma copy test
chroma_copy_test = executable(
    'chroma_copy_test',
    chroma_copy_lib_srcs,
    c_args: ['-DCOPY_TEST', '-DCOPY_TEST_NOOPTIM'],
    dependencies: [libvlccore_dep],
    include_directories: [vlc_include_dirs]
)
test('chroma_copy', chroma_copy_test, suite: 'video_chroma')
endif
