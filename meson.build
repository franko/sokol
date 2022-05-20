project('sokol', 'c', default_options : ['c_std=c99', 'c_winlibs=[]'])

# See https://github.com/floooh/sokol-tools/blob/master/docs/sokol-shdc.md
# for a list of Sokol target and related shader dialects (slang) to use.

sokol_target = get_option('target')
sokol_impl_def = '-DSOKOL_' + sokol_target.to_upper()
sokol_c_args = [ sokol_impl_def ]

cc = meson.get_compiler('c')

if host_machine.system() == 'windows'
    sokol_rt_libs = [ cc.find_library('kernel32'), cc.find_library('user32'), cc.find_library('shell32') ]
    if sokol_target == 'd3d11'
        sokol_3d_libs = [ cc.find_library('d3d11'), cc.find_library('dxgi') ]
    elif sokol_target in ['glcore33', 'gles2', 'gles3']
        sokol_3d_libs = [ cc.find_library('gdi32') ]
    endif
    sokol_app_libs = sokol_3d_libs
elif host_machine.system() == 'linux'
    sokol_rt_libs = [ cc.find_library('m'), cc.find_library('dl') ]
    sokol_3d_libs = [ dependency('opengl') ]
    sokol_app_libs = [ dependency('x11'), dependency('xcursor'), dependency('xi') ]
endif

if get_option('default_library') != 'static'
    sokol_c_args += '-DSOKOL_DLL'
endif

pkg = import('pkgconfig')

if get_option('gfx')
    sokol_gfx_deps = sokol_rt_libs + sokol_3d_libs

    libsokol_gfx = library('libsokol-gfx', 'impl/sokol_gfx.c',
        c_args : sokol_c_args,
        dependencies : sokol_gfx_deps,
        install : true,
    )

    install_headers('sokol_gfx.h')

    pkg.generate(libsokol_gfx,
        filebase : 'libsokol-gfx',
        name : 'libsokol-gfx',
        extra_cflags : sokol_impl_def,
        description : 'Sokol GFX library for 3D graphics',
        url : 'github.com/floooh/sokol',
    )
endif

if get_option('app')
    sokol_app_deps = sokol_rt_libs + sokol_app_libs

    libsokol_app = library('libsokol-app', 'impl/sokol_app.c',
        c_args : sokol_c_args,
        dependencies : sokol_app_deps,
        install : true,
    )

    install_headers('sokol_app.h')

    pkg.generate(libsokol_app,
        filebase : 'libsokol-app',
        name : 'libsokol-app',
        extra_cflags : sokol_impl_def,
        description : 'Sokol App library for 3D graphics',
        url : 'github.com/floooh/sokol',
    )
endif

