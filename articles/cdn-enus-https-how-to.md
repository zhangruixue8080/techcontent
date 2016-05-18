<properties linkid="dev-net-common-tasks-cdn" urlDisplayName="CDN" pageTitle="How to use CDN—HTTPS" metaKeywords="Azure CDN, Azure CDN, Azure blobs, Azure HTTPS, CDN HTTPS, Azure caching, Azure add-ons, CDN, CDN acceleration, CDN service, mainstream CDN, multi-scenario acceleration, free CDN, CDN website acceleration, website acceleration, web page acceleration, static acceleration, download acceleration, VOD acceleration, live streaming acceleration, cloud services, storage account, cache refresh, return to source, cloud acceleration, acceleration results, node, traffic, CNAME, bandwidth, connection speed, anti-theft chain, HTTPS acceleration, low-cost bandwidth, access acceleration, CDN cache, storage account, cloud services, website, media services, ICP record number, ICP number, ICP, cache refresh, content prefetching, log download, CDN help file, CDN technical documentation" description="Learn how to create HTTPS CDN acceleration type." metaCanonical="" services="" documentationCenter=".NET" title="" authors="" solutions="" manager="" editor="" />
<tags ms.service="cdn" ms.date="" wacn.date="5/3/2016" />

# Azure CDN HTTPS acceleration service


## Request access
The Azure content delivery network (CDN) HTTPS acceleration service is not available if you use an Azure free account.

1. **To apply for access:** Please contact the [Azure technical support team](https://www.azure.com/support/contact). You will need to provide the Azure subscription ID that you want to use the HTTPS acceleration service for.


2. **Self-service creation:** After the Azure CDN team receives and approves your access request, the team will start the HTTPS acceleration service for the Azure subscription that you provided. You can then sign in to the Azure Management Portal to complete the self-service creation process. Refer to the instructions for the self-service creation process.

    ![][1]


3. **SSL certificate application and configuration:** After your configuration request is received, the Azure CDN will apply for an SSL certificate on your behalf. See the notes at the end of this article for details about certificate types.
    > **Notes**

    > The application and configuration process for this certificate takes around ***five working days***. You will also need to cooperate during the application process so that the certificate issuer can confirm ownership of the domain name. Details about this confirmation process are given later in this article.

4. **When the configuration is finished and takes effect:** After the configuration process is complete, you can complete the final setup of the canonical name (CNAME) record as you would to create services with other types of acceleration. Finally, you can complete any other associated management and configuration tasks through the unified Azure CDN Management Portal.


## Self-service creation process
1. Once you have finished creating the HTTPS acceleration type on the Azure Management Portal, press the **Management** button shown in the following image to jump to the Azure CDN Management Portal to complete the subsequent access procedure.

	![][2]

	![][3]

2. In the Azure CDN Management Portal, under **Domain name management**, select the HTTPS acceleration domain name that you need to configure, and then select **View HTTPS configuration status**.

	![][4]

3. You will then be able to see the five steps that are required to complete the whole HTTPS access configuration process. In Step 2: “Submit certificate application,” the interface prompts you to provide the following additional information:

	**Estimated bandwidth**: Specify the estimated peak bandwidth that's required.

	**Estimated average file size**: Specify the estimated average size of the files for which cache acceleration is required.

	**Acceleration demand time**: Specify whether the demand for HTTPS acceleration is long term or short term.

	**Access port for client access to CDN node**: Specify how the client that you want to enable will access the CDN node.

	1) Only enable HTTPS access (HTTP access is forbidden)

	2) Enable HTTP and HTTPS access

	3) Always redirect HTTP access to HTTPS access

	**Return-to-source port for CDN node access to the source station**: Specify how the CDN node can achieve return-to-source access.

	1) Only use HTTP to return to source

	2) Only use HTTPS to return to source

	3) Use both HTTP and HTTPS to return to source

	**Test URL**: Enter a URL that can be used subsequently to check access. You must make sure that this URL on the source station is accessible.

4. After you have filled out all the relevant information for the previous step, press the **Confirm** button to complete the operation. To view the HTTPS configuration status again later:

	![][5]

	After the Azure CDN service provider’s backend confirms all the submitted information and has also submitted the official certificate application to the certificate issuer, you can proceed to the interface for Step 3 as shown in the following image:

	![][6]

