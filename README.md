# ansible-reports
A template for some ansible reporting: a non-disruptive system status report.

Running this report against your linux servers on a regular basis and reviewing the diffs
can help an administrator or security analyst keep an eye on many aspects of the linux servers
with a detailed dataset in ansible playbook output for security auditing and supplemental
computer forensic data.

example:

ansible-playbook -u root -i ~/hosts.inventory report.yml | tee ansible.report.$(date +%y%d%m%H%M%S).txt

ansible-playbook -u root -i ~/hosts.inventory facts.yml | tee ansible.facts.$(date +%y%d%m%H%M%S).txt

Or use the wrapper utility in this repo:

chmod +x ./runreports

sudo cp runreports /usr/local/bin/runreports

runreports



The runreports is simply these steps:

cd ~
touch ansible.report.current.txt
touch ansible.report.last.txt
cp ansible.report.current.txt ansible.report.last.txt
stamp=$(date +%Y%m%d%H%M%S%N)
ansible-playbook -u root -i ~/hosts.inventory ~/report.yml  | tee ansible.report."$stamp".txt
cp ansible.report."$stamp".txt ansible.report.current.txt



Note that this report gathers a lot of information: every running process, open file, network connection, installed package, and more.
If you need to do a full review, there are two potentially disruptive elements that have been intentionally left out of report.yml:
complete memory dump (including kernel memory!), and complete disk clone (dd)
Those two elements should also be considered if you want a deeper review. Perhaps done less frequently than run report, only during maintenance or whatnot :)
