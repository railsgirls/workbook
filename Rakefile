require 'rubygems'
require 'doc_raptor'

namespace :build do
  task :pdf do
    DocRaptor.api_key(ENV['DOCRAPTOR_API_KEY'])

    mkdir_p 'build'

    options = {
      document_content: File.read('print.html'),
      name: 'workbook.pdf',
      document_type: '..',
      test: true
    }

    DocRaptor.create(options) do |pdf, response|
      if response.success?
        File.open('build/workbook.pdf', 'w+b') do |file|
          file.write(pdf)
        end

        puts "Successfully generated the PDF!"
        exit 0
      else
        puts "Failed to generate PDF: "
        response['errors']['error'].each do |error|
          puts " * #{error}"
        end
        exit 1
      end
    end
  end
end

task default: 'build:pdf'