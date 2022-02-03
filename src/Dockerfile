# * Build Naviserver
ARG TCL_BUILD_IMAGE_TAG="${TCL_BUILD_IMAGE_TAG:-22.01}"
  FROM oupfiz5/tcl-build:${TCL_BUILD_IMAGE_TAG} as build

# ** Args
ARG NS_VERSION \
    NS_MODULES_VERSION \
    RL_JSON_VERSION \
    TCLLIB_VERSION \
    TCL_VERSION \
    TDOM_VERSION \
    THREAD_VERSION \
    XOTCL_VERSION

# ** Environments
ENV NS_VERSION="${NS_VERSION}" \
    NS_MODULES_VERSION="${NS_MODULES_VERSION}" \
    RL_JSON_VERSION_VERSION="${RL_JSON_VERSION_VERSION}" \
    TCLLIB_VERSION="${TCLLIB_VERSION}" \
    TCL_VERSION="${TCL_VERSION}" \
    TDOM_VERSION="${TDOM_VERSION}" \
    THREAD_VERSION="${THREAD_VERSION}" \
    XOTCL_VERSION="${XOTCL_VERSION}"

# ** Build
WORKDIR /
RUN mkdir /workspaces
COPY builds /builds
RUN  /bin/bash /builds/all-build.sh

# * From
FROM ubuntu:20.04

# ** Arguments
# ARG BUILD_DATE \
#     VERSION \
#     NS_VERSION \
#     NS_MODULES_VERSION \
#     RL_JSON_VERSION \
#     TCLLIB_VERSION \
#     TCL_VERSION \
#     TDOM_VERSION \
#     THREAD_VERSION \
#     XOTCL_VERSION

# ** Copy Naviserver
WORKDIR /
COPY --from=build /usr/local/ns /usr/local/ns/

# ** Labels
# hadolint ignore=DL3048
LABEL \
    maintainer="Oupfiz V <oupfiz5@yandex.ru>" \
    org.opencontainers.image.created="${BUILD_DATE}" \
    org.opencontainers.image.authors="Oupfiz V (Five)" \
    org.opencontainers.image.url="https://chiselapp.com/user/oupfiz5/repository/naviserver/home" \
    org.opencontainers.image.documentation="https://chiselapp.com/user/oupfiz5/repository/naviserver/wiki" \
    org.opencontainers.image.source="https://chiselapp.com/user/oupfiz5/repository/naviserver/brlist" \
    org.opencontainers.image.version="0.0.1d" \
    org.opencontainers.image.revision="" \
    org.opencontainers.image.vendor="" \
    org.opencontainers.image.licenses="" \
    org.opencontainers.image.ref.name="" \
    org.opencontainers.image.title="NaviServer on ubuntu base docker image" \
    org.opencontainers.image.description="NaviServer on ubuntu base docker image" \
    custom.package.version.naviserver="${NS_VERSION}" \
    custom.package.version.naviserver_modules="${NS_MODULES_VERSION}" \
    custom.package.version.rl_json_version="${RL_JSON_VERSION}" \
    custom.package.version.tcl="${TCL_VERSION}" \
    custom.package.version.tcllib="${TCLLIB_VERSION}" \
    custom.package.version.tdom="${TDOM_VERSION}" \
    custom.package.version.thread="${THREAD_VERSION}" \
    custom.package.version.xotcl="${XOTCL_VERSION}"

# ** Run
# hadolint ignore=DL3008
RUN export LANG="en_US.UTF-8" \
    && export LC_ALL="en_US.UTF-8" \
    && apt-get update \
    && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y --no-install-recommends install libssl-dev locales libpq-dev postgresql-client wget \
    && locale-gen en_US.UTF-8 \
    && update-locale LANG="en_US.UTF-8" \
    && update-locale LC_ALL="en_US.UTF-8" \
    && apt-get clean \
    && apt-get auto-remove -y \
    && rm -rf /tmp/* /var/lib/apt/lists/* /var/cache/apt/* \
    && groupadd nsadmin \
    && useradd -g nsadmin nsadmin \
    && chown -R  nsadmin:nsadmin /usr/local/ns

# ** Copy naviserver configurations
COPY rootfs/usr/local/ns/conf /usr/local/ns/conf

# ** Expose
EXPOSE 8080

# ** Environment
Env NS_CONF="/usr/local/ns/conf/nsd-config.tcl"

# ** Workdir
WORKDIR /usr/local/ns

# ** Entrypoint
ENTRYPOINT ["/usr/local/ns/bin/nsd"]

# ** Cmd
CMD [ "-f", "-u","nsadmin","-g","nsadmin","-t", "/usr/local/ns/conf/nsd-config.tcl"]