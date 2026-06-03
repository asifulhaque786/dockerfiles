# Creator  David Darville - bw7
# Maintainer Divyam Azad <nihaldivyam@gmail.com>
# Start with Ubuntu LTS 22.04
FROM ubuntu:resolute

RUN apt-get update
# Otherwise, the install requires keyboard interaction
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends tzdata
RUN apt-get install -y jq gzip gnupg wget apt-transport-https ca-certificates unzip bind9-dnsutils vim less dateutils curl libdbd-pg-perl libdata-dumper-simple-perl inotify-tools --no-install-recommends
RUN apt-get dist-upgrade -y

# Enable Google Kubernetes repo
RUN echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.31/deb/ /" | tee /etc/apt/sources.list.d/kubernetes.list
RUN curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.31/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Enable Azure repo
RUN mkdir -p /etc/apt/keyrings
RUN curl -sLS https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor | tee /etc/apt/keyrings/microsoft.gpg > /dev/null chmod go+r /etc/apt/keyrings/microsoft.gpg
RUN echo "deb [arch=`dpkg --print-architecture` signed-by=/etc/apt/keyrings/microsoft.gpg] https://packages.microsoft.com/repos/azure-cli/ jammy main" | tee /etc/apt/sources.list.d/azure-cli.list

RUN apt-get update
RUN apt-get install -y kubectl --no-install-recommends

# Download and install AWS CLI v2
RUN export CPU_ARCHITECTURE=$([ "$(uname -m)" = "x86_64" ] && echo "x86_64" || echo "aarch64") &&\
	wget https://awscli.amazonaws.com/awscli-exe-linux-"${CPU_ARCHITECTURE}".zip && \
	unzip awscli-exe-linux-"${CPU_ARCHITECTURE}".zip && \
	aws/install && \
	rm -fr aws/ awscli-exe-linux-"${CPU_ARCHITECTURE}".zip

# Install AZURE cli
RUN apt-get install -y azure-cli --no-install-recommends

# Remove stuff that is now unneeded
RUN apt-get purge -v gnupg apt-transport-https ca-certificates unzip
RUN apt-get autoremove --yes
RUN apt-get clean
RUN rm -fr /var/cache /var/lib/apt

# Create user account
RUN echo "script:x:1000:1000::/home/script:/bin/bash" >> /etc/passwd
RUN echo "script:x:1000:" >> /etc/group
RUN mkdir /home/script
RUN chown 1000:1000 /home/script
USER 1000:1000

CMD ["/bin/bash", "/mnt/script"]
