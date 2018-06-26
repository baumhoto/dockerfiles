# Container contains Ranger filemanager and Vim
# run with:
# docker run -it -v /home/$USER:/home/$USER -w /home/$USER --name ranger ranger
FROM alpine:latest
LABEL maintainer "baumhoto <baumhoto@gmail.com>"

RUN apk --no-cache add \
	ca-certificates \
	python3\
        less \
        file \
        sudo \
	vim 

RUN buildDeps=' \
		build-base \
		git \
	' \
	set -x \
	&& apk --no-cache add $buildDeps \
	&& git clone --depth 1 https://github.com/ranger/ranger /usr/src/ranger \
	&& cd /usr/src/ranger

# prevent segmentation fault: https://bugs.alpinelinux.org/issues/6839
COPY alpine_patch.diff /usr/src/ranger/

RUN patch /usr/src/ranger/ranger/gui/ui.py /usr/src/ranger/alpine_patch.diff \
	&& cd /usr/src/ranger \
	&& sudo make install \
	&& rm -rf /usr/src/ranger \
	&& apk del $buildDeps

CMD 'ranger'