5. The next step, which is the most critical step in the certificate application process, is when the user needs to confirm ownership of the domain name. As the Azure CDN service provider submits the certificate application to the certificate issuer on the user’s behalf, the certificate issuer will confirm domain name ownership with the user during this process. In order to confirm ownership, **the domain name owner must complete final confirmation in the manner that's specified in the email sent by the domain name issuer**.

	There are two methods to receive the email:

	**Default method**: After the certificate issuer receives the request, the certificate issuer will send the domain name confirmation email by default to the email address that's associated with the acceleration domain name as soon as possible. See the previous image for details. If you choose to obtain the confirmation email by this method, you can press the **Confirm** button to go directly to the next step.
		
		

	**DNS TXT method**: If you are unable to sign in to the email account for the default method to complete confirmation of the domain name ownership, you can complete the confirmation process **by creating a DNS TXT record** as shown in the following image. This method may take a little longer to complete.

	![][8]

	After you have created the DNS TXT log, you can check it by using the following commands at a Windows command prompt: nslookup -qt=txt www.cdn.test.com

	![][9]

	You can then sign in to the email account that's specified in the DNS TXT log, just as you would for the default method, in order to complete the subsequent domain name ownership verification process.

	After you have successfully created the corresponding DNS TXT log in this step, press the **Confirm** button and proceed to the next step.

	![][10]

	

6. The user can complete the domain name ownership verification process by accessing the email.
	
	After the Azure CDN backend confirms the method that you have chosen to receive the confirmation email, you will see the following interface:

	![][11]

	At this point, you can proceed to the corresponding email account to finish the verification of domain name ownership.

	Email subject: Please validate ownership of your domain www.cdn.test.com -- DigiCert order 00123456

	Email body:

	![][7]

	You need to click on the confirmation link in the email to complete confirmation of domain name ownership.

	After this, you need to click on **Complete** in the interface for Step 4 to finally complete confirmation of the entire domain name.

7. Next, the entire configuration process will move into the final step, as shown in the following image:

	![][12]

	In this step, after the Azure CDN backend receives the corresponding certificate from the certificate issuer, it will take a certain amount of time to complete the final configuration tasks. After this, you will see the final completion interface:

	![][13]

	Finally, you can complete the CNAME configuration process through your domain name service provider just as you would to create accelerated domain names with other acceleration types. You direct the user-defined accelerated domain name to the CDN domain name that's provided by the Azure CDN platform, which should have an extension similar to **.mschcdn.com**. This completes the configuration.




## Notes

### About the SSL certificates
The SSL certificate type that is used is a SAN multi-domain name certificate (SAN/UCC SSL):

Subject alternative name (SAN) certificates are also known as unified communication certificates (UCC). With SAN SSL certificates, you can add multiple domain names or server names that need protection within the same certificate. This feature provides a huge amount of flexibility. You can create an SSL certificate that is not only easy to use and install, but it is also more secure than wildcard SSL certificates. The certificate is also perfectly suited to your server security requirements.

For more information about certificate issuers, see the [Digicert website](https://www.digicert.com).
	
The Azure CDN will apply for, install, and maintain the SSL certificate on your behalf.

### Information about charges
Information about charges for CDN HTTPS acceleration nodes and other CDN acceleration nodes, can be viewed through the corresponding [Azure account portal](https://account.windowsazure.com) and is summarized below the corresponding Azure subscription.


### CDN HTTPS pricing
The Azure CDN HTTPS acceleration service is currently included in the premium version. See the [official Azure website](https://www.azure.com/home/features/cdn/#price) for details about pricing and charges.


<!--Image references-->
[1]: ./media/cdn-https/h001.png
[2]: ./media/cdn-https/h002.png
[3]: ./media/cdn-https/h003.png
[4]: ./media/cdn-https/h004.png
[5]: ./media/cdn-https/h005.png
[6]: ./media/cdn-https/h006.png
[7]: ./media/cdn-https/h007.png
[8]: ./media/cdn-https/h008.png
[9]: ./media/cdn-https/h009.png
[10]: ./media/cdn-https/h010.png
[11]: ./media/cdn-https/h011.png
[12]: ./media/cdn-https/h012.png
[13]: ./media/cdn-https/h013.png

<!---HONumber=Acom_0503_2016_CDN-->
