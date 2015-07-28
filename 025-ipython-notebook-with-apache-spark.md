#Using IPython Notebook with Apache Spark

In this tutorial we are going to configure IPython with Apache Spark on YARN in a few steps. We will also explore how to use IPython with Spark after we configure it.

IPython is an interactive Python shell which lets you interact with your data one step at a time and also perform simple visualizations.

Here are some of the differences between the pyspark shell and the IPython shell. IPython supports tab autocompletion on class names, functions, methods, variables. It offers more explicit and colour-highlighted error messages than the command line python shell. It also provides basic UNIX shell integration allowing you can run simple shell commands such as cp, ls, rm, cp, etc. directly from the IPython. IPython integrates with many common GUI modules like PyQt, PyGTK, tkinter as well wide variety of data science Python packages.

###Prerequisites

The only prerequiste for this tutorial is the latest [Hortonworks Sandbox](http://hortonworks.com/sandbox) installed on your computer or on the [cloud](http://hortonworks.com/blog/hortonworks-sandbox-azure/). In case you are running an Hortonworks Sandbox with an earlier version of Apache Spark, for the instruction in this tutorial, you need to install the Apache Spark 1.3.1.
###Installing and configuring IPython

To begin, login in to Hortonworks Sandbox through SSH:![](https://www.dropbox.com/s/tzsxvsnxfo26jn7/Screenshot_2015-04-13_07_58_43.png?dl=1)The default password is `hadoop`.

Now let’s configure the dependencies by typing in the following command:

```bash
yum install nano centos-release-SCL zlib-devel \
bzip2-devel openssl-devel ncurses-devel \
sqlite-devel readline-devel tk-devel \
gdbm-devel db4-devel libpcap-devel xz-devel \
libpng-devel libjpg-devel atlas-devel
```
![](https://www.dropbox.com/s/f2ebp87ed2mllms/Screenshot%202015-07-20%2010.04.16.png?dl=1)

IPython has a requirement for Python 2.7 or higher. So, let's install the "Development tools" dependency for Python 2.7

```bash
yum groupinstall "Development tools"
```
![](https://www.dropbox.com/s/fubupr8ivfuff0o/Screenshot%202015-07-20%2010.08.06.png?dl=1)

Now we are ready to install Python 2.7.

```bash
yum install python27
```
![](https://www.dropbox.com/s/jpmuio6y4cwyho2/Screenshot%202015-07-20%2010.09.55.png?dl=1)

Now the Sandbox has multiple versions of Python, so we have to select which version of Python we want to use in this session. We will choose to use Python 2.7 in this session.

```bash
source /opt/rh/python27/enable
```

Then we will download `easy_install` which we will use to configure `pip`, a Python package installer.

```bash
wget https://bitbucket.org/pypa/setuptools\
/raw/bootstrap/ez_setup.py
```

Now let's configure `easy_install` with the following command:

```bash
python ez_setup.py
```

Now we can install `pip` with `easy_install` using the following command:

```bash
easy_install-2.7 pip
```

![](https://www.dropbox.com/s/c9wou8cgctlf7pz/Screenshot%202015-07-20%2010.21.02.png?dl=1)

`pip` makes it really easy to install the Python packages. We will use `pip` to install the data science packages we might need using the following command:

```bash
pip install numpy scipy pandas \
scikit-learn tornado pyzmq \
pygments matplotlib jinja2 jsonschema
```

![](https://www.dropbox.com/s/ves4gqmtz7acsux/Screenshot%202015-07-20%2010.58.32.png?dl=1)

Finally, we are ready to install IPython notebook using `pip` using the following command:

```bash
pip install "ipython[notebook]"
```

![](https://www.dropbox.com/s/k8brxy9dgik6ohw/Screenshot%202015-07-20%2011.00.00.png?dl=1)

###Configuring IPython

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
