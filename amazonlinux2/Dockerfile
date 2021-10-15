# global args
ARG __BUILD_DIR__="/build"
ARG OS_VERSION="2.0.20211001"



FROM amazonlinux:2 as first_stage

ARG __BUILD_DIR__
ARG OS_VERSION
ARG OS_CODENAME="amazon-linux-2"
ARG __USER__="root"
ARG __WORK_DIR__="/root"

ENV \
  LANG="POSIX" \
  LC_ALL="POSIX"

USER ${__USER__}

COPY "files/" "${__WORK_DIR__}/"

WORKDIR "${__WORK_DIR__}"

RUN \
# build env
    echo '--> setting build env' && \
    set +h && \
# import rpm key
    echo '--> import rpm keys' && \
    rpm --import "/etc/pki/rpm-gpg/RPM-GPG-KEY-${OS_CODENAME}" && \
# dependencies
    echo '--> instaling dependencies' && \
    yum --assumeyes --quiet install \
        yum-utils && \
# build structure
    echo '--> creating build structure' && \
    install --directory --owner="${__USER__}" --group="${__USER__}" --mode=0755 "${__BUILD_DIR__}" && \
# build first stage
    echo '--> building first stage' && \
    yumdownloader --assumeyes --quiet --arch="$(arch)" --destdir="${__WORK_DIR__}" system-release && \
    rpm --root "${__BUILD_DIR__}" --initdb && \
    rpm --root "${__BUILD_DIR__}" --import "/etc/pki/rpm-gpg/RPM-GPG-KEY-${OS_CODENAME}" && \
    rpm --root "${__BUILD_DIR__}" --install --nodeps --noscripts "${__WORK_DIR__}"/system-release*.rpm && \
    yum --assumeyes --quiet --disablerepo='*' --enablerepo='amzn2-core' --installroot="${__BUILD_DIR__}" --setopt=tsflags='nodocs' --setopt=override_install_langs="en_US.utf8" install \
        bash \
        coreutils \
        yum \
        yum-plugin-ovl \
        yum-plugin-priorities && \
# clean up and tweaks...
    echo '--> system settings and tweaks' && \
    sed -i -e '/distroverpkg=system-release/a exclude=*.i?86\noverride_install_langs=en_US.utf8\ntsflags=nodocs' "${__BUILD_DIR__}/etc/yum.conf" && \
    rm -rf "${__BUILD_DIR__}/usr/share/info"/* && \
    rm -rf "${__BUILD_DIR__}/usr/share/man"/* && \
    rm -rf "${__BUILD_DIR__}/var/cache/yum"/* && \
    rm -rf "${__BUILD_DIR__}/var/log"/* && \
    rm -rf "${__BUILD_DIR__}/sbin/sln" && \
    rm -rf "${__BUILD_DIR__}/dev"/.??* && \
    rm -rf "${__BUILD_DIR__}/home"/.??* && \
    rm -rf "${__BUILD_DIR__}/root"/.??* && \
    rm -rf "${__BUILD_DIR__}/tmp"/.??* && \
    rm -rf "${__BUILD_DIR__}/boot" && \
    rm -rf "${__BUILD_DIR__}/proc" && \
    rm -rf "${__BUILD_DIR__}/sys" && \
    find "${__BUILD_DIR__}/usr/share/doc" -mindepth 1 -not -type d -not \( -name 'COPYING*' -o -name 'GPL' -o -name '*LICENSE*' \) -delete && \
    find "${__BUILD_DIR__}/usr/share/doc" -mindepth 1 -type d -empty -delete && \
    find "${__BUILD_DIR__}/var/cache" -type f -delete && \
# copy tests
    echo '--> copying test files' && \
    install --owner="${__USER__}" --group="${__USER__}" --mode=0755 --target-directory="${__BUILD_DIR__}/usr/bin" "${__WORK_DIR__}/tests"/* && \
# check version
    echo '--> os version' && \
    cat "${__BUILD_DIR__}/etc/system-release"* && \
    rpm --root "${__BUILD_DIR__}" --query --queryformat='%{VERSION}\n' system-release && \
# done
  echo '--> all done!'



FROM scratch

ARG __BUILD_DIR__
ARG OS_VERSION

LABEL \
  maintainer="Frederico Martins <https://hub.docker.com/u/fscm/>" \
  vendor="fscm" \
  cmd="docker container run --interactive --rm --tty fscm/amazonlinux" \
  params="" \
  org.label-schema.schema-version="1.0" \
  org.label-schema.name="fscm/amazonlinux" \
  org.label-schema.description="A small Amazon Linux base image" \
  org.label-schema.url="https://aws.amazon.com/amazon-linux-2/" \
  org.label-schema.vcs-url="https://github.com/fscm/docker-amazonlinux/" \
  org.label-schema.vendor="fscm" \
  org.label-schema.version=$OS_VERSION \
  org.label-schema.docker.cmd="docker container run --interactive --rm --tty fscm/amazonlinux" \
  org.label-schema.docker.cmd.test="docker container run --interactive --rm --tty fscm/amazonlinux in_sanity" \
  org.label-schema.docker.params=""

COPY --from=first_stage "${__BUILD_DIR__}" "/"

ENV \
  LANG="POSIX" \
  LC_ALL="POSIX"

CMD ["/bin/bash"]
