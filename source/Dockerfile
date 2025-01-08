FROM hashicorp/terraform:1.10.4

RUN addgroup -g 1000 ansible && \
  adduser -u 1000 -G ansible -h /work -D ansible && \
  mkdir -p /work /work/terraform && \
  chmod 775 /work && \
  chown ansible:ansible -R /work

# hadolint ignore=DL3018
RUN apk add --no-cache python3 bash gcc python3-dev build-base linux-headers && \
  rm -rf /var/cache/apk/* && \
  python3 -m ensurepip

WORKDIR /work
ENV HOME /work

ARG BUILD_VERSION
RUN touch /version.txt && \
  chown ansible:ansible /version.txt && \
  chmod 777 /version.txt

USER 1000:1000

COPY --chown=ansible:ansible "./playbook/requirements.txt" "./playbook/requirements.yml" \
  "./playbook/ansible.cfg"  /work/
COPY --chown=ansible:ansible ./playbook/exec/ /work/exec/
RUN /work/exec/exec ansible_setup /work

COPY --chown=ansible:ansible ./playbook/ /work

RUN echo "$BUILD_VERSION" > /version.txt

ENTRYPOINT [ "/work/entrypoint" ]
