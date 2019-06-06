# phd_code_style_utils_and_42
Collection of links to code, style and everything related to the old one Cthulhu, 42 and Reply

# Coding, Projects and Reproducible Research
- Cookiecutter for data-sciene: how to kickstart your local python code.
  - https://medium.com/@rrfd/cookiecutter-data-science-organize-your-projects-atom-and-jupyter-2be7862f487e
  - https://drivendata.github.io/cookiecutter-data-science/#be-conservative-in-changing-the-default-folder-structure
 
## setting mondgodb on IRISA virtual machine
### Install MongoDB
MongoDB 4 is installed on CentOS 7 using the upstream repository. Add the repository to your CentOS 7 server by running below commands:

```bash
$ cat >/etc/yum.repos.d/mongodb-org-4.0.repo<<EOF
[mongodb-org-4.0]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/7/mongodb-org/4.0/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-4.0.asc
EOF
```
Once the repo has been added, install mongodb-org package
```bash
sudo yum install mongodb-org
```
The installation of the above package will install the following dependency packages:
- **mongodb-org-server**: This provides MongoDB daemon mongod
- **mongodb-org-mongos**: This is a MongoDB Shard daemon
- **mongodb-org-shell**: This provides a shell to MongoDB
- **mongodb-org-tools**: MongoDB tools used for export, dump, import e.t.c

### Mount the partition for the data
mount the network partition in Nephila
```bash
# mount nas4.irisa.fr:/mongoDB_panama/mongoDB_panama /mnt/nephila/
```
Create the directory for mongo
```bash
$ /mnt/nephila/data
$ /mnt/nephila/data/mongo/
```
Provinde access to the user mongod to the folder
```
$ chown -R mongod:mongod /mnt/nephila/data/
$ chmod -R 775 /mnt/nephila/data
```
Change MongoDB data store location
```bash
$ sudo vim /etc/mongod.conf
storage:
dbPath: /mnt/nephila/data/mongo
journal:
  enabled: true
...
```
### Start Mongo server
When all is set, start and set mongod service to start on boot.
```bash
$ sudo systemctl enable mongod # start
$ sudo systemctl status mongod # check
```
### Uninstall
1. Stop MongoDB
```bash
$ sudo service mongod stop
```
2. Remove Packages.
```bash
$ sudo yum erase $(rpm -qa | grep mongodb-org)
```
3. Remove Data Directories.
```bash
$ sudo rm -r /var/log/mongodb
$ sudo rm -r /var/lib/mongo
```
