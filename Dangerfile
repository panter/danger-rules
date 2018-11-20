# frozen_string_literal: true

# include all //Dangerfile
Dir['src/*/**/Dangerfile'].each do |danger_file|
  danger_folder = File.dirname(danger_file)

  message "Using '#{danger_file}'"

  danger.import_dangerfile(path: danger_folder)
end
