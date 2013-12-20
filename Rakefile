#!/usr/bin/env rake

require 'fileutils'
require 'yaml'
require 'RMagick'
require 'chunky_png/rmagick'
require 'psd'

namespace :asset do

  desc 'Generate assets from Assetfile'
  task :generate do

    ASSETS = YAML.load_file('Assetfile')
    
    ASSETS.each do |asset, asset_config|
      puts "In #{asset} ..."
      asset_config.each do |mode, option|
        case mode.downcase
        when 'export'

          # Generate a image
          psd = PSD.new(asset)
          psd.parse!
          base_image = ChunkyPNG::RMagick.export(psd.image.to_png)
        
          option.each do |name, size|
            puts "  export #{name} with #{size} ..."
            # Prepare the destination folder
            path = File.join 'src/images', name
            FileUtils.mkdir_p(File.dirname(path)) unless File.directory?(File.dirname(path))

            # Resizing image
            image = base_image.dup
            image = image.resize *size.split('x').map(&:to_i)
            image.write path
          end

        when 'slice'

          puts "  slice #{asset} ..."

          # Prepare psd
          puts "    parsing psd ..."
          psd = PSD.new(asset)
          psd.parse!
          psd.tree.children.each do |node|
          
          file = node.name.scan(/\A([\w\-@]+?\..+?)\Z/).first
          if file
              puts "    save #{node.name} ..."
              # Prepare the destination folder
              path = File.join 'src/images', file
              FileUtils.mkdir_p(File.dirname(path)) unless File.directory?(File.dirname(path))

              node.save_as_png path
            end
          end

        end
      end
    end

    puts "All done!"
    
  end
end