# The FROM field will be overridden by our build configuration
# in the Hono template and is here just for completeness.

FROM fabric8/s2i-java:3.0-java11

ENV \
    NETTY_TCNATIVE_VERSION=2.0.25.Final

USER root

RUN yum update -y
RUN yum install -y iproute less

#
# Install APR for performance improved TLS/SSL
#

RUN yum install -y apr

#
# Install a locally compiled version of the JNI library for netty-tcnative
# matching with the locally available SSL library.
#

RUN \
    yum install -y git autoconf automake libtool which apr-devel make openssl-devel java-11-openjdk-devel && \
    git clone https://github.com/netty/netty-tcnative && \
    cd netty-tcnative && \
    git checkout netty-tcnative-parent-$NETTY_TCNATIVE_VERSION && \
    cd .. && \
    JAVA_HOME=/usr/lib/jvm/java-11 mvn -B clean install -f netty-tcnative/openssl-dynamic/pom.xml -am -DskipTests && \
    install -m 0755 netty-tcnative/openssl-dynamic/target/native-lib-only/META-INF/native/linux64/libnetty_tcnative.so /usr/lib64/libnetty_tcnative.so && \
    rm -Rf ~/.m2/repository && \
    rm -Rf netty-tcnative && \
    yum -y history undo last && yum -y clean all && rm -rf /var/cache/yum

#
# Switch back to user
#

USER 1000
