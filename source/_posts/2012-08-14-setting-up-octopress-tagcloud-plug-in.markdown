---
layout: post
title: "Setting Up octopress-tagcloud Plug-in"
date: 2012-08-14 23:54
comments: true
categories: Blog
tags: octopress plugin tag_cloud tag ruby
---
`Problem: Setting Up octopress-tagcloloud Plug-in`

`Status: Solved`

`Comment: Clever Solution`

Details:

[octopress-tagcloud](https://github.com/tokkonopapa/octopress-tagcloud) is 3rd party plug-in which list in [octopress github wiki](https://github.com/imathis/octopress/wiki/3rd-party-plugins). I needed kinds of plug-in to provide quick tag access point for my readers. So I started setting up.

I was able to get all required files in <https://github.com/tokkonopapa/octopress-tagcloud>.

But ReadMe file in this repository was not kind. This files has below instructions.

1. Place below files in your octopress folder from tagcloud git repository.
 
    - plugins/tag_cloud.rb
    - source/_includes/custom/asides/tag_cloud.html
    - source/_includes/custom/asides/category_list.html
 			
2. Change `default_asides` array in `_config.yml`.
 
In your octopress folder, you can find `_config.yml`. It contains below line in middle of something.

{% codeblock lang:yml %}
# list each of the sidebar modules you want to include, in the order you want them to appear.
# To add custom asides, create files in /source/_includes/custom/asides/ and add them to the list like 'custom/asides/custom_aside_name.html'
default_asides: [asides/recent_posts.html, asides/github.html, asides/twitter.html, asides/delicious.html, asides/pinboard.html, asides/googleplus.html]
{% endcodeblock %}

Add `custom/asides/category_list.html` and `custom/asides/tag_cloud.html` inside [blah, blah]. It is list. So result was look like below:

{% codeblock lang:yml %}
default_asides: [asides/recent_posts.html, asides/github.html, asides/twitter.html, asides/delicious.html, asides/pinboard.html, asides/googleplus.html, custom/asides/category_list.html, custom/asides/tag_cloud.html]
{% endcodeblock %}

-------------------------

YAP!! I followed all instructions in ReadMe file. So I checked my updated blog. But there was problem. Why Category list has exactly same as Tags list?

I figured out that I didn't add any tags from posts. So I was about to add tags, but HOW?
I thought something is wrong with code. I needed to fix it from code level. Though I am pythonian, I have to delve into ruby code, `tag_cloud.rb`.

This is what I changed. I simply replaced `categories` to `tags`, and `category` to `tag`. Here is code. 
``` ruby TagCloud Class Which I edited https://github.com/tokkonopapa/octopress-tagcloud/blob/master/plugins/tag_cloud.rb See Original
  class TagCloud < Liquid::Tag

    def initialize(tag_name, markup, tokens)
      @opts = {}
      if markup.strip =~ /\s*counter:(\w+)/i
        @opts['counter'] = ($1 == 'true')
        markup = markup.strip.sub(/counter:\w+/i,'')
      end
      super
    end

    def render(context)
      lists = {}
      max, min = 1, 1
      config = context.registers[:site].config
      tag_dir = config['root'] + config['tag_dir'] + '/'
      tags = context.registers[:site].tags
      tags.keys.sort_by{ |str| str.downcase }.each do |tag|
        count = tags[tag].count
        lists[tag] = count
        max = count if count > max
      end

      html = ''
      lists.each do | tag, counter |
        url = tag_dir + tag.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase
        style = "font-size: #{100 + (60 * Float(counter)/max)}%"
        html << "<a href='#{url}' style='#{style}'>#{tag}"
        if @opts['counter']
          html << "(#{tags[tag].count})"
        end
        html << "</a> "
      end
      html
    end
  end
```
    
One more thing. You should add below line into `_config.yml`.

    tag_dir: blog/tags

So `_config.yml` contains portions that looks like this

{% codeblock lang:yml %}
# If publishing to a subdirectory as in http://site.com/project set 'root: /project'
root: /
permalink: /blog/:year/:month/:day/:title/
source: source
destination: public
plugins: plugins
code_dir: downloads/code
category_dir: blog/categories
tag_dir: blog/tags
markdown: rdiscount
pygments: false # default python pygments have been replaced by pygments.rb
{% endcodeblock %}

Finally tag list works well.
