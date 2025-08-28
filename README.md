# file_organizer.rb
require 'fileutils'

class FileOrganizer
  FILE_TYPES = {
    "Images" => [".jpg", ".jpeg", ".png", ".gif", ".bmp"],
    "Documents" => [".pdf", ".doc", ".docx", ".txt", ".md", ".odt"],
    "Spreadsheets" => [".xls", ".xlsx", ".csv", ".ods"],
    "Presentations" => [".ppt", ".pptx", ".odp"],
    "Music" => [".mp3", ".wav", ".flac", ".aac"],
    "Videos" => [".mp4", ".mkv", ".avi", ".mov"],
    "Archives" => [".zip", ".rar", ".7z", ".tar", ".gz"],
    "Scripts" => [".rb", ".py", ".js", ".sh", ".php"],
    "Others" => [] # catch-all
  }

  def initialize(directory)
    @directory = directory
  end

  def organize
    Dir.foreach(@directory) do |file|
      next if File.directory?(file)

      ext = File.extname(file).downcase
      folder = FILE_TYPES.find { |_, exts| exts.include?(ext) }&.first || "Others"

      target_dir = File.join(@directory, folder)
      FileUtils.mkdir_p(target_dir)

      FileUtils.mv(File.join(@directory, file), target_dir)
      puts "Moved #{file} → #{folder}/"
    end
    puts "\n✅ Organization complete!"
  end
end

# CLI
print "Enter directory path to organize: "
path = gets.chomp

if Dir.exist?(path)
  organizer = FileOrganizer.new(path)
  organizer.organize
else
  puts "❌ Directory does not exist!"
end
