FROM ruby:2.5.1

ENV LANG C.UTF-8

RUN apt-get update -qq && \
 apt-get install -y --no-install-recommends \
  build-essential \
  libpq-dev \
  libfontconfig1 \
  less \
  vim && \
  rm -rf /var/lib/apt/lists/*

WORKDIR /app/ruby

ENV BUNDLE_PATH vendor/bundle

RUN bundle config --local build.nokogiri --use-system-libraries && \
    bundle config --local job 10
