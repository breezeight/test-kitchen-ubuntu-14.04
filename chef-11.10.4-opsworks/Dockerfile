# test-kitchen

FROM  breezeight/test-kitchen-ubuntu-14.04:chef-11.10.4

# install dependencies
RUN apt-get install -y curl libxml2-dev libxslt-dev libyaml-dev

# create opsworks directories and seed them with expected config files
RUN mkdir -p /etc/aws/opsworks/ /opt/aws/opsworks/ /var/log/aws/opsworks/ /var/lib/aws/opsworks/ /var/lib/cloud/
COPY opsworks /var/lib/aws/opsworks

RUN mkdir temp_agent && \
  cd temp_agent && \
  wget -O opsworks-agent.tgz https://opsworks-instance-agent.s3.amazonaws.com/33200020141203204624/opsworks-agent-installer.tgz && \
  tar -xvzpof opsworks-agent.tgz && \
  cd opsworks-agent-installer/opsworks-agent/bin/ && \
  ./installer_wrapper.sh -R opsworks-instance-assets.s3.amazonaws.com && \
  rm -rf /temp_agent

# ensure we can access the opsworks chef gem
RUN echo "export PATH=\$PATH:/opt/aws/opsworks/current/bin" > /etc/profile.d/opsworks_path.sh

# don't automatically run opsworks-agent
RUN rm -f /etc/monit.d/opsworks-agent.monitrc /etc/monit/conf.d/opsworks-agent.monitrc
