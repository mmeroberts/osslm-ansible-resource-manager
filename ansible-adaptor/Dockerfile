FROM ansible/ansible:ubuntu1604py3

RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY requirements.txt /usr/src/app/

RUN pip3 install --no-cache-dir -r requirements.txt

COPY . /usr/src/app
COPY ansible.cfg /etc/ansible/
COPY tslvault.txt /etc/ansible/
COPY ans_alm_settings.yml /etc/ansible/

# device handling
RUN pip3 install --no-cache-dir -r device-requirements.txt
RUN ansible-galaxy install Juniper.junos
# fixes a bug in the junos libs (for python3)
COPY junos_install_config /etc/ansible/roles/Juniper.junos/library/.
COPY junos_get_config /etc/ansible/roles/Juniper.junos/library/.

# temp fix to enable port security disabling.
COPY openstackcloud.py /usr/local/lib/python3.5/dist-packages/shade/openstackcloud.py
COPY os_port.py /usr/local/lib/python3.5/dist-packages/ansible/modules/cloud/openstack/os_port.py


EXPOSE 8080
CMD ["python3","-m", "swagger_server"]
