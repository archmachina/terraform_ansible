FROM hashicorp/terraform:1.5.4

RUN addgroup -g 1000 ansible && \
  adduser -u 1000 -G ansible -h /ansible -D ansible && \
  mkdir -p /ansible /work && \
  chmod 775 /ansible /work && \
  chown ansible:ansible /ansible /work

RUN apk add python3 bash && python3 -m ensurepip

WORKDIR /work
ENV HOME /ansible

ARG BUILD_VERSION
RUN touch /version.txt && \
  chown ansible:ansible /version.txt && \
  chmod 777 /version.txt

USER 1000:1000

COPY --chown=ansible:ansible "./playbook/requirements.txt" "./playbook/requirements.yml" \
  "./playbook/ansible.cfg"  /ansible/
COPY --chown=ansible:ansible ./playbook/exec/ /ansible/exec/
RUN /ansible/exec/exec ansible_setup /ansible

COPY --chown=ansible:ansible ./playbook/ /ansible

RUN echo "$BUILD_VERSION" > /version.txt

ENTRYPOINT [ "/ansible/entrypoint" ]
