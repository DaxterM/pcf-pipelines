FROM cloudfoundry/cflinuxfs2

COPY om-linux /usr/local/bin/om-linux
COPY cf /usr/local/bin/cf
COPY cliaas-linux /usr/local/bin/cliaas-linux
COPY pivnet-cli /usr/local/bin/pivnet-cli
COPY govc /usr/local/bin/govc
COPY om-linux-beta /usr/local/bin/om-linux-beta
COPY stemcell-downloader /usr/local/bin/stemcell-downloader

RUN \
# jq
wget -O /usr/local/bin/jq https://github.com/stedolan/jq/releases/download/jq-1.5/jq-linux64 && chmod +x /usr/local/bin/jq && \

# Terraform
wget -q -O- https://www.terraform.io/downloads.html | grep linux_amd64 | sed -e's/^.*"\(.*\)".*$/\1/g' | xargs wget -O /tmp/terraform.zip && unzip -d /usr/local/bin /tmp/terraform.zip && chmod +x /usr/local/bin/terraform && \

# gcloud SDK
export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)" && \
echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | sudo tee /etc/apt/sources.list.d/google-cloud-sdk.list && \
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
apt-get -y update && sudo apt-get -y install --no-install-recommends google-cloud-sdk && \

# pip
apt-get install -y --no-install-recommends python-pip python-dev && \

# UAA Client
gem install cf-uaac --no-ri --no-rdoc && \

# AWS Client
pip install awscli && \

# Openstack Client
pip install python-openstackclient pytz python-neutronclient && \

# Clean up
apt-get remove -y python-pip python-dev && \
apt-get -y clean && apt-get -y autoremove --purge && rm -rf /etc/apt/ && \
rm -rf /tmp/* && \
find /var/lib/apt/lists -type f | xargs rm -f && \
find /var/cache/debconf -type f -name '*-old' | xargs rm -f && \
find /var/log -type f -user root | xargs rm -rf && \
for file in $(find /var/log -type f -user syslog); do echo > $file; done