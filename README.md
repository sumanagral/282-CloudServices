# 282-CloudServices
Homework #1 Active Directory
282 - Cloud Services

#### Team Members: 
Chinmayi Sunku
Suma Nagral
Sakshi Kekre

#### Github Reference Link: https://github.com/sumanagral/282-CloudServices
#### Database Link:  https://github.com/datacharmer/test_db

Step 1: Create an Active Directory
1.	Choose AWS Managed Microsoft AD
  

2.	Insert Directory DNS name
 
3.	Choose a password
 
4.	Choose VPC, and subnets in different availability zones
 
5.	Create the directory
 

6.	Wait for 20-45 minutes for the directory to be active.
 

 

Step 2: Launch EC2 Instance 
1.	Launch an EC2 instance with Microsoft OS image
 
 
 
 



Step 3: Connect to EC2 Instance using RDP client Microsoft Remote Desktop
1.	Download Microsoft Remote Desktop for Mac Users
2.	Get the Public IP4 address for EC2 Instance
3.	Get password for the EC2 instance by uploading the private keypair
4.	Connect to the Instance using the credentials

 
 
 



Step 4: Change Network Settings in Instance
1.	Go to Control Panel > Network and Internet > Network Connections
2.	Click on IP4 Properties
3.	Insert DNS addressed from the Directory Service 
 
 
	5. Go to Advance System Settings> Computer Name> Change> Input Domaiin
 


	6, Once we are connected to Active Directory, check the computer name
 

	7, Download and Install the admin tool 
 
	8. Open the Active Directory Users and Computers
 
 
Get Data from github and convert it into a CSV file: https://github.com/datacharmer/test_db

Run the following script in the PowerShell:
 Import-Module activedirectory
  
#Store the data from ADUsers.csv in the $ADUsers variable
$ADUsers = Import-csv C:\Users\admin\Desktop\assigmentactive.csv

#Loop through each row containing user details in the CSV file 
foreach ($User in $ADUsers)
{
	#Read user data from each field in each row and assign the data to a variable as below
		
	$Username 	= $User.userName
	$Password 	= $User.password
	$Firstname 	= $User.firstName
    $DisplayName= $User.displayName
	$Lastname 	= $User.lastName
    $email      = $User.emailAddress
    $groups     = $User.memberOf
    $description = $User.Description


	#Check to see if the user already exists in AD
	if (Get-ADUser -F {SamAccountName -eq $Username})
	{
		 #If user does exist, give a warning
		 Write-Warning "A user account with username $Username already exist in Active Directory."
	}
	else
	{
		#User does not exist then proceed to create the new user account
        $secureString = ConvertTo-SecureString -AsPlainText "April456#@123123421" -Force
        $quotedSecureString = $secureString
        #Account will be created in the OU provided by the $OU variable read from the CSV file
		New-ADUser `
            -SamAccountName $Username `
            -UserPrincipalName $email `
            -Name "$Firstname $Lastname" `
            -GivenName $Firstname `
            -Surname $Lastname `
            -Enabled $True `
            -DisplayName "$Lastname, $Firstname" `
            -Description $description `
            -AccountPassword $quotedSecureString `
	}
} 


 

 

 

Get User Count:
 
![image](https://user-images.githubusercontent.com/83566582/220047214-d6b80973-c7e4-416d-8360-2d27cc7a4da7.png)


