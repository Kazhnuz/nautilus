project('nautilus', 'c',
  version: '3.27.2',
  meson_version: '>= 0.41.0',
  license: 'GPL3+'
)

add_global_arguments('-Wno-deprecated-declarations', language: 'c')

# Directory variables
bindir = get_option('bindir')
datadir = get_option('datadir')
desktopdir = join_paths(datadir, 'applications')
includedir = get_option('includedir')
libdir = get_option('libdir')
extensiondir = join_paths(libdir, 'nautilus', 'extensions-3.0')
prefix = get_option('prefix')
servicedir = join_paths(datadir, 'dbus-1', 'services')
#

# Compiler variables
cc = meson.get_compiler('c')
#

# Imports
gnome = import('gnome')
i18n = import('i18n')
#

# Dependencies
glib_ver = '>= 2.51.2'

libgd = subproject(
  'libgd',
  default_options: [
    'static=true',
    'with-gtk-hacks=true',
    'with-main-view=true',
    'with-tagged-entry=true'
  ]
)
libgd_dep = libgd.get_variable('libgd_dep')

libm = cc.find_library('libm')

gail = dependency('gail-3.0')
gexiv = dependency('gexiv2', version: '>= 0.10.0')
gio = dependency('gio-2.0', version: glib_ver)
gio_unix = dependency('gio-unix-2.0', version: glib_ver)
glib = dependency('glib-2.0', version: glib_ver)
gmodule = dependency('gmodule-no-export-2.0', version: glib_ver)
gnome_autoar = dependency('gnome-autoar-0', version: '>= 0.2.1')
gnome_desktop = dependency('gnome-desktop-3.0', version: '>= 3.0.0')
gtk = dependency('gtk+-3.0', version: '>= 3.22.26')
selinux = []
if get_option('selinux')
  selinux = dependency('libselinux', version: '>= 2.0')
endif
tracker_sparql = dependency('tracker-sparql-2.0')
x11 = dependency('x11')
xml = dependency('libxml-2.0', version: '>= 2.7.8')
#

# Configuration
conf = configuration_data()

conf.set_quoted('VERSION', meson.project_version())
conf.set_quoted('PACKAGE_VERSION', meson.project_version())
conf.set_quoted('GETTEXT_PACKAGE', 'nautilus')
conf.set_quoted('LOCALEDIR', join_paths(prefix, get_option('localedir')))
conf.set_quoted('NAUTILUS_DATADIR', join_paths(datadir, 'nautilus'))
conf.set_quoted('NAUTILUS_EXTENSIONDIR', join_paths(prefix, extensiondir))

if get_option('packagekit')
  conf.set10('ENABLE_PACKAGEKIT', true)
endif
if get_option('profiling')
  conf.set10('ENABLE_PROFILING', true)
endif
if get_option('selinux')
  conf.set10('HAVE_SELINUX', true)
endif

configure_file(output: 'config.h', configuration: conf)
#

nautilus_include_dirs = include_directories('.')

subdir('eel')
subdir('libnautilus-extension')

subdir('src')
subdir('test')
subdir('data')
subdir('po')
if get_option('docs')
  subdir('docs')
endif
if get_option('nst_extension')
  subdir('nautilus-sendto-extension')
endif

# Compile GSettings schemas when installing from source.
meson.add_install_script('build-aux/meson/postinstall.py')
