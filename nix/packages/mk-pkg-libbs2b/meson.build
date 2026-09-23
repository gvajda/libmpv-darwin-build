# Native meson build for libbs2b, bypassing the upstream autotools/libtool
# scaffolding. The upstream configure requires a C++ compiler (for the
# bs2bclass.cpp wrapper) and libsndfile (only used by the bs2bconvert/bs2bstream
# CLI tools), neither of which are needed for FFmpeg's --enable-libbs2b probe.
# Building just the library is enough. Matches the layout produced by upstream
# Makefile.am (lib_LTLIBRARIES = libbs2b; headers under $includedir/bs2b).

project('libbs2b', 'c',
    version: '3.1.0',
    license: 'MIT',
    default_options: ['c_std=c99'],
)

cc = meson.get_compiler('c')
libm_dep = cc.find_library('m', required: false)

bs2b_lib = shared_library('bs2b',
    'src/bs2b.c',
    include_directories: include_directories('src'),
    dependencies: libm_dep,
    install: true,
    version: '0.0.0',
    soversion: '0',
)

install_headers(
    [
        'src/bs2b.h',
        'src/bs2btypes.h',
        'src/bs2bversion.h',
    ],
    subdir: 'bs2b',
)

pkg = import('pkgconfig')
pkg.generate(bs2b_lib,
    name: 'libbs2b',
    description: 'Bauer stereophonic-to-binaural DSP library',
    version: meson.project_version(),
    subdirs: 'bs2b',
    libraries_private: '-lm',
)

libbs2b_dep = declare_dependency(
    link_with: bs2b_lib,
    include_directories: include_directories('src'),
)
meson.override_dependency('libbs2b', libbs2b_dep)
