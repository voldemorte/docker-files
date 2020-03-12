FROM alpine:3.11

ENV DEFAULT_TERRAFORM_VERSION=0.12.23
ENV DEFAULT_TERRAGRUNT_VERSION=0.21.10
ENV DEFAULT_TFLINT_VERSION=0.13.2
ENV KUBECTL_VERSION=1.17.3

RUN apk update && \
    apk add curl wget git && \
    curl -LOks https://releases.hashicorp.com/terraform/${DEFAULT_TERRAFORM_VERSION}/terraform_${DEFAULT_TERRAFORM_VERSION}_linux_amd64.zip && \
    mkdir -p /usr/local/bin/tf/versions/${DEFAULT_TERRAFORM_VERSION} && \
    unzip terraform_${DEFAULT_TERRAFORM_VERSION}_linux_amd64.zip -d /usr/local/bin/tf/versions/${DEFAULT_TERRAFORM_VERSION} && \
    ln -s /usr/local/bin/tf/versions/${DEFAULT_TERRAFORM_VERSION}/terraform /usr/local/bin/terraform

# Add terragrunt binary
RUN curl -s -Lo terragrunt https://github.com/gruntwork-io/terragrunt/releases/download/v${DEFAULT_TERRAGRUNT_VERSION}/terragrunt_linux_amd64 && \
    chmod +x terragrunt && \
    mv terragrunt /usr/local/bin

# Add tflint binary
RUN curl -s -Lo tflint.zip https://github.com/terraform-linters/tflint/releases/download/v${DEFAULT_TFLINT_VERSION}/tflint_linux_amd64.zip && \
    unzip tflint.zip && \
    chmod +x tflint && \
    mv tflint /usr/local/bin

# Add kubectl & AWS authenticator
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/ && \
    curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.15.10/2020-02-22/bin/linux/amd64/aws-iam-authenticator && \
    chmod +x aws-iam-authenticator && \
    mv aws-iam-authenticator /usr/local/bin/

RUN apk -v --update add \
    py3-pip \
    groff \
    less && \
    pip3 install awscli

RUN apk -v --purge del curl wget && \
    rm /var/cache/apk/*

COPY entrypoint.sh /usr/local/bin/entrypoint

ENTRYPOINT ["entrypoint"]
