# frozen_string_literal: true

ALL_DANGER_FILE = 'all/Dangerfile'

danger_files = Dir['*/**/Dangerfile'].reject { |file| file == ALL_DANGER_FILE }

# include all //Dangerfile
danger_files.each do |danger_file|
  danger_folder = File.dirname(danger_file)

  message "Using '#{danger_file}'"

  danger.import_dangerfile(path: danger_folder)
end
