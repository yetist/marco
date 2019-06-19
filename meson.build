project(
  'marco', 'c',
  version: '1.23.1',
  meson_version: '>=0.47.0',
  license: 'GPL2',
  default_options : [ 'warning_level=1', 'buildtype=debugoptimized', 'c_std=gnu11'],
)

# Versionning
version_arr = meson.project_version().split('.')
version_major = version_arr[0].to_int()
version_minor = version_arr[1].to_int()
version_micro = version_arr[2].to_int()

soversion = 2

if version_minor.is_odd()
  interface_age = 0
else
  interface_age = version_micro
endif

current = version_minor * 100 + version_micro - interface_age
revision = interface_age
libversion = '@0@.@1@.@2@'.format(soversion, current, revision)

# Compiler & Project arguments
add_project_arguments([
  '-DHAVE_CONFIG_H',
  '-I' + meson.build_root(),
  ], language: 'c')

# get suported warning flags
test_args = [
  #'-fstack-protector-strong',
  #'-Waggregate-return',
  #'-Warray-bounds',
  #'-Wcast-align',
  #'-Wclobbered',
  #'-Wdeclaration-after-statement',
  #'-Wempty-body',
  #'-Wextra',
  #'-Wformat=2',
  #'-Wformat-nonliteral',
  #'-Wformat-security',
  #'-Wformat-signedness',
  #'-Wignored-qualifiers',
  #'-Wimplicit-function-declaration',
  #'-Winit-self',
  #'-Winline',
  #'-Wmissing-declarations',
  #'-Wmissing-format-attribute',
  #'-Wmissing-include-dirs',
  #'-Wmissing-noreturn',
  #'-Wmissing-parameter-type',
  #'-Wmissing-prototypes',
  #'-Wnested-externs',
  #'-Wno-discarded-qualifiers',
  #'-Wno-missing-field-initializers',
  #'-Wno-strict-aliasing',
  #'-Wno-suggest-attribute=format',
  #'-Wno-unused-parameter',
  #'-Wold-style-definition',
  #'-Woverride-init',
  #'-Wpacked',
  #'-Wpointer-arith',
  #'-Wredundant-decls',
  #'-Wreturn-type',
  #'-Wshadow',
  #'-Wsign-compare',
  #'-Wstrict-aliasing',
  #'-Wstrict-prototypes',
  #'-Wswitch-default',
  #'-Wtype-limits',
  #'-Wundef',
  #'-Wuninitialized',
  #'-Wunused-but-set-variable',
  #'-Wwrite-strings'
  '-Wchar-subscripts',
  '-Wmissing-declarations',
  '-Wmissing-prototypes',
  '-Wnested-externs',
  '-Wpointer-arith',
  '-Wcast-align',
  '-Wsign-compare',
  '-Wall',
  '-ansi',
  '-MD',
  '-MP',
  ]

cc = meson.get_compiler('c')

foreach arg: test_args
  if cc.has_argument(arg)
    add_project_arguments(arg, language : 'c')
  endif
endforeach

gnome = import('gnome')
i18n = import('i18n')
pkg = import('pkgconfig')

# Paths
rootdir = include_directories('.')

srcdir = meson.current_source_dir()
builddir = meson.current_build_dir()

prefix = get_option('prefix')

package_name = meson.project_name()
package_string = '@0@-@1@.0'.format(package_name, soversion)

bindir = join_paths(prefix, get_option('bindir'))
includedir = join_paths(prefix, get_option('includedir'))
libdir = join_paths(prefix, get_option('libdir'))
libexecdir = join_paths(prefix, get_option('libexecdir'))
datadir = join_paths(prefix, get_option('datadir'))
pkgincludedir = join_paths(includedir, package_string)
pkgconfigdir = join_paths(libdir, 'pkgconfig')
pkglibdir = join_paths(libdir, package_name)
pkgdatadir = join_paths(datadir, package_name)

dbusdir = join_paths(datadir, 'dbus-1')
glibdir = join_paths(datadir, 'glib-2.0')
gtkdocdir = join_paths(datadir, 'gtk-doc')
localdir = join_paths(datadir, 'local')
vapidir = join_paths(datadir, 'vala', 'vapi')


# Dependencies
glib_req = '>= 2.58.0'
gio_req = '>= 2.25.10'
gtk_req = '>= 3.22.0'
pango_req = '>= 1.2.0'
libcanberra_gtk_req = '>= 3.22.0'
startup_notification_req = '=0.7'

libm = cc.find_library('m', required: false)
glib = dependency('glib-2.0', version: glib_req)
gtk = dependency('gtk+-3.0', version: gtk_req)
gio = dependency('gio-2.0', version: gio_req)
pango = dependency('pango', version: pango_req)
libcanberra_gtk = dependency('libcanberra-gtk3')

