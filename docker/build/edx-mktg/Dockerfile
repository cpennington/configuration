# To build this Dockerfile:
#
# From the root of configuration:
#
# docker build -f docker/build/course-discovery/Dockerfile .
#
# This allows the dockerfile to update /edx/app/edx_ansible/edx_ansible
# with the currently checked-out configuration repo.

FROM edxops/trusty-common:latest
MAINTAINER edxops

ARG EDX_MKTG_VERSION=master
ARG REPO_OWNER=edx
ARG GIT_IDENTITY=""

ADD . /edx/app/edx_ansible/edx_ansible

WORKDIR /edx/app/edx_ansible/edx_ansible/docker/plays

COPY docker/build/course-discovery/ansible_overrides.yml /
RUN sudo /edx/app/edx_ansible/venvs/edx_ansible/bin/ansible-playbook edx_mktg.yml \
    -c local -i '127.0.0.1,' \
    -t 'install' \
    --extra-vars="@/ansible_overrides.yml" \
    --extra-vars="EDX_MKTG_VERSION=$EDX_MKTG_VERSION" \
    --extra-vars="COMMON_GIT_PATH=$REPO_OWNER" \
    --extra-vars="EDX_MKTG_GIT_IDENTITY=$GIT_IDENTITY"

USER root 
CMD ["/edx/app/supervisor/venvs/supervisor/bin/supervisord", "-n", "--configuration", "/edx/app/supervisor/supervisord.conf"]
