FROM ccon/20160123

# Install dependencies
RUN emerge-webrsync
RUN etc-update

RUN echo '=app-admin/ec2-ami-tools-1.5.6 ~amd64' > /etc/portage/package.accept_keywords

RUN echo -e "$(cat /root/.ssl-pass)\n$(cat /root/.ssl-pass)\nCA\nBC\nVancouver\nccon\nDevOps\n\n" | openssl req -x509 -newkey rsa:2048 -keyout key.pem -out cert.pem -days 365

RUN emerge ec2-ami-tools sys-fs/multipath-tools sys-boot/grub sys-boot/grub-static
RUN sed -i '243,254s/^/#/' /usr/lib/ec2/platform/linux/image.rb
RUN echo "$(cat /root/.ssl-pass)" | ec2-bundle-vol --no-inherit -P mbr -d /mnt -k cert.pem -c /etc/ec2/amitools/cert-ec2.pem  -u $(cat /root/.aws-account) -r x86_64
