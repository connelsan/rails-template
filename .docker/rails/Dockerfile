FROM ruby:2.7

RUN apt-get update && apt-get install -y npm

RUN npm install --global yarn
RUN yarn global add n
RUN n 12.22.5

RUN mkdir /app
WORKDIR /app

COPY ../../Gemfile Gemfile
COPY ../../Gemfile.lock Gemfile.lock

RUN gem update --system
RUN bundle install
RUN bundle exec rails webpacker:compile

EXPOSE 3000