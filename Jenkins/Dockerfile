FROM jenkins/jenkins:lts-jdk21

USER root

# Install Docker CLI
RUN apt-get update \
  && apt-get install -y ca-certificates curl \
  && install -m 0755 -d /etc/apt/keyrings \
  && curl -fsSL https://download.docker.com/linux/debian/gpg \
  -o /etc/apt/keyrings/docker.asc \
  && chmod a+r /etc/apt/keyrings/docker.asc \
  && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
  > /etc/apt/sources.list.d/docker.list \
  && apt-get update \
  && apt-get install -y docker-ce-cli \
  && rm -rf /var/lib/apt/lists/*



# Install kubectl with retries
RUN set -eux; \
  ARCH="$(dpkg --print-architecture)"; \
  K8S_VERSION="$(curl --retry 5 --retry-all-errors \
  --retry-delay 2 -fsSL https://dl.k8s.io/release/stable.txt)"; \
  curl --retry 5 --retry-all-errors --retry-delay 2 \
  -fL --connect-timeout 20 \
  -o /usr/local/bin/kubectl \
  "https://dl.k8s.io/release/${K8S_VERSION}/bin/linux/${ARCH}/kubectl"; \
  chmod 0755 /usr/local/bin/kubectl

# Install Helm CLI
RUN set -eux; \
  HELM_VERSION="v4.3.0"; \
  ARCH="$(dpkg --print-architecture)"; \
  curl --retry 5 --retry-all-errors --retry-delay 2 \
  -fL --connect-timeout 20 \
  -o /tmp/helm.tar.gz \
  "https://get.helm.sh/helm-${HELM_VERSION}-linux-${ARCH}.tar.gz"; \
  tar -xzf /tmp/helm.tar.gz -C /tmp; \
  install -m 0755 "/tmp/linux-${ARCH}/helm" /usr/local/bin/helm; \
  rm -rf /tmp/helm.tar.gz "/tmp/linux-${ARCH}"

USER jenkins