project('io.github.lainsce.Khronos', ['vala', 'c'], version: '3.5.1')
gnome = import('gnome')
i18n = import('i18n')

add_project_arguments([
		'--target-glib=2.40',
	],
	language: 'vala',
)
add_project_arguments(
    '-DGETTEXT_PACKAGE="@0@"'.format(meson.project_name()),
    language: 'c'
)

asresources = gnome.compile_resources(
    'as-resources', 'data/style.gresource.xml',
    source_dir: 'data',
    c_name: 'as'
)

sources = files(
    'src/Application.vala',
    'src/MainWindow.vala',
    'src/Prefs.vala',
    'src/Services/TaskManager.vala',
    'src/Services/FileManager.vala',
    'src/Services/Dialog.vala',
    'src/Widgets/LogRow.vala',
)

cc = meson.get_compiler('c')
m_dep = cc.find_library('m', required: true)
dependencies = [
    dependency('gtk4'),
    dependency('gee-0.8'),
    dependency('libadwaita-1'),
    dependency('json-glib-1.0'),
    m_dep,
]

if get_option('profile') == 'development'
  name_prefix = '(Development) '
  profile = 'Devel'
else
  name_prefix = ''
  profile = ''
endif

app_id = 'io.github.lainsce.Khronos@0@'.format(profile)

conf = configuration_data()
conf.set_quoted('APP_ID', app_id)
conf.set_quoted('G_LOG_DOMAIN', 'io.github.lainsce.Khronos@0@'.format(profile))
conf.set_quoted('NAME_PREFIX', name_prefix)
conf.set_quoted('PACKAGE_NAME', meson.project_name())
conf.set_quoted('PACKAGE_VERSION', meson.project_version())
conf.set_quoted('PROFILE', profile)
conf.set_quoted('VERSION', meson.project_version())
conf.set_quoted('GETTEXT_PACKAGE', meson.project_name())
conf.set_quoted('DATADIR', join_paths(get_option('prefix'), get_option('datadir')))
conf.set_quoted('GNOMELOCALEDIR', join_paths(get_option('prefix'), get_option('localedir')))
configure_file(output: 'config.h', configuration: conf)
config_h_dir = include_directories('.')


add_project_arguments(
    '-include', 'config.h',
    '-DGETTEXT_PACKAGE="@0@"'.format(meson.project_name()),
    language: 'c'
)

# Let's define our executable
executable(
    meson.project_name(),
    sources,
    asresources,
    dependencies: dependencies,
    vala_args: [meson.source_root() + '/src/Config.vapi'],
    install : true
)

subdir('data')
subdir('po')

meson.add_install_script('build-aux/post_install.py', join_paths(get_option('prefix'), get_option('datadir')))
