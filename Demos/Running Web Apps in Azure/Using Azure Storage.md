
# Using Azure storage
A demo that show the use of Azure storage for storing static files in a website

## Show image being served from server
Run the site locally

F12 > DOM Explorer

Select image and show local path (/index_files/titlewebplatform.jpg)
	
## Show Azure Storage account
https://portal.azure.com

Create a new storage account
* Explain Type and redundancy options
* Use existing MSWebDay resource group
* North Europe
* OK
	
Add a BLOB container 
* Blob Service > Containers
* Called 'Static'
* Set Access type to 'Blob'

## Cloud Explorer
Open VS Cloud Explorer (view > Other Windows or search)

Navigate to 'Static container

Open 'Blob Container Editor'

Upload /index_files/titlewebplatform.jpg  to the container

Capture the blob URL
* Right-click > copy URL
	
## Set HTML to use storage file
Paste the URL of the blob into src for image

Run and use f12 to show new path

Do a network trace

Show how quicker the image loads compared to others
* Use size to show they are the same