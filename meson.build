project('FFT3DFilter', 'cpp',
  default_options : ['buildtype=release', 'b_ndebug=if-release', 'cpp_std=c++17'],
  license : 'GPL2',
  meson_version : '>=0.49.0',
  version : '2'
)

cc = meson.get_compiler('cpp')

sources = [
  'FFT3DFilter.cpp',
  'FFT3DFilter.h',
  'fft3dfilter_c.cpp',
  'FFT3DFilterTransform.cpp',
  'Plugin.cpp'
]

if cc.get_argument_syntax() == 'msvc'
  fftw3f_dep = dependency('fftwf')
  deps = [fftw3f_dep]
  install_dir = 'installed'  # dummy
else
  vapoursynth_dep = dependency('vapoursynth')
  fftw3f_dep = dependency('fftw3f')
  deps = [vapoursynth_dep, fftw3f_dep]
  install_dir = join_paths(vapoursynth_dep.get_pkgconfig_variable('libdir'), 'vapoursynth')
  if host_machine.cpu_family().startswith('x86')
    add_project_arguments('-mfpmath=sse', '-msse2', language : 'cpp')
  endif
endif

code = '''
#include <fftw3.h>
int main() {
    fftwf_init_threads();
    return 0;
}
'''
if not cc.links(code, dependencies : fftw3f_dep)
  deps += cc.find_library('fftw3f_threads')
endif

shared_module('fft3dfilter', sources,
  dependencies : deps,
  install : true,
  install_dir : install_dir,
  gnu_symbol_visibility : 'hidden'
)
