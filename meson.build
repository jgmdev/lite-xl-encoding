project('encoding',
    ['c', 'cpp'],
    version : '1.0',
    license : 'MIT',
    default_options : [
        'c_std=c11',
        'cpp_std=c++11',
        'buildtype=release',
        'wrap_mode=forcefallback'
    ]
)

#===============================================================================
# Compiler Settings
#===============================================================================
cc = meson.get_compiler('c')
compiler_args = ['-DSDL_MAIN_HANDLED', '-Wno-declaration-after-statement']

#===============================================================================
# Linker Settings
#===============================================================================
link_args = []
if cc.get_id() == 'gcc' and get_option('buildtype') == 'release'
    link_args += ['-static-libgcc']
endif

if host_machine.system() == 'darwin'
    link_args += ['-framework', 'CoreServices', '-framework', 'Foundation']
endif

#===============================================================================
# Dependencies
#===============================================================================
default_fallback_options = ['warning_level=0', 'werror=false', 'default_library=static']

# Optional Lua
use_lua = not get_option('use_lua').disabled()

if use_lua
    lua_dep = dependency('lua-5.4',
        version: '>= 5.4',
        fallback: ['lua', 'lua_dep'],
        default_options: default_fallback_options + [
            'line_editing=false', 'interpreter=false'
        ]
    )
    compiler_args += '-DUSE_LUA'
endif

# uchardet
uchardet_dep = dependency('uchardet', fallback: ['uchardet', 'uchardet_dep'],
    default_options: default_fallback_options
)

# SDL2
sdl_options = []

# we explicitly need these
sdl_options += 'test=false'
sdl_options += 'run_test=false'
sdl_options += 'use_loadso=enabled'
sdl_options += 'prefer_dlopen=true'
sdl_options += 'use_video=enabled'
sdl_options += 'use_atomic=enabled'
sdl_options += 'use_threads=enabled'
sdl_options += 'use_timers=enabled'
sdl_options += 'use_events=enabled'

if host_machine.system() == 'darwin' or host_machine.system() == 'windows'
    sdl_options += 'use_video_x11=disabled'
    sdl_options += 'use_video_wayland=disabled'
else
    sdl_options += 'use_render=enabled'
    sdl_options += 'use_video_x11=auto'
    sdl_options += 'use_video_wayland=auto'
endif

# we leave this up to what the host system has except on windows
if host_machine.system() != 'windows'
    sdl_options += 'use_video_opengl=auto'
    sdl_options += 'use_video_openglesv2=auto'
else
    sdl_options += 'use_video_opengl=disabled'
    sdl_options += 'use_video_openglesv2=disabled'
endif

# we don't need these
sdl_options += 'use_sensor=disabled'
sdl_options += 'use_haptic=disabled'
sdl_options += 'use_audio=disabled'
sdl_options += 'use_cpuinfo=disabled'
sdl_options += 'use_joystick=disabled'
sdl_options += 'use_video_vulkan=disabled'
sdl_options += 'use_video_offscreen=disabled'
sdl_options += 'use_power=disabled'

sdl_dep = dependency('sdl2',
    fallback: ['sdl2', 'sdl2_dep'],
    default_options: default_fallback_options + sdl_options
)

# Dependencies list
encoding_dependencies = [sdl_dep, uchardet_dep]

if use_lua
    encoding_dependencies += lua_dep
endif

#===============================================================================
# Library Suffix
#===============================================================================
library_suffix = []

# On macOS the default suffix is .dylib but on lite-xl we need .so
if host_machine.system() == 'darwin'
    library_suffix = 'so'
endif

#===============================================================================
# Plugin Library
#===============================================================================
shared_library(
    'encoding',
    ['src/encoding.c'],
    name_prefix: '',
    name_suffix: library_suffix,
    dependencies: encoding_dependencies,
    c_args: compiler_args,
    link_args: link_args
)
