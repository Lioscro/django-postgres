FROM phusion/passenger-full:1.0.9

LABEL maintainer="Joseph Min <phoenixter96@gmail.com>"

ENV HOME /root
WORKDIR /home/app/app
ENV PYTHONUNBUFFERED 1

# Copy files and install dependencies
COPY --chown=app:app . .
RUN python3 bin/get-pip.py
RUN pip install -r requirements.txt

RUN rm /etc/nginx/sites-enabled/default
COPY webapp.conf /etc/nginx/sites-enabled/webapp.conf
RUN rm -f /etc/service/nginx/down
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

CMD ["make", "deploy"]
