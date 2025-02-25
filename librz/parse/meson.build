parse_plugins = [
  '6502_pseudo',
  'arm_pseudo',
  'att2intel',
  'avr_pseudo',
  'chip8_pseudo',
  'tms320_pseudo',
  'dalvik_pseudo',
  'm68k_pseudo',
  'mips_pseudo',
  'ppc_pseudo',
  'v850_pseudo',
  'wasm_pseudo',
  'x86_pseudo',
]

if get_option('use_gpl')
  parse_plugins += [
    'sh_pseudo',
    'riscv_pseudo',
    'z80_pseudo'
  ]
endif

rz_parse_sources = [
  'filter.c',
  'parse.c',
  'p/parse_6502_pseudo.c',
  'p/parse_arm_pseudo.c',
  'p/parse_att2intel.c',
  'p/parse_avr_pseudo.c',
  'p/parse_chip8_pseudo.c',
  'p/parse_dalvik_pseudo.c',
  'p/parse_m68k_pseudo.c',
  'p/parse_mips_pseudo.c',
  'p/parse_ppc_pseudo.c',
  'p/parse_sh_pseudo.c',
  'p/parse_tms320_pseudo.c',
  'p/parse_v850_pseudo.c',
  'p/parse_wasm_pseudo.c',
  'p/parse_x86_pseudo.c',
  'p/parse_riscv_pseudo.c',
  'p/parse_z80_pseudo.c'
]

rz_parse = library('rz_parse', rz_parse_sources,
  include_directories: platform_inc,
  c_args: library_cflags,
  dependencies: [
    rz_util_dep,
    rz_flag_dep,
    rz_syscall_dep,
    rz_reg_dep,
    rz_cons_dep,
  ],
  install: true,
  implicit_include_directories: false,
  install_rpath: rpath_lib,
  link_args: library_linkflags,
  soversion: rizin_libversion,
  name_suffix: lib_name_suffix,
  name_prefix: lib_name_prefix,
)

rz_parse_dep = declare_dependency(link_with: rz_parse,
                                 include_directories: platform_inc)
meson.override_dependency('rz_parse', rz_parse_dep)

pkgconfig_mod.generate(rz_parse,
  subdirs: 'librz',
  version: rizin_version,
  name: 'rz_parse',
  filebase: 'rz_parse',
  requires: [
    'rz_util',
    'rz_flag',
    'rz_cons',
    'rz_syscall',
    'rz_reg'
  ],
  description: 'rizin foundation libraries',
  variables: [
    'plugindir=@0@'.format(rizin_plugins),
    'datdir=@0@'.format(rizin_datdir_rz),
  ],
)

if not is_static_libs_only
  conf = configuration_data()
  conf.set('RZ_VERSION', rizin_version)
  conf.set('RIZIN_MODULE', rz_parse.name())
  conf.set('RIZIN_MODULE_DEPS', ' '.join(['rz_util', 'rz_flag', 'rz_syscall', 'rz_reg', 'rz_cons']))
  conf.set('PACKAGE_RELATIVE_PATH', cmake_package_relative_path)
  conf.set('INSTALL_INCDIR', rizin_incdir)
  conf.set('INSTALL_LIBDIR', rizin_libdir)
  conf.set('INSTALL_PLUGDIR', rizin_plugins)
  conf.set('rizin_libname', rz_parse.name())
  cmake_mod.configure_package_config_file(
    name: conf.get('rizin_libname'),
    input: '../RzModulesConfig.cmake.in',
    install_dir: rizin_cmakedir / conf.get('rizin_libname'),
    configuration: conf,
  )
endif
