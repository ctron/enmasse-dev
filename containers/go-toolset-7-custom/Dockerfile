# The FROM field will be overridden by our build configuration
# in the Hono template and is here just for completeness.

FROM centos/go-toolset-7-centos7:latest

USER root

RUN yum update -y
RUN yum install -y epel-release
RUN yum install -y python2-qpid qpid-dispatch-tools qpid-dispatch-router

USER 1001
