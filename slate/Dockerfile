FROM ruby:2.5-alpine
EXPOSE 4567

RUN apk update \
  && apk add coreutils git make g++ nodejs

RUN git clone https://github.com/lord/slate /slate

RUN cd /slate && bundle install 

VOLUME /slate
VOLUME /slate/build

WORKDIR /slate
CMD bundle exec middleman server --watcher-force-polling
