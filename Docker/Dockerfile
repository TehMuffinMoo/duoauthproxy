# escape=`

# From Microsoft's official Centos-7 image
FROM centos:8

# Install GPG Keys + Update
RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
RUN yum update -y

# Required for gssntlmssp
RUN yum install -y  gcc make libffi-devel perl zlib-devel diffutils wget

# Install & Configure Duo Security LDAP Proxy
RUN mkdir -p /tmp
RUN cd /tmp
RUN useradd -s /sbin/nologin duo
RUN wget https://dl.duosecurity.com/duoauthproxy-latest-src.tgz -P /tmp/
RUN mkdir /tmp/duoauthproxy-latest-src
RUN tar xzf /tmp/duoauthproxy-latest-src.tgz -C /tmp/duoauthproxy-latest-src --strip-components=1
RUN ls /tmp/duoauthproxy-latest-src/
WORKDIR /tmp/duoauthproxy-latest-src/
RUN echo "$PWD Hello"
RUN make
RUN ./duoauthproxy-build/install --install-dir /opt/duoauthproxy --service-user duo_authproxy_svc --log-group duo_authproxy_grp --create-init-script yes
RUN chown -R duo:duo /opt/duoauthproxy
USER duo
ENTRYPOINT ["/opt/duoauthproxy/bin/authproxy"]
