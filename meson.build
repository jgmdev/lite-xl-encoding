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
compiler_args = ['-DSDL_MAIN_HANDLED']

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
default_fallback_options = ['warning_level=0', 'werror=false']

# Lua
lua_dep = dependency('lua-5.4',
    version: '>= 5.4',
    fallback: ['lua', 'lua_dep'],
    default_options: default_fallback_options + [
        'default_library=static', 'line_editing=false', 'interpreter=false'
    ]
)

# SDL2
sdl_options = ['default_library=static']

# we explicitly need these
sdl_options += 'test=false'
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
sdl_options += 'test=false'
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
    [
        # uchardet sources
        'uchardet/CharDistribution.cpp',
        'uchardet/JpCntx.cpp',
        'uchardet/LangModels/LangArabicModel.cpp',
        'uchardet/LangModels/LangBulgarianModel.cpp',
        'uchardet/LangModels/LangCroatianModel.cpp',
        'uchardet/LangModels/LangCzechModel.cpp',
        'uchardet/LangModels/LangEsperantoModel.cpp',
        'uchardet/LangModels/LangEstonianModel.cpp',
        'uchardet/LangModels/LangFinnishModel.cpp',
        'uchardet/LangModels/LangFrenchModel.cpp',
        'uchardet/LangModels/LangDanishModel.cpp',
        'uchardet/LangModels/LangGermanModel.cpp',
        'uchardet/LangModels/LangGreekModel.cpp',
        'uchardet/LangModels/LangHungarianModel.cpp',
        'uchardet/LangModels/LangHebrewModel.cpp',
        'uchardet/LangModels/LangIrishModel.cpp',
        'uchardet/LangModels/LangItalianModel.cpp',
        'uchardet/LangModels/LangLithuanianModel.cpp',
        'uchardet/LangModels/LangLatvianModel.cpp',
        'uchardet/LangModels/LangMalteseModel.cpp',
        'uchardet/LangModels/LangPolishModel.cpp',
        'uchardet/LangModels/LangPortugueseModel.cpp',
        'uchardet/LangModels/LangRomanianModel.cpp',
        'uchardet/LangModels/LangRussianModel.cpp',
        'uchardet/LangModels/LangSlovakModel.cpp',
        'uchardet/LangModels/LangSloveneModel.cpp',
        'uchardet/LangModels/LangSwedishModel.cpp',
        'uchardet/LangModels/LangSpanishModel.cpp',
        'uchardet/LangModels/LangThaiModel.cpp',
        'uchardet/LangModels/LangTurkishModel.cpp',
        'uchardet/LangModels/LangVietnameseModel.cpp',
        'uchardet/nsHebrewProber.cpp',
        'uchardet/nsCharSetProber.cpp',
        'uchardet/nsBig5Prober.cpp',
        'uchardet/nsEUCJPProber.cpp',
        'uchardet/nsEUCKRProber.cpp',
        'uchardet/nsEUCTWProber.cpp',
        'uchardet/nsEscCharsetProber.cpp',
        'uchardet/nsEscSM.cpp',
        'uchardet/nsGB2312Prober.cpp',
        'uchardet/nsMBCSGroupProber.cpp',
        'uchardet/nsMBCSSM.cpp',
        'uchardet/nsSBCSGroupProber.cpp',
        'uchardet/nsSBCharSetProber.cpp',
        'uchardet/nsSJISProber.cpp',
        'uchardet/nsUTF8Prober.cpp',
        'uchardet/nsLatin1Prober.cpp',
        'uchardet/nsUniversalDetector.cpp',
        'uchardet/uchardet.cpp',
        # encoding lua extension
        'src/encoding.c'
    ],
    name_prefix: '',
    name_suffix: library_suffix,
    include_directories: ['uchardet'],
    dependencies: [lua_dep, sdl_dep],
    c_args: compiler_args,
    link_args: link_args
)
