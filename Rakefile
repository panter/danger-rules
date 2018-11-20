# frozen_string_literal: true

REQUIRE_PATTERN = /^require ["'].+?["']$/.freeze
FROZEN_STRING = '# frozen_string_literal: true'
ALL_DANGER_FILE = 'all/Dangerfile'

desc "Generate the complete #{ALL_DANGER_FILE}"
task 'generate:all' do
  contents = [
    header,
    lib_files_content
  ]

  danger_files.each do |file|
    contents << inlined_content(file)
  end

  write_content("rules/#{ALL_DANGER_FILE}", contents)
end

desc 'Generate every single */**/Dangerfile'
task 'generate:single' do
  danger_files.each do |file|
    contents = [
      header,
      lib_files_content,
      inlined_content(file)
    ]
    output_file = file.sub(%r{^src/}, 'rules/')
    write_content(output_file, contents)
  end
end

desc 'Documents all available rules in the README'
task 'generate:readme' do
  files = [ALL_DANGER_FILE, *danger_files].map do |file|
    file
      .sub(%r{^src/}, '')
      .sub(%r{/Dangerfile$}, '')
      .gsub('_', '\_')
      .yield_self { |f| "rules/#{f}" }
  end

  rules_list = files.map { |file| "    - #{file}\n" }.join

  readme = read_content('README.md')
  readme.gsub!(
    %r{(<!-- rules -->\n).+?( +<!-- /rules -->)}m,
    "\\1#{rules_list}\\2"
  )

  File.write('README.md', readme)
end

desc 'Generate all Dangerfiles and update the rules description in the README'
task generate: %i[generate:all generate:single generate:readme]

def files(*patterns)
  Dir[*patterns].sort
end

def danger_files
  files('src/*/**/Dangerfile')
end

def lib_files
  files('lib/*.rb')
end

def read_content(file)
  File.open(file, 'r:UTF-8', &:read)
end

def write_content(file, contents)
  directory = File.dirname(file)
  FileUtils.mkdir_p(directory)

  File.write(file, "#{contents.join.strip}\n")
end

def clean_content(content)
  content
    .gsub(FROZEN_STRING, '')
    .gsub(REQUIRE_PATTERN, '')
    .gsub(/\n\n+/, '')
end

def header
  <<~HEADER
    #{FROZEN_STRING}

    # THIS IS AN AUTOGENERATED FILE. DO NOT EDIT THIS FILE DIRECTLY.
    # RUN `rake generate` TO REGENERATE.

  HEADER
end

def included_file_header(file, content)
  "# --> #{file}:\n#{content}\n"
end

def inlined_content(file)
  content = read_content(file)

  content = clean_content(content)

  included_file_header(file, content)
end

def lib_files_content
  lib_files.map { |file| inlined_content(file) }
end
