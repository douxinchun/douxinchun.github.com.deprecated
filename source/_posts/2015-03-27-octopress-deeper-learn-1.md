---
layout: post
title: "Octopress学习旅程(1)"
date: 2015-03-27 15:08:02 +0800
comments: true
categories: octopress
---   

主要用来记录学习Octopress中的一些知识点.  

先记录一个命令,rake -T 可以查看命令rake的介绍:  
{% codeblock octopress-bash %}
localhost:octopress douxinchun$ rake -T
rake clean                     # Clean out caches: .pygments-cache, .gist-cache, .sass-cache
rake copydot[source,dest]      # copy dot files for deployment
rake deploy                    # Default deploy task
rake gen_deploy                # Generate website and deploy
rake generate                  # Generate jekyll site
rake install[theme]            # Initial setup for Octopress: copies the default theme into the path of Jekyll's generator
rake integrate                 # Move all stashed posts back into the posts directory, ready for site generation
rake isolate[filename]         # Move all other posts than the one currently being worked on to a temporary stash location (stash) so regenerat...
rake list                      # list tasks
rake new_page[filename]        # Create a new page in source/(filename)/index.md
rake new_post[title]           # Begin a new post in source/_posts
rake preview                   # preview the site in a web browser
rake push                      # deploy public directory to github pages
rake rsync                     # Deploy website via rsync
rake set_root_dir[dir]         # Update configurations to support publishing to root or sub directory
rake setup_github_pages[repo]  # Set up _deploy folder and deploy branch for Github Pages deployment
rake update_source[theme]      # Move source to source.old, install source theme updates, replace source/_includes/navigation.html with source....
rake update_style[theme]       # Move sass to sass.old, install sass theme updates, replace sass/custom with sass.old/custom
rake watch                     # Watch the site and regenerate when it changes
{% endcodeblock %}


追加一个介绍的很详细的Octopress使用者的总结,内容很详细,留作备忘  
http://shengmingzhiqing.com/blog/octopress-tutorials-toc.html/