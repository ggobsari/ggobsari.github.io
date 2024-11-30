# frozen_string_literal: true

source "https://rubygems.org"

# Jekyll 및 Chirpy 테마
gem "jekyll", "~> 4.3.0"
gem "jekyll-theme-chirpy"

# HTML 유효성 검사 (테스트 환경에서만 사용)
gem "html-proofer", "~> 5.0", group: :test

# Windows 환경 및 JRuby 지원
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:mingw, :x64_mingw, :mswin]

# Jekyll 서버 실행을 위한 webrick
group :development do
  gem "webrick", "~> 1.7"
end
