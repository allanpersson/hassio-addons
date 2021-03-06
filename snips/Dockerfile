ARG BUILD_FROM
FROM $BUILD_FROM

# Copy data
COPY run.sh /
COPY mosquitto.conf /etc/
COPY customtts.sh /usr/bin
COPY snips-entrypoint.sh /

ARG BUILD_ARCH

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        dirmngr \
        apt-utils \
        apt-transport-https \
        unzip \
        supervisor \
        mpg123 \
    && rm -rf /var/lib/apt/lists/* \
    && if [ "$BUILD_ARCH" = "amd64" ]; \
        then \
            bash -c 'echo "deb https://debian.snips.ai/stretch stable main" > /etc/apt/sources.list.d/snips.list' \
            && apt-key adv --keyserver pgp.surfnet.nl --recv-keys F727C778CCB0A455; \
        else \
            bash -c 'echo "deb https://raspbian.snips.ai/stretch stable main" > /etc/apt/sources.list.d/snips.list' \
            && apt-key adv --keyserver pgp.surfnet.nl --recv-keys D4F50CDCA10A2849; \
        fi

ARG SNIPS_VERSION
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        snips-platform-voice=${SNIPS_VERSION} \
        snips-asr=${SNIPS_VERSION} \
        snips-injection=${SNIPS_VERSION} \
        snips-watch=${SNIPS_VERSION} \
        mosquitto \
        snips-audio-server=${SNIPS_VERSION} \
        snips-dialogue=${SNIPS_VERSION} \
        snips-hotword=${SNIPS_VERSION} \
        snips-nlu=${SNIPS_VERSION} \
        snips-tts=${SNIPS_VERSION} \
    && rm -rf /var/lib/apt/lists/* \
    && curl -L -o /assistant_Hass_de.zip https://s3.amazonaws.com/hassio-addons-data/assistant_Hass_de.zip \
    && curl -L -o /assistant_Hass_en.zip https://s3.amazonaws.com/hassio-addons-data/assistant_Hass_en.zip \
    && curl -L -o /assistant_Hass_fr.zip https://s3.amazonaws.com/hassio-addons-data/assistant_Hass_fr.zip

ENTRYPOINT [ "/run.sh" ]
