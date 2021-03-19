FROM ubuntu:latest
VOLUME ["/myvolume"]
WORKDIR /app
ADD . /app
RUN apt update && apt install python -y
RUN echo "Hello Docker" > /tmp/README
CMD python /app/main.py
ENV user ajesrani
LABEL color=red
