---
title: jekyll serve 실행시 webrick (LoadError) 발생 해결
category: information
tags:
- gitblog
---

깃블로그 서버를 실행하려고 jekyll serve를 실행시 다음과 같은 오류 스택이 뜨며 서버가 정상실행이 안되는 경우가 있다.

```
C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `require': cannot load such file -- webrick (LoadError)
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve/servlet.rb:3:in `<top (required)>'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:179:in `require_relative'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:179:in `setup'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:100:in `process'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `block in process_with_graceful_fail'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `each'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/command.rb:91:in `process_with_graceful_fail'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/lib/jekyll/commands/serve.rb:86:in `block (2 levels) in init_with_program'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `block in execute'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `each'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/command.rb:221:in `execute'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary/program.rb:44:in `go'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/mercenary-0.4.0/lib/mercenary.rb:21:in `program'
        from C:/Ruby30-x64/lib/ruby/gems/3.0.0/gems/jekyll-4.2.0/exe/jekyll:15:in `<top (required)>'
        from C:/Ruby30-x64/bin/jekyll:23:in `load'
        from C:/Ruby30-x64/bin/jekyll:23:in `<main>'
```

해당 문제가 일어나는 경우는 Ruby 3.0.0 이상을 사용할 때 일어난다.
이유는 webrick 이 Ruby 3.x 이상에선 Bundle 로 제공되지 않기 때문이다.
그러므로 이에 대한 해결책은 해당 번들을 설치해주는 것이다.

터미널에서 다음 명령을 실행해주자
```
bundle add webrick
```

jekyll 의 이슈에서 다뤄졌던 내용 : <a href="https://github.com/jekyll/jekyll/issues/8523">link</a>
