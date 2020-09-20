# ansible-reports
a template for some ansible reporting

example:

ansible-playbook -u root -i ~/hosts.inventory report.yml | tee ansible.report.$(date +%y%d%m%H%M%S).txt
