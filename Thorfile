#!/usr/bin/env ruby
# -*- encoding: utf-8 -*-

require 'uri'
require 'nokogiri'
require 'open-uri'
require 'pathname'
require 'thor'

script_path = Pathname.new(__FILE__).dirname

class NaverTasks < Thor
  desc "get_image", "get image file from web page"
  option :url, :type => :string, :required => true
  option :name, :type => :string, :default => ''
  option :output, :default => '.'
  def get_image()
    url = options['url'].gsub(/\?.+$/, '')

    # htmlを取得
    html = Nokogiri::HTML(open(url).read, url)

    # 保存名を取得
    if options['name'] == '' then
      name = File.basename(url, '.*')
    else
      name = options['name']
    end

    # 全ページ数を取得
    page_length = html.css('div[data-na="NA:pager"] a').last.text.to_i
    p page_length

    # 保存先のフォルダを作成(ページ名)
    output_dir = Pathname(options['output']) + name
    FileUtils.mkdir_p(output_dir)

    # 画像インデックス
    image_index = 1

    1.upto(page_length) do |page_index|
      list_page_url = url + '?page=' + page_index.to_s
      p list_page_url

      html = Nokogiri::HTML(open(list_page_url).read, list_page_url)
      html.css('div.LyMain a[data-na="NL:image_end"]').each do |node|
        # 画像URLを取得
        image_page_url = node.attribute('href').value

        # htmlを取得
        image_html = Nokogiri::HTML(open(image_page_url).read, image_page_url)
        large_image_url = image_html.css('div.LyMain a[target="_blank"]')[0].attribute('href').value
        p large_image_url

        image_file_name = name + '_' + image_index.to_s.rjust(3, '0') + File.extname(large_image_url)
        image_file_path = output_dir + image_file_name

        # 画像を保存
        begin
          open(image_file_path.to_s, 'wb') do |saved_file|
            open(large_image_url, 'rb') do |read_file|
              saved_file.write(read_file.read)
            end
          end
          image_index += 1
        rescue Exception

        end
      end
    end
  end
end

NaverTasks.start(ARGV)
