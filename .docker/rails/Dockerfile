FROM ruby:2.7

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor -o /usr/share/keyrings/yarn-archive-keyring.gpg \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/yarn-archive-keyring.gpg] https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list > /dev/null
RUN apt-get update && apt-get install -y nodejs yarn

RUN mkdir /app
WORKDIR /app

COPY ../../Gemfile Gemfile
COPY ../../Gemfile.lock Gemfile.lock

RUN gem update --system
RUN bundle install
RUN bundle exec rails webpacker:compile

EXPOSE 3000