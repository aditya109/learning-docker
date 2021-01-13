# Assignment: Bind Mounts

- [ ] Use Jekyll `static site generator` to start a local web server.
- [ ] Don't have to be web developer: this is example of bridging the gap between local file access and apps running in containers.
- [ ] Source is in this directory under name `assignment bind mount`.
- [ ] We edit files with editor on our host using native tools.
- [ ] Container detects changes with host files and updates web server.
- [ ] Start container with `docker run -p 80:4000 -v ${pwd}:/site/bretfisher/jekyll-serve`.

```powershell
Aditya :: assignment bind mount » docker container run -p 81:4000 -v ${pwd}:/site bretfisher/jekyll-serve
Unable to find image 'bretfisher/jekyll-serve:latest' locally
latest: Pulling from bretfisher/jekyll-serve
df20fa9351a1: Pull complete
b79bab524d4c: Pull complete
8f5dd72031b5: Pull complete
87774b8e0425: Pull complete
445c0e8670ac: Pull complete
cf940378a78b: Pull complete
083e5a96fb9b: Pull complete
18f0e9b7da82: Pull complete
172d0a51f312: Pull complete
Digest: sha256:701b6c4778cf85987b3693f7c6b4bf9b950f9973e6890a3e5ae6f5a7fde07f3a
Status: Downloaded newer image for bretfisher/jekyll-serve:latest
The dependency tzinfo (~> 1.2) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x64-mingw32, x86-mswin32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x64-mingw32 x86-mswin32 java`.
The dependency tzinfo-data (>= 0) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x64-mingw32, x86-mswin32, java. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x64-mingw32 x86-mswin32 java`.
The dependency wdm (~> 0.1.1) will be unused by any of the platforms Bundler is installing for. Bundler is installing for ruby but the dependency is only for x86-mingw32, x64-mingw32, x86-mswin32. To add those platforms to the bundle, run `bundle lock --add-platform x86-mingw32 x64-mingw32 x86-mswin32`.
Fetching gem metadata from https://rubygems.org/..........
Fetching gem metadata from https://rubygems.org/.
Resolving dependencies...
Using bundler 2.1.4
Using colorator 1.1.0
Using eventmachine 1.2.7
Using http_parser.rb 0.6.0
Using forwardable-extended 2.6.0
Using rb-fsevent 0.10.4
Using liquid 4.0.3
Using mercenary 0.4.0
Using safe_yaml 1.0.5
Using unicode-display_width 1.7.0
Using pathutil 0.16.2
Fetching rexml 3.2.4
Fetching public_suffix 4.0.6
Fetching rouge 3.26.0
Fetching ffi 1.14.2
Fetching concurrent-ruby 1.1.7
Using terminal-table 1.8.0
Fetching em-websocket 0.5.2
Installing em-websocket 0.5.2
Installing rexml 3.2.4
Installing public_suffix 4.0.6
Installing concurrent-ruby 1.1.7
Installing rouge 3.26.0
Using addressable 2.7.0
Fetching kramdown 2.3.0
Installing ffi 1.14.2 with native extensions
Installing kramdown 2.3.0
Fetching i18n 1.8.7
Installing i18n 1.8.7
Using kramdown-parser-gfm 1.1.0
Using sassc 2.4.0
Using rb-inotify 0.10.1
Using jekyll-sass-converter 2.1.0
Fetching listen 3.4.0
Installing listen 3.4.0
Using jekyll-watch 2.2.1
Fetching jekyll 4.1.1
Installing jekyll 4.1.1
Fetching jekyll-seo-tag 2.7.1
Fetching jekyll-feed 0.15.1
Installing jekyll-feed 0.15.1
Installing jekyll-seo-tag 2.7.1
Fetching minima 2.5.1
Installing minima 2.5.1
Bundle complete! 6 Gemfile dependencies, 31 gems now installed.
Use `bundle info [gemname]` to see where a bundled gem is installed.
Post-install message from i18n:

HEADS UP! i18n 1.1 changed fallbacks to exclude default locale.
But that may break your application.

If you are upgrading your Rails application from an older version of Rails:

Please check your Rails app for 'config.i18n.fallbacks = true'.
If you're using I18n (>= 1.1.0) and Rails (< 5.2.2), this should be
'config.i18n.fallbacks = [I18n.default_locale]'.
If not, fallbacks will be broken in your app by I18n 1.1.x.

If you are starting a NEW Rails application, you can ignore this notice.

For more info see:
https://github.com/svenfuchs/i18n/releases/tag/v1.1.0

Configuration file: /site/_config.yml
            Source: /site
       Destination: /site/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.998 seconds.
                    Auto-regeneration may not work on some Windows versions.
                    Please see: https://github.com/Microsoft/BashOnWindows/issues/216
                    If it does not work, please upgrade Bash on Windows or run Jekyll with --no-watch. Auto-regeneration: enabled for '/site'
    Server address: http://0.0.0.0:4000/
  Server running... press ctrl-c to stop.
[2021-01-13 15:19:32] ERROR `/favicon.ico' not found.
```

