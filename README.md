# 282-CloudServices
## Homework #1 Active Directory
## 282 - Cloud Services

#### Team Members: 
Chinmayi Sunku
Suma Nagral
Sakshi Kekre

#### Github Reference Link: https://github.com/sumanagral/282-CloudServices
#### Database Link:  https://github.com/datacharmer/test_db

Step 1: Create an Active Directory
1.	Choose AWS Managed Microsoft AD
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220047798-07e5d308-b4ce-4c1a-86ff-43e608a946ec.png">

  

2.	Insert Directory DNS name
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220047837-94e54803-830e-41a3-acea-3d3d99e4beca.png">

 
3.	Choose a password
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220047863-fe54a5af-55de-41ac-a489-150b74169ca2.png">

 
4.	Choose VPC, and subnets in different availability zones
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220047903-f19850cf-e951-48e2-b852-37c0dddd318a.png">

 
5.	Create the directory
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220047927-702ddb4a-eb1a-48c2-a9ef-388df737f2d2.png">


6.	Wait for 20-45 minutes for the directory to be active.
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220047976-b270b19e-7fc2-4531-952a-47b73689a5ea.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220047994-41d887f1-207d-4c28-b402-de37e14fb62e.png">

 

 

Step 2: Launch EC2 Instance 
1.	Launch an EC2 instance with Microsoft OS image
 
 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048024-fd04ce3b-0555-4045-a26f-9d36be195b4f.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048052-7a537d31-39c2-416c-9c45-1341f625b178.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048072-35360852-b77a-4acb-afa0-f13276ca7a25.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048090-49515e8e-fcfd-4b56-81db-e886bce6c650.png">

 



Step 3: Connect to EC2 Instance using RDP client Microsoft Remote Desktop
1.	Download Microsoft Remote Desktop for Mac Users
2.	Get the Public IP4 address for EC2 Instance
3.	Get password for the EC2 instance by uploading the private keypair
4.	Connect to the Instance using the credentials

 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048143-d1d32b36-f9cf-4022-9ef8-3c33cd7a8c51.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048164-aefbe7e0-8947-4ee5-9322-c634c269fa10.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048184-bb179d9b-261c-48e0-afc7-c4177440fc9b.png">

 



Step 4: Change Network Settings in Instance
1.	Go to Control Panel > Network and Internet > Network Connections
2.	Click on IP4 Properties
3.	Insert DNS addressed from the Directory Service 
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048226-111f4331-b0d7-480f-a060-182d0e9769b7.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048259-112c197b-50de-4f86-9149-ae10a9199c52.png">

 
	5. Go to Advance System Settings> Computer Name> Change> Input Domaiin
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048290-3ac00253-82a1-4fce-a97d-a915edebc04c.png">



	6, Once we are connected to Active Directory, check the computer name
 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048332-db36550d-71f8-4cd4-a231-1eae017a2f5f.png">


	7, Download and Install the admin tool 
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048365-d886266c-7844-47a6-8a76-4275ba34808b.png">

 
	8. Open the Active Directory Users and Computers
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048391-857ba648-9df5-4c50-a606-420990d297a2.png">

 
 
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


 

 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048458-d8d1046b-ecb8-4a1b-90c7-0521ab2b3984.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048482-ac6e3e73-cd47-4ea0-bd22-5ee51e714b75.png">
<img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048516-48f2186b-0a70-4efb-9ba9-441e0ab48956.png">


 

Get User Count:


 <img width="468" alt="image" src="https://user-images.githubusercontent.com/83566582/220048571-4a360074-7480-4cc1-b4a4-003fddb8e098.png">



