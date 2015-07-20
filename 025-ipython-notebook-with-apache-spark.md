#Using IPython Notebook with Apache Spark



```bash
yum install nano centos-release-SCL zlib-devel \
bzip2-devel openssl-devel ncurses-devel \
sqlite-devel readline-devel tk-devel \
gdbm-devel db4-devel libpcap-devel xz-devel \
libpng-devel libjpg-devel atlas-devel
```
![](https://www.dropbox.com/s/f2ebp87ed2mllms/Screenshot%202015-07-20%2010.04.16.png?dl=1)

```bash
yum groupinstall "Development tools"
```
![](https://www.dropbox.com/s/fubupr8ivfuff0o/Screenshot%202015-07-20%2010.08.06.png?dl=1)

```bash
yum install python27
```
![](https://www.dropbox.com/s/jpmuio6y4cwyho2/Screenshot%202015-07-20%2010.09.55.png?dl=1)

```bash
source /opt/rh/python27/enable
```


```bash
wget https://bitbucket.org/pypa/setuptools\
/raw/bootstrap/ez_setup.py

python ez_setup.py

easy_install-2.7 pip
```

![](https://www.dropbox.com/s/c9wou8cgctlf7pz/Screenshot%202015-07-20%2010.21.02.png?dl=1)

```bash
pip install numpy scipy pandas \
scikit-learn tornado pyzmq \
pygments matplotlib jinja2 jsonschema
```

![](https://www.dropbox.com/s/ves4gqmtz7acsux/Screenshot%202015-07-20%2010.58.32.png?dl=1)

```bash
pip install "ipython[notebook]"
```

![](https://www.dropbox.com/s/k8brxy9dgik6ohw/Screenshot%202015-07-20%2011.00.00.png?dl=1)

```bash
ipython profile create pyspark
```

![](https://www.dropbox.com/s/2klc4095wrxyz5d/Screenshot%202015-07-20%2011.01.58.png?dl=1)

```
c.NotebookApp.ip = '*'
c.NotebookApp.open_browser = False
c.NotebookApp.port = 8889
c.NotebookApp.notebook_dir = u'/usr/hdp/current/spark-client/'
```

![](https://www.dropbox.com/s/xcdasm4tmmnyibi/Screenshot%202015-07-20%2011.10.50.png?dl=1)

`nano ~/start_ipython_notebook.sh`

```
#!/bin/bash
source /opt/rh/python27/enable
IPYTHON_OPTS="notebook --profile pyspark" pyspark
```

![](https://www.dropbox.com/s/r9sagxlzixee8mk/Screenshot%202015-07-20%2011.15.27.png?dl=1)

```bash
chmod +x start_ipython_notebook.sh
```
![](https://www.dropbox.com/s/ofqdaeuevnk05mo/Screenshot%202015-07-20%2011.17.19.png?dl=1)


Port Forwarding

![](https://www.dropbox.com/s/3lcecis4oajtu63/Screenshot%202015-07-20%2011.18.35.png?dl=1)

![](https://www.dropbox.com/s/5xr5bprqde2epr6/Screenshot%202015-07-20%2011.20.00.png?dl=1)

```
./start_ipython_notebook.sh
```
![](https://www.dropbox.com/s/phtrtmv6s01g13k/Screenshot%202015-07-20%2011.21.00.png?dl=1)


![](https://www.dropbox.com/s/2ga17v2a8klpdz9/Screenshot%202015-07-20%2011.22.06.png?dl=1)


```
