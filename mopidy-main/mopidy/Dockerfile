ARG BUILD_FROM

FROM $BUILD_FROM AS build-base

RUN apk add --no-cache \
      python3=3.10.14-r1 \
      py3-gst=1.20.3-r0 \
      py3-pip=22.1.1-r0 \
      mopidy=3.3.0-r0 \
      && ln -s /usr/lib/libpython3.10.so.1.0 /usr/lib/libpython3.10.so


FROM build-base AS mopidy-iris

RUN pip3 install --no-cache-dir --user Mopidy-Iris==3.64.1 &&\
      ls -al /root/.local/lib/python3.10/site-packages


FROM build-base AS mopidy-mpd

RUN pip3 install --no-cache-dir --user Mopidy-MPD==3.3.0 &&\
      ls -al /root/.local/lib/python3.10/site-packages
      


FROM build-base AS mopidy-radionet

RUN pip3 install --no-cache-dir --user Mopidy-RadioNet==0.2.2 &&\
      ls -al /root/.local/lib/python3.10/site-packages

      

FROM build-base AS mopidy-spotify

RUN pip3 install --no-cache-dir --user Mopidy-Spotify==4.0.1
       ls -al /root/.local/lib/python3.10/site-packages

       

FROM build-base AS mopidy-youtube

RUN pip3 install --no-cache-dir --user Mopidy-YouTube==3.1.0
       ls -al /root/.local/lib/python3.10/site-packages



FROM $BUILD_FROM AS run

RUN apk add --no-cache \
      python3=3.10.14-r1 \
      py3-gst=1.20.3-r0 \
      mopidy=3.3.0-r0 \
      ca-certificates=20240226-r0 \
      sudo=1.9.12-r1 \
      && ln -s /usr/lib/libpython3.10.so.1.0 /usr/lib/libpython3.10.so

COPY --from=mopidy-iris /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-mpd /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-radionet /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-spotify /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/
COPY --from=mopidy-youtube /root/.local/lib/python3.10/site-packages /usr/lib/python3.10/site-packages/


RUN echo "root ALL=NOPASSWD: /usr/lib/python3.10/site-packages/mopidy_iris/system.sh" >> /etc/sudoers

ENTRYPOINT [ "/init" ]
CMD []
COPY root /