xcomposite_req ='>= 0.3'
xrender_req ='>= 0.0'
startup_notification_dep = dependency('libstartup-notification-1.0', version:startup_notification_req, required: false)
xcomposite_dep = dependency('xcomposite', version:xcomposite_req, required: false)
xrender_dep = dependency('xrender', version:xrender_req, required: false)
xcursor_dep = dependency('xcursor', required: false)
libgtop_dep = dependency('libgtop-2.0', required: false)

build_startup_notification = not get_option('disable-startup-notification')
if build_startup_notification and not startup_notification_dep.found()
  build_startup_notification = false
endif

build_xcomposite = not get_option('disable-compositor')
if build_xcomposite and not xcomposite_dep.found()
  build_xcomposite = false
endif

build_render = not get_option('disable-render')
if build_render and not xrender_dep.found()
  build_render = false
endif

build_xcursor = true
if not xcursor_dep.found()
  build_xcursor = false
endif

build_libgtop = true
if not libgtop_dep.found()
  build_libgtop = false
endif

#build_xinerama = true
#if not xinerama_dep.found()
#  build_xinerama = false
#endif

#build_shape = not get_option('disable-shape')
#if build_shape and not shape_dep.found()
#  build_shape = false
#endif

#build_xsync = not get_option('disable-xsync')
#if build_xsync and not xsync_dep.found()
#  build_xsync = false
#endif

#build_ = not get_option('disable-')
#if build_ and not _dep.found()
#  build_ = false
#endif
#build_ = not get_option('disable-')
#if build_ and not _dep.found()
#  build_ = false
#endif

build_xcomposite = not get_option('disable-sm')
build_xcomposite = not get_option('disable-startup-notification')
build_xcomposite = not get_option('disable-startup-notification')
xrandr_dep = dependency('xrandr', required : false)

#build_gtk_doc = get_option('documentation')
#if build_gtk_doc and not gtk_doc_dep.found()
#  build_gtk_doc = false
#endif

config_h = configuration_data()
config_h.set_quoted('PACKAGE', meson.project_name())
config_h.set_quoted('PACKAGE_NAME', meson.project_name())
config_h.set_quoted('PACKAGE_STRING', package_string)
config_h.set_quoted('PACKAGE_VERSION', meson.project_version())
config_h.set_quoted('GETTEXT_PACKAGE', meson.project_name())
config_h.set_quoted('MARCO_LOCALEDIR', localdir)
config_h.set_quoted('LIBDIR', libdir)
config_h.set_quoted('MARCO_DATADIR', datadir)
config_h.set_quoted('VERSION', meson.project_version())

#enable_gvfs_metadata = get_option('enable-gvfs-metadata')
#if enable_gvfs_metadata == 'yes' or (enable_gvfs_metadata == 'auto' and host_machine.system() == 'linux')
#  enable_gvfs_metadata = true
#else
#  enable_gvfs_metadata = false
#endif

#if enable_gvfs_metadata
#  config_h.set('ENABLE_GVFS_METADATA', 1)
#endif

config_h.set('GEDIT_MAJOR_VERSION', version_major)
config_h.set('GEDIT_MINOR_VERSION', version_minor)
config_h.set('GEDIT_MICRO_VERSION', version_micro)

configure_file(
  output: 'config.h',
  configuration: config_h
)

module_suffix = []
# Keep the autotools convention for shared module suffix because GModule
# depends on it: https://gitlab.gnome.org/GNOME/glib/issues/520
if ['darwin', 'ios'].contains(host_machine.system())
  module_suffix = 'so'
  add_languages('objc')
endif

# Options
#build_plugins = get_option('plugins')

#generate_gir = get_option('introspection')
#if generate_gir and not introspection_dep.found()
#  generate_gir = false
#endif
#
#generate_vapi = generate_gir and get_option('vapi')
#if generate_vapi and not vapigen_dep.found()
#  generate_vapi = false
#endif


subdir('doc')
subdir('src')
subdir('po')

summary = [
  '',
  'marco-@0@:'.format(meson.project_version()),
  '',
  '	prefix:                   @0@'.format(prefix),
  '	source code location:     @0@'.format(srcdir),
  #  '	compiler:                 ${CC}'.format(),
  '',
  #'	XFree86 Xinerama:         @0@'.build_format(),
  #  '	Solaris Xinerama:         @0@'.format(),
  '	Startup notification:     @0@'.format(build_startup_notification),
  '	Compositing manager:      @0@'.format(build_xcomposite),
  #  '	Session management:       @0@'.format(),
  #'	Shape extension:          @0@'.format(build_xsync),
  #  '	Resize-and-rotate:        @0@'.format(),
  #  '	Xsync:                    @0@'.format(),
  #  '	Xpresent:                 @0@'.format(),
  #'	Render:                   @0@'.format(build_render),
  #'	Xcursor:                  @0@'.format(build_xcursor),
  '',
]

meson.add_install_script(
  'meson_post_install.py',
  libdir, datadir,
)

message('\n'.join(summary))
