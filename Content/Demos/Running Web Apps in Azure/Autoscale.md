
# Autoscale in Azure
A demo that show the autoscale features in Azure

## Setup Azure for auto scale
Login to azure at https://portal.azure.com

Open the 'MSWebDayScale' website

Note that it is currently at 1 instance

All Settings > Scale Out (App Service Plan) > Scale By > CPU
* Default up to 10 Instances
* Target 15 > 25 %
	
Save

## Setup a web test
Open a new instance of Visual Studio

New > Test > Web Performance and Load test project > OK

Right click WebTest1 and AddRequest

In the new node, open the Properties window and update the URL to point to the azure site
* http://mswebdayscale.azurewebsites.net/
	
Right-click WebTest1 and AddLoop

Select the ForLoop rule
* Context Parameter Name: `Iterator`
* Terminating Value: `1000`
* Increment Value: `1`
* Select the website for first and last items
* OK

## Setup a load test that uses the web test
Right click project > Add > New > Load Test
* On premise (runs from local machine)
* Duration: `10 minutes`
* `Do not use think times`
* User count: `1000`
* `Sequential order`
* Add WebTest1
* Network Mix: Default
* Counter Sets: Default
	
Save

Right-click > Run Load Test

In Test Results, right-click > View Run
* Page response time gradually increased, then drops as another server is added
* Show how the average page time changed as the test progressed

## Show that Azure site has auto scaled
Login to azure at https://portal.azure.com

Open the MSWebDayScale website

Show that instance count is now 2 or more
* You may have to wait several minutes before the azure portal has caught up
	
		

