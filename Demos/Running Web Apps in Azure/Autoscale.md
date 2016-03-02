
# Autoscale in Azure
A demo that show the autoscale features in Azure

## Setup Azure for auto scale
Login to azure at https://portal.azure.com

Open the 'MSWebDayScale' website

Note that it is currently at 1 instance

All Settings > Scale > Scale By > CPU
* Default up to 10 Instances
* Target 15 > 30 %
	
Save

## Setup a web test
Open a new instance of visual studio

New > Test > Web Performance and Load test project > OK

Right click WebTest1 and AddRequest

In the new node, open the Properties window and update the URL to point to the azure site
* http://mswebdayscale.azurewebsites.net/
	
Right-click WebTest1 and AddLoop

Select the ForLoop rule
* Context Parameter Name: Iterator
* Terminating Value: 1000
* Increment Value: 1
* Select the website for first and last items
* OK

## Setup a load test that uses the web test
Right click project > Add > New > Load Test
* Do not use think times
* User count: 250
* Sequential order
* Add WebTest1
* Network Mix: Default
* Counter Sets: Default
* Duration: 5 minutes
* Use default test location
	
Save

Right-click > Run Load Test

Show the test report
* Page response time gradually increased
* Show how processor time was managed as the test progressed

## Show that Azure site has auto scaled
Login to azure at https://portal.azure.com

Open the MSWebDayScale website

Show that instance count is now 2
	
		

