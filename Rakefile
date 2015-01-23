require 'rubygems'
require 'doc_raptor'
require 'nokogiri'
require 'mimemagic'

namespace :build do
  task :compile do
    sh 'middleman build'
  end

  task html: [:compile] do
    file = File.read('build/index.html')

    document = Nokogiri::HTML(file)

    # Check for the correct number of pages
    number_of_pages = document.css('.page').count
    if (number_of_pages % 4) > 0
      puts "Number of pages must be a multiple of 4 for printing, but is #{number_of_pages}!"
      exit 1
    end

    # Embed stylesheets
    document.css('link[rel=stylesheet]').each do |link|
      style = Nokogiri::XML::Node.new('style', document)
      style.content = File.read("build/#{link['href']}")
      link.replace(style)
    end

    # Embed images
    document.css('img').each do |image|
      path = "build/#{image['src']}"

      content = File.read(path)
      content_type = MimeMagic.by_path(path)

      data = [content].flatten.pack('m').gsub("\n","")

      image['src'] = "data:#{content_type};base64,#{data}"
    end

    File.open('build/index.html', 'w') do |file|
      file.write(document.to_html)
    end

    puts "Successfully generated the HTML!"
  end

  task pdf: [:html] do
    DocRaptor.api_key(ENV['DOCRAPTOR_API_KEY'])

    options = {
      document_content: File.read('build/index.html'),
      name: 'workbook.pdf',
      document_type: 'pdf',
      test: true,
      prince_options: {
        css_dpi: 72
      }
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