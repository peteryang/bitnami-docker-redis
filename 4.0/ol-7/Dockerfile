FROM oraclelinux:7-slim
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV BITNAMI_PKG_CHMOD="-R g+rwX" \
    HOME="/" \
    OS_ARCH="x86_64" \
    OS_FLAVOUR="ol-7" \
    OS_NAME="linux"

COPY prebuildfs /
# Install required system packages and dependencies
RUN install_packages ca-certificates curl glibc gzip hostname libaio-devel procps-ng sudo tar unzip which
RUN . ./libcomponent.sh && component_unpack "redis" "4.0.14-3" --checksum c894cff0e8e14d13086aee2ef4e1c315d19385ab95d15ba54dda7f8df49d0730
RUN yum upgrade -y && \
    rm -r /var/cache/yum
RUN /build/install-gosu.sh
RUN curl --silent -L https://github.com/bitnami/wait-for-port/releases/download/v1.0/wait-for-port.zip > /tmp/wait-for-port.zip && \
    echo "8d26181f4629211b70db4f96236616056b1ed8e5920d8023f7c883071e76c1ed /tmp/wait-for-port.zip" | sha256sum --check && \
    unzip -q -d /usr/local/bin -o /tmp/wait-for-port.zip wait-for-port && \
    mkdir -p /opt/bitnami/licenses && \
    curl --silent -L https://raw.githubusercontent.com/bitnami/wait-for-port/master/COPYING > /opt/bitnami/licenses/wait-for-port-1.0.txt

COPY rootfs /
RUN /postunpack.sh
ENV BITNAMI_APP_NAME="redis" \
    BITNAMI_IMAGE_VERSION="4.0.14-ol-7-r323" \
    NAMI_PREFIX="/.nami" \
    PATH="/opt/bitnami/redis/bin:$PATH"

EXPOSE 6379

USER 1001
ENTRYPOINT [ "/entrypoint.sh" ]
CMD [ "/run.sh" ]
