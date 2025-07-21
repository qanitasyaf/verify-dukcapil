# Dockerfile di ./jenkins
FROM jenkins/jenkins:lts

USER root

# Install Docker CLI (from previous solution)
RUN apt-get update && \
    apt-get install -y --no-install-recommends docker.io && \
    rm -rf /var/lib/apt/lists/*

# Install Google Cloud CLI
ENV CLOUDSDK_CORE_DISABLE_PROMPTS=1
RUN apt-get update && apt-get install -y --no-install-recommends \
    apt-transport-https ca-certificates gnupg \
    && echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list \
    && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key --keyring /usr/share/keyrings/cloud.google.gpg add - \
    && apt-get update && apt-get install -y google-cloud-sdk \
    && rm -rf /var/lib/apt/lists/*

# Install Terraform CLI
# Mendapatkan versi Terraform terbaru (sesuaikan jika Anda butuh versi spesifik)
# Contoh untuk Terraform 1.x
RUN curl -LO https://releases.hashicorp.com/terraform/1.9.0/terraform_1.9.0_linux_amd64.zip \
    && unzip terraform_1.9.0_linux_amd64.zip -d /usr/local/bin/ \
    && rm terraform_1.9.0_linux_amd64.zip

# Tambahkan jenkins user ke grup docker (jika belum)
RUN usermod -aG docker jenkins

USER jenkins