project('libcursespp', 'cpp')

inc = include_directories('include')
subdir('include')
subdir('src')

curses_dep = dependency('curses')

libcursespp = library('cursescpp',
  sources,
  include_directories : inc,
  install : true,
  dependencies : curses_dep)

libcursespp_dep = declare_dependency(include_directories: inc,
  link_with : libcursespp)
