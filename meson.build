project('SphinxDocumentation', 'c')

sphinx = find_program('sphinx-build', required : true)
pip = find_program('pip', required : true)
sphinx_autobuild = find_program('sphinx-autobuild', required : true)

source_dir = 'source'
build_dir = 'build'

# 这个目标是用来生成文档的，之前已经定义
custom_target('sphinx-html',
  output : ['dummy_output'], # Meson requires at least one output, but Sphinx handles its own output directory.
  command : [sphinx, '-b', 'html', source_dir, build_dir],
  build_by_default : true
)

# 安装依赖
run_target('install-dep',
  command : [pip, 'install', '-r', 'requirements.txt']
)

# 实时预览文档
run_target('preview',
  command : [sphinx_autobuild, source_dir, join_paths(build_dir, 'html')]
)

# 清理构建目录
run_target('clean',
  command : ['rm', '-rf', build_dir]
)

