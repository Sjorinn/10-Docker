#docker build -t ft-rails .
#docker build -t ex02 -f app/Dockerfile .
#docker run -p 3000:3000 -it --rm ex02

FROM ruby:2.5

#installation requise dans le container pour l’application
RUN apt-get update && apt-get install -y --no-install-recommends apt-utils wget nodejs build-essential libpq-dev gems
RUN apt-get install -y bundler rake

RUN	gem install rails 


# - create app directory
RUN mkdir /opt/app

ONBUILD WORKDIR /opt/app
ONBUILD COPY app /opt/app

ONBUILD EXPOSE 3000

# - bunde install = install dependances
# - rake db:migrate = create tables in db
# - rake db:seed = fill db
ONBUILD RUN bundle install &&\
	rake db:migrate && \
	rake db:seed