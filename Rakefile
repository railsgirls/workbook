require 'rubygems'
require 'doc_raptor'
require 'nokogiri'

namespace :build do
  task :setup do
    mkdir_p 'build'
  end

  task html: [:setup] do
    file = File.read('source/workbook.html')

    document = Nokogiri::HTML(file)

    # Embed stylesheets
    document.css('link[rel=stylesheet]').each do |link|
      style = Nokogiri::XML::Node.new('style', document)
      style.content = File.read("source/#{link['href']}")
      link.replace(style)
    end

    number_of_pages = document.css('.page').count
    if (number_of_pages % 4) > 0
      puts "Number of pages must be a multiple of 4 for printing, but is #{number_of_pages}!"
      exit 1
    end

    File.open('build/workbook.html', 'w') do |file|
      file.write(document.to_html)
    end

    puts "Successfully generated the HTML!"
  end

  task pdf: [:html] do
    DocRaptor.api_key(ENV['DOCRAPTOR_API_KEY'])

    options = {
      document_content: File.read('build/workbook.html'),
      name: 'workbook.pdf',
      document_type: 'pdf',
      test: true
    }

    DocRaptor.create(options) do |pdf, response|
      if response.success?
        File.open('build/workbook.pdf', 'w+b') { |file| file.write(pdf.read) }

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