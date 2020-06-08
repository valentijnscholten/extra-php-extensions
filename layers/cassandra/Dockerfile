ARG PHP_VERSION
FROM bref/build-php-$PHP_VERSION AS ext

# newest
ENV LIBUV_VERSION 1.35.0
ENV CCP_DRIVER_VERSION 2.15.2

# post processor 
# ENV LIBUV_VERSION 1.18.0
# ENV CCP_DRIVER_VERSION 2.8.1
#ENV CASSANDRA_VERSION 1.3.2


# Fix library path
ENV LD_LIBRARY_PATH=/usr/lib:/usr/lib64:$LD_LIBRARY_PATH


WORKDIR /tmp
# RUN curl -o libuv.rpm https://downloads.datastax.com/cpp-driver/centos/7/dependencies/libuv/v1.35.0/libuv-1.35.0-1.el7.x86_64.rpm
# RUN rpm -qlp libuv.rpm

# RUN curl -o cpp.rpm https://downloads.datastax.com/cpp-driver/centos/7/cassandra/v2.15.1/cassandra-cpp-driver-2.15.1-1.el7.x86_64.rpm
# RUN rpm -qlp cpp.rpm

# RUN curl -o cpp-dev.rpm https://downloads.datastax.com/cpp-driver/centos/7/cassandra/v2.15.1/cassandra-cpp-driver-devel-2.15.1-1.el7.x86_64.rpm
# RUN rpm -qlp cpp-dev.rpm


# Install dependencies
RUN curl -o -v libuv.rpm https://downloads.datastax.com/cpp-driver/centos/7/dependencies/libuv/v$LIBUV_VERSION/libuv-$LIBUV_VERSION-1.el7.x86_64.rpm
# RUN curl -o libuv.rpm https://downloads.datastax.com/cpp-driver/centos/7/dependencies/libuv/v$LIBUV_VERSION/libuv-$LIBUV_VERSION-1.el7.centos.x86_64.rpm
RUN yum install -y libuv.rpm
RUN curl -o -v libuv-dev.rpm https://downloads.datastax.com/cpp-driver/centos/7/dependencies/libuv/v$LIBUV_VERSION/libuv-devel-$LIBUV_VERSION-1.el7.x86_64.rpm
# RUN curl -o libuv-dev.rpm https://downloads.datastax.com/cpp-driver/centos/7/dependencies/libuv/v$LIBUV_VERSION/libuv-devel-$LIBUV_VERSION-1.el7.centos.x86_64.rpm
RUN yum install -y libuv-dev.rpm
#RUN ln -s /usr/local/lib/libuv.so.1 /usr/lib64/libuv.so.1
#RUN ln -s /usr/local/lib/libuv-devel.so.1 /usr/lib64/libuv-devel.so.1


RUN yum install -y https://downloads.datastax.com/cpp-driver/centos/7/cassandra/v$CCP_DRIVER_VERSION/cassandra-cpp-driver-$CCP_DRIVER_VERSION-1.el7.x86_64.rpm
RUN yum install -y https://downloads.datastax.com/cpp-driver/centos/7/cassandra/v$CCP_DRIVER_VERSION/cassandra-cpp-driver-devel-$CCP_DRIVER_VERSION-1.el7.x86_64.rpm
# RUN yum install -y https://downloads.datastax.com/cpp-driver/centos/7/cassandra/v$CCP_DRIVER_VERSION/cassandra-cpp-driver-$CCP_DRIVER_VERSION-1.el7.centos.x86_64.rpm
# RUN yum install -y https://downloads.datastax.com/cpp-driver/centos/7/cassandra/v$CCP_DRIVER_VERSION/cassandra-cpp-driver-devel-$CCP_DRIVER_VERSION-1.el7.centos.x86_64.rpm


RUN yum install -y automake cmake gcc gcc-c++ git libtool openssl-devel wget gmp gmp-devel boost php-devel pcre-devel 

# RUN curl -A "Docker" -o /tmp/cassandra-cpp-driver_$CCP_DRIVER_VERSION-1_amd64.deb -D - -L -s https://downloads.datastax.com/cpp-driver/ubuntu/$UBUNTU_VERSION/cassandra/v$CCP_DRIVER_VERSION/cassandra-cpp-driver_$CCP_DRIVER_VERSION-1_amd64.deb  \
#     && mkdir -p /tmp/cassandra-cpp && cd /tmp/cassandra-cpp \
#     && ar x /tmp/cassandra-cpp-driver_$CCP_DRIVER_VERSION-1_amd64.deb \
#     && tar xf /tmp/cassandra-cpp/data.tar.xz \  
#     && cp /tmp/cassandra-cpp/usr/lib/x86_64-linux-gnu/libcassandra.so.2 /tmp/libcassandra.so \
#     && rm -rf /tmp/cassandra-cpp /tmp/cassandra-cpp-driver_$CCP_DRIVER_VERSION-1_amd64.deb

# RUN curl -A "Docker" -o /tmp/cassandra-cpp-driver-dev_$CCP_DRIVER_VERSION-1_amd64.deb -D - -L -s https://downloads.datastax.com/cpp-driver/ubuntu/$UBUNTU_VERSION/cassandra/v$CCP_DRIVER_VERSION/cassandra-cpp-driver-dev_$CCP_DRIVER_VERSION-1_amd64.deb  \
#     && mkdir -p /tmp/cassandra-cpp-dev && cd /tmp/cassandra-cpp-dev \
#     && ar x /tmp/cassandra-cpp-driver-dev_$CCP_DRIVER_VERSION-1_amd64.deb \
#     && tar xf /tmp/cassandra-cpp-dev/data.tar.xz \
#     && cp /tmp/cassandra-cpp-dev/usr/lib/x86_64-linux-gnu/libcassandra_static.a /tmp/libcassandra_static.a \
#     && cp /tmp/cassandra-cpp-dev/usr/include/cassandra.h /tmp/cassandra.h \
#     && rm -rf /tmp/cassandra-cpp-dev /tmp/cassandra-cpp-driver-dev_$CCP_DRIVER_VERSION-1_amd64.deb

#  RUN git clone https://github.com/datastax/cpp-driver.git && \
#     mkdir cpp-driver/build  && \
#     cd cpp-driver/build && \
#     cmake ..  && \ 
#     # sudo make install
#     make install


RUN cd /tmp && \
    git clone https://github.com/datastax/php-driver.git && \
    cd php-driver && \
    pecl install ext/package.xml

RUN cp `php-config --extension-dir`/cassandra.so /tmp/cassandra.so

# Cassandra PHP Driver with submoduled, statically compiled C/C++ driver
# RUN curl -o 141.patch https://git.remirepo.net/cgit/rpms/php/pecl/php-pecl-cassandra.git/plain/141.patch
# RUN cd /tmp && \
#     git clone https://github.com/datastax/php-driver.git && \
#     cd php-driver && \
#     git submodule update --init && \
# # force the cpp driver submodule to the latest master so we get a necessary
# # build fix
# #   the commit missing is: https://github.com/datastax/cpp-driver/commit/0edf3d2eff88d7594ed7dc070f889b1bcacb677f
#     git submodule foreach git pull origin master && \
#     cd ext && \
# # note that this will delete, then create, then use, then delete again:
# #   /tmp/php-driver-installation
#     # scl enable php72 -- bash -c "\
#     cp /tmp/141.patch . && \
#     git apply 141.patch && \
#     bash -c "\ 
#     ./install.sh" && \
#     rm -rf /tmp/php-driver && \
#     echo "extension=cassandra.so" > /etc/opt/remi/php72/php.d/97-cassandra.ini

# RUN cat config.log

# RUN curl -A "Docker" -o /tmp/libuv_$LIBUV_VERSION-1_amd64.deb -D - -L -s https://downloads.datastax.com/cpp-driver/ubuntu/$UBUNTU_VERSION/dependencies/libuv/v$LIBUV_VERSION/libuv_$LIBUV_VERSION-1_amd64.deb  \
#     && mkdir -p /tmp/libuv && cd /tmp/libuv \
#     && ar x /tmp/libuv_$LIBUV_VERSION-1_amd64.deb \
#     && tar xf /tmp/libuv/data.tar.xz \
#     && cp /tmp/libuv/usr/lib/x86_64-linux-gnu/libuv.so.1 /tmp/libuv.so \
#     && rm -rf /tmp/libuv /tmp/libuv_$LIBUV_VERSION-1_amd64.deb

# RUN pecl install cassandra-$CASSANDRA_VERSION 
    # && docker-php-ext-enable cassandra

#RUN cp `php-config --extension-dir`/cassandra.so /tmp/cassandra.so

# Build the final image from the lambci image that is close to the production environment
FROM lambci/lambda:provided

# Copy things we installed to the final image
# COPY --from=ext /tmp/cassandra.so /opt/bref-extra/cassandra.so
# COPY --from=ext /tmp/cassandra.h /opt/bref/include/cassandra.h
# COPY --from=ext /tmp/libcassandra_static.a /opt/bref/lib64/libcassandra_static.a
# COPY --from=ext /tmp/libuv.so /opt/bref/lib64/libuv.so


# php extension binary
COPY --from=ext /tmp/cassandra.so /opt/bref-extra/cassandra.so
# cassandra cpp driver files
# COPY --from=ext /usr/lib64/libcassandra.so /opt/bref/lib64/libcassandra.so
COPY --from=ext /usr/lib64/libcassandra.so.2 /opt/bref/lib64/libcassandra.so.2
# COPY --from=ext /usr/lib64/libcassandra.so.2.15.1 /opt/bref/lib64/libcassandra.so.2.15.1
COPY --from=ext /usr/lib64/libcassandra_static.a /opt/bref/lib64/libcassandra_static.a

COPY --from=ext /usr/include/cassandra.h /opt/bref/include/cassandra.h
# COPY --from=ext /usr/include/dse.h /opt/bref/include/dse.h

COPY --from=ext /usr/lib64/pkgconfig/cassandra.pc /opt/bref/lib64/pkgconfig/cassandra.pc
COPY --from=ext /usr/lib64/pkgconfig/cassandra_static.pc /opt/bref/lib64/pkgconfig/cassandra_static.pc

# dependencies
COPY --from=ext /usr/lib64/libuv.so.1 /opt/bref/lib64/libuv.so.1


