ARG POSTGIS_IMAGE_TAG=17-3.6-alpine
FROM postgis/postgis:${POSTGIS_IMAGE_TAG}

LABEL maintainer="info@redmic.es"

ARG PG_CRON_VERSION \
	PG_PARTMAN_VERSION \
	BUILD_BASE_VERSION \
	CLANG_VERSION \
	LLVM_VERSION \
	CA_CERTIFICATES_VERSION \
	OPENSSL_VERSION \
	TAR_VERSION

# hadolint ignore=DL3003
RUN apk add --no-cache --no-check-certificate --virtual .build-deps \
		build-base="${BUILD_BASE_VERSION}" \
		clang="${CLANG_VERSION}" \
		llvm="${LLVM_VERSION}" \
		ca-certificates="${CA_CERTIFICATES_VERSION}" \
		openssl="${OPENSSL_VERSION}" \
		tar="${TAR_VERSION}" && \
# install pg_cron
	wget -q -O pg_cron.tar.gz "https://github.com/citusdata/pg_cron/archive/v${PG_CRON_VERSION}.tar.gz" && \
	tar -xzf pg_cron.tar.gz && \
	cd pg_cron-* && \
	sed -i.bak -e 's/-Werror//g' Makefile && \
	sed -i.bak -e 's/-Wno-implicit-fallthrough//g' Makefile && \
	make && \
	make install && \
	cd .. ; \
# install pg_partman
	wget -q -O pg_partman.tar.gz "https://github.com/pgpartman/pg_partman/archive/v${PG_PARTMAN_VERSION}.tar.gz" && \
	tar -xzf pg_partman.tar.gz && \
	cd pg_partman-* && \
	make && \
	make NO_BGW=1 install && \
	cd .. ; \
# clean
	rm -rf pg_cron* pg_partman* && \
	apk del .build-deps
