FROM gcr.io/cloud-builders/gcloud

ENV VERSION v2.8.2

COPY helm.bash /builder/helm.bash

ENV PATH=/builder/helm/:$PATH
ENV HELM_HOME=/helm

RUN chmod +x /builder/helm.bash && \
  mkdir -p /builder/helm && \
  apt-get update && \
  apt-get install -y curl && \
  curl -SL https://storage.googleapis.com/kubernetes-helm/helm-${VERSION}-linux-amd64.tar.gz -o helm.tar.gz && \
  tar zxvf helm.tar.gz --strip-components=1 -C /builder/helm linux-amd64/helm && \
  rm helm.tar.gz && \
  curl -SL https://go.mozilla.org/sops/dist/sops_2.0.9_amd64.deb -o sops.deb && \
  apt install ./sops.deb && \
  echo "${HELM_HOME}/plugins" && \
  mkdir -p "${HELM_HOME}/plugins" && \
  helm plugin install https://github.com/futuresimple/helm-secrets --version 1.3.1 && \
  apt-get remove --purge -y curl && \
  apt-get --purge -y autoremove && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*


ENTRYPOINT ["/builder/helm.bash"]
