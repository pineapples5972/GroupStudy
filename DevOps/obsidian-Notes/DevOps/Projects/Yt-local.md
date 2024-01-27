original project link - https://git.sr.ht/~heckyel/yt-local

What is Yt-local?
Its an bloatless frontend for youtube self hostable written in pyhon and flash framework uses very little footprint and allows tor routing for annonymous surfing of youtube.

==Original yoututbe platform is bloated and very slow to use on low end machines thats why this project came into existence==

Can be deployed using this docker image: 
https://hub.docker.com/r/rusian/yt-local

deployed on port 8080 by default

build image:
`docker build -t yt-local .`

Run with:
`docker run -d -p 8080:8080 yt-local`

access on:
http://localhost:8080

Here is the dockerfile extracted from docker hub
Dockerfile of rusian/yt-local image
``` .Dockerfile
ADD file:32ff5e7a78b890996ee4681cc0a26185d3e9acdb4eb1e2aaccb2411f922fed6b in /
CMD ["/bin/sh"]
ENV PATH=/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LANG=C.UTF-8
RUN /bin/sh -c set -eux; apk add --no-cache ca-certificates tzdata ; # buildkit
ENV GPG_KEY=A035C8C19219BA821ECEA86B64E628F8D684696D
ENV PYTHON_VERSION=3.11.5
RUN /bin/sh -c set -eux; apk add --no-cache --virtual .build-deps gnupg tar xz bluez-dev bzip2-dev dpkg-dev dpkg expat-dev findutils gcc gdbm-dev libc-dev libffi-dev libnsl-dev libtirpc-dev linux-headers make ncurses-dev openssl-dev pax-utils readline-dev sqlite-dev tcl-dev tk tk-dev util-linux-dev xz-dev zlib-dev ; wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz"; wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc"; GNUPGHOME="$(mktemp -d)"; export GNUPGHOME; gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys "$GPG_KEY"; gpg --batch --verify python.tar.xz.asc python.tar.xz; gpgconf --kill all; rm -rf "$GNUPGHOME" python.tar.xz.asc; mkdir -p /usr/src/python; tar --extract --directory /usr/src/python --strip-components=1 --file python.tar.xz; rm python.tar.xz; cd /usr/src/python; gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"; ./configure --build="$gnuArch" --enable-loadable-sqlite-extensions --enable-optimizations --enable-option-checking=fatal --enable-shared --with-lto --with-system-expat --without-ensurepip ; nproc="$(nproc)"; EXTRA_CFLAGS="-DTHREAD_STACK_SIZE=0x100000"; LDFLAGS="${LDFLAGS:--Wl},--strip-all"; make -j "$nproc" "EXTRA_CFLAGS=${EXTRA_CFLAGS:-}" "LDFLAGS=${LDFLAGS:-}" "PROFILE_TASK=${PROFILE_TASK:-}" ; rm python; make -j "$nproc" "EXTRA_CFLAGS=${EXTRA_CFLAGS:-}" "LDFLAGS=${LDFLAGS:--Wl},-rpath='\$\$ORIGIN/../lib'" "PROFILE_TASK=${PROFILE_TASK:-}" python ; make install; cd /; rm -rf /usr/src/python; find /usr/local -depth \( \( -type d -a \( -name test -o -name tests -o -name idle_test \) \) -o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name 'libpython*.a' \) \) \) -exec rm -rf '{}' + ; find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec scanelf --needed --nobanner --format '%n#p' '{}' ';' | tr ',' '\n' | sort -u | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }' | xargs -rt apk add --no-network --virtual .python-rundeps ; apk del --no-network .build-deps; python3 --version # buildkit
RUN /bin/sh -c set -eux; for src in idle3 pydoc3 python3 python3-config; do dst="$(echo "$src" | tr -d 3)"; [ -s "/usr/local/bin/$src" ]; [ ! -e "/usr/local/bin/$dst" ]; ln -svT "$src" "/usr/local/bin/$dst"; done # buildkit
ENV PYTHON_PIP_VERSION=23.2.1
ENV PYTHON_SETUPTOOLS_VERSION=65.5.1
ENV PYTHON_GET_PIP_URL=https://github.com/pypa/get-pip/raw/9af82b715db434abb94a0a6f3569f43e72157346/public/get-pip.py
ENV PYTHON_GET_PIP_SHA256=45a2bb8bf2bb5eff16fdd00faef6f29731831c7c59bd9fc2bf1f3bed511ff1fe
RUN /bin/sh -c set -eux; wget -O get-pip.py "$PYTHON_GET_PIP_URL"; echo "$PYTHON_GET_PIP_SHA256 *get-pip.py" | sha256sum -c -; export PYTHONDONTWRITEBYTECODE=1; python get-pip.py --disable-pip-version-check --no-cache-dir --no-compile "pip==$PYTHON_PIP_VERSION" "setuptools==$PYTHON_SETUPTOOLS_VERSION" ; rm -f get-pip.py; pip --version # buildkit
CMD ["python3"]
LABEL MAINTAINER=heckyel@riseup.net
WORKDIR /srv/app
/bin/sh -c apk update && apk upgrade && apk --no-cache add tor curl
COPY dir:d7661c06c9473530f6242b83b5770e00fa46bfce5741849e2bc70eb22428901a in /usr/local
COPY dir:a290e1fd7fa0aaaccb211fdf7fd46129a62505bd0c614d8be5b3c00bf0feca7b in /srv/app
EXPOSE 8080
COPY file:6fb9f1d07e6502dc9d298617f51dfcb76040e4d0e730af8964bfd28a74ef5b3a in /
COPY file:3b62ec1a5f2f2c7ccd80ce4d919e5135077c68e1736f63115a82b5c47310f711 in /
/bin/sh -c chmod u+x /entrypoint-tor.sh
/bin/sh -c chmod u+x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
```