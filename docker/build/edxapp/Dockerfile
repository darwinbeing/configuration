# To build this Dockerfile:
#
# From the root of configuration:
#
# docker build -f docker/build/edxapp/Dockerfile . -t edxops/edxapp:latest
#
# This allows the dockerfile to update /edx/app/edx_ansible/edx_ansible
# with the currently checked-out configuration repo.


ARG IMAGE_PREFIX
FROM edxops/${IMAGE_PREFIX}xenial-common:latest
MAINTAINER edxops
USER root
CMD ["/edx/app/supervisor/venvs/supervisor/bin/supervisord", "-n", "--configuration", "/edx/app/supervisor/supervisord.conf"]

ADD . /edx/app/edx_ansible/edx_ansible
WORKDIR /edx/app/edx_ansible/edx_ansible/docker/plays

COPY docker/build/edxapp/ansible_overrides.yml /

ARG OPENEDX_RELEASE=master
ENV OPENEDX_RELEASE=${OPENEDX_RELEASE}
RUN sudo /edx/app/edx_ansible/venvs/edx_ansible/bin/ansible-playbook edxapp.yml \
    -c local -i '127.0.0.1,' \
    -t 'install,assets,devstack' \
    --extra-vars=edx_platform_version=${OPENEDX_RELEASE} \
    --extra-vars="@/ansible_overrides.yml"

EXPOSE 18000 18010
