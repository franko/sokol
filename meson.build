project('sokol', 'c', default_options : ['c_std=c99'])

# See https://github.com/floooh/sokol-tools/blob/master/docs/sokol-shdc.md
# for a list of Sokol target and related shader dialects (slang) to use.

sokol_target = get_option('target')
sokol_impl_def = '-DSOKOL_' + sokol_target.to_upper()
sokol_c_args = [ sokol_impl_def ]

cc = meson.get_compiler('c')

sokol_gfx_deps = []
if host_machine.system() == 'windows'
    foreach lib : ['kernel32', 'user32', 'shell32', 'gdi32']
        sokol_gfx_deps += cc.find_library(lib)
    endforeach
    if sokol_target == 'd3d11'
        foreach lib : ['dxgi', 'd3d11']
            sokol_gfx_deps += cc.find_library(lib)
        endforeach
    endif
elif host_machine.system() == 'linux'
    foreach lib : ['m', 'dl']
        sokol_gfx_deps += cc.find_library(lib)
    endforeach
    sokol_gfx_deps += dependency('opengl')
endif

if get_option('default_library') != 'static'
    sokol_c_args += '-DSOKOL_DLL'
endif

libsokol_gfx = library('libsokol-gfx', 'impl/sokol_gfx.c',
    c_args : sokol_c_args,
    dependencies : sokol_gfx_deps,
    install : true,
)

install_headers('sokol_gfx.h')

pkg = import('pkgconfig')

pkg.generate(libsokol_gfx,
    filebase : 'libsokol-gfx',
    name : 'libsokol-gfx',
    extra_cflags : sokol_impl_def,
    description : 'Sokol GFX library for 3D graphics',
    url : 'github.com/floooh/sokol',
)

