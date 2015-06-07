# Configuring Hortonworks Sandbox on Azure

Start by logging into the Azure Portal with your Azure account:[https://portal.azure.com/](https://portal.azure.com/)

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/01.png)  


Navigate to the **‘MarketPlace’ ** 

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/04.jpeg)  


Search for Hortonworks. Click on the Hortonworks Sandbox icon.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/08.png)  


To go directly to the Hortonworks Sandbox on Azure page navigate to [http://azure.microsoft.com/en-us/marketplace/partners/hortonworks/hortonworks-sandbox-sandbox22/](http://azure.microsoft.com/en-us/marketplace/partners/hortonworks/hortonworks-sandbox-sandbox22/)

This will launch the wizard to configure Hortonworks Sandbox for deployment.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/10.png)  


Note the highlighted text in the instructions above. You will need to note down the `hostname` and the `username`/`password` that you enter in the next steps to be able to access the Hortonworks Sandbox once deployed.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/12.png)  


You can change these configurations if you want, but the defaults work well. I usually change the location to the datacenter to where my preexisting Azure resources are or the one closest to me.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/14.png)  


Click `Buy` if you agree with everything on this page.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/16.png)  


At this point it should take you back to the Azure portal home page where you can see the deployment in progress.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/18.png)  


Once the deployment completes you will see this page with configuration and status of you VM. Again it is important to note down the DNS name of your VM which you will use in the next steps.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/20.png)  


If you scroll down you can see the Estimated spend and other metrics for your VM.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/22.png)  


Let’s navigate to the home page of your Sandbox by pointing your browser to the URL: `http://<hostname>.cloudapp.net:8888` , where `<hostname>` is the hostname you entered during configuration.

If you are doing it for the first time, it will take you to the registration page.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/24.png)  


Once you register, you will see the homepage of your Sandbox.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/26.png)  


By, navigating to port 8000 of your Hortonworks Sandbox on Azure you can access the Hue interface for your Sandbox.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/28.png)  


Enable Ambari by clicking on the `Enable` button. Ambari is crucial for managing your HDP instance.

![](http://hortonassets.s3.amazonaws.com/tutorial/azure-sandbox/30.png)  


If you want a full list of tutorial that you can use with your newly minted Hortonworks Sandbox on Azure go to [http://hortonworks.com/tutorials](http://hortonworks.com/tutorials).
