## Deploying a Static Website to AWS S3 with HTTPS using CloudFront.
![static website](https://user-images.githubusercontent.com/119833411/242142634-39d8fe8b-cff0-4ee3-858d-d770f1f3f362.jpg)
### Prerequisites.
* **You should have a domain name already purchased to link with your Static Website.**
* **Some Web content to display on your domain.**
### By the end of this post you will be able to: 
* **Create an S3 bucket and Configure it for static website hosting.**
* **Create a record in Route 53.**
* **Create a CloudFront distribution and link it with your custom domain.**
* **Create Certificates in AWS Certificate Manager.**
* **Then Finally Link the CloudFront CDN, S3, Custom Domain and SSL Certificate via Route 53 To securly access your webpage.**
### I have created a simple index.html file.
![1](https://user-images.githubusercontent.com/119833411/242149802-8f277b18-7a3e-40c7-b329-ef21825c754a.jpg)
![2](https://user-images.githubusercontent.com/119833411/242150489-8dce2ec3-92d0-4abe-be71-c2c288f42e81.jpg)

**index.html**
```
<html>
<body>

<h1> "Be A Better Version of Yourself".</h1>

<img src="image/image.jpg" alt="Be-a-Better-Version-of-Yourself">

</body>
</html>
```
### Your webpage will look like this.
![3](https://user-images.githubusercontent.com/119833411/242150783-4478a115-825d-4f22-a42a-74415bcbaa0a.jpg)

### Open Amazon S3 in your aws console and click on Create bucket.
![4](https://user-images.githubusercontent.com/119833411/242151541-f1ca50ee-2e9c-4113-ba35-5c21f22b35ae.jpg)

### Name your bucket it should be unique, so i am giving my domain name to s3 bucket.
![5](https://user-images.githubusercontent.com/119833411/242153573-11bb1383-ddaa-40a9-ade8-b355ddfd3373.jpg)

### In Block Public Access settings for this bucket, uncheck "Block all public access" and select the acknowledge at the end.
![6](https://user-images.githubusercontent.com/119833411/242153980-ace15321-3257-4edf-a4c6-5017c1ca917b.jpg)

### Rest of all settings are fine, click on create bucket at bottom.
![7](https://user-images.githubusercontent.com/119833411/242154263-a7979b5b-ad59-469c-9922-5780c0966441.jpg)

### Once done you should be able to see your bucket.
![image](https://user-images.githubusercontent.com/119833411/242154593-b25a5c71-bbbf-4345-99b6-c088beffa577.png)

### Now go inside the s3 bucket and upload the web content you have.
![9](https://user-images.githubusercontent.com/119833411/242155221-9f46764e-bbc4-4775-88c4-3a2300b9a08b.jpg)

![10](https://user-images.githubusercontent.com/119833411/242155482-46d0b1b9-1c58-48d6-85cb-aa369cb34d15.jpg)

**Select upload once your done.**
![11](https://user-images.githubusercontent.com/119833411/242155723-6ef912b6-225c-4a0a-b917-8d67a8d66fd6.jpg)

**Once files are uploaded you can see the status "Succeeded".**
![12](https://user-images.githubusercontent.com/119833411/242155912-4dfcb765-5a39-40d1-8c20-4e33be1c893c.jpg)

### Now go to "Properties" tab and enable "Static website hosting" enter index.html in index document section and save changes.
![13](https://user-images.githubusercontent.com/119833411/242156447-d1936eeb-02c8-4275-a83d-5134d182cd9f.jpg)

![14](https://user-images.githubusercontent.com/119833411/242156939-0f4c1cc6-df56-432d-9d9c-89ce164d588f.jpg)

![15](https://user-images.githubusercontent.com/119833411/242156950-c2b93672-05dc-4a47-8a22-f3a7efb662a5.jpg)

![16](https://user-images.githubusercontent.com/119833411/242157266-710060ea-2341-404c-8b8e-83bf8f9dd295.jpg)

### Now go to "Permissions" tab and enter bucket policy and save chnages.
![17](https://user-images.githubusercontent.com/119833411/242157791-df42a6a7-6674-4658-b5ef-4df007724765.jpg)

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "s3:GetObject"
            ],
            "Resource": [
                "arn:aws:s3:::learnwithsampath.world/*"
            ]
        }
    ]
} 
```
![18](https://user-images.githubusercontent.com/119833411/242158364-42b749fe-2c59-49dd-873f-d2b15f629352.jpg)

**This policy will grants public read access to all objects within the "learnwithsampath.world" S3 bucket.**

### Now you can see bucket access as "public" previously it was "object can be public". 
![19](https://user-images.githubusercontent.com/119833411/242158954-8cc24e1c-0eb2-4032-8a93-1fe1abc973a8.jpg)

### Now open index.html in s3 bucket.
![image](https://user-images.githubusercontent.com/119833411/242159674-d34ecd88-2889-428c-9b46-6b08e92299b1.png)

### You can see your website, but it is not your custome domain url.
![21](https://user-images.githubusercontent.com/119833411/242159910-ac0b9d1a-2186-414d-aedd-cc5eef1cb19c.jpg)

**Now we have to redirect the click to learnwithsampath.world to this s3 bucket.**

### I have already purchased a domain name "learnwithsampath.world" with godaddy Internet domain registrar.
![22](https://user-images.githubusercontent.com/119833411/242161035-c1fec579-6bff-4198-8ea0-4fb8f2714ddc.jpg)

### Now go to Route 53 Dashboard and click on "create hosted zone".
![23](https://user-images.githubusercontent.com/119833411/242161516-8a8aab5f-1fd2-4dda-9d01-50f3b20998b0.jpg)

### Enter your Domain name, type "public hosted zone" is fine click "create hosted zone"
![24](https://user-images.githubusercontent.com/119833411/242162118-c11e4432-8e19-423d-9ba9-8b2c8bd60859.jpg)

### Once your done, you should see one hosted zone created in your Route53 console.
![25](https://user-images.githubusercontent.com/119833411/242162870-4b2ae54b-0007-4945-a713-1df515f5155b.jpg)

![26](https://user-images.githubusercontent.com/119833411/242163018-8e3cc4cf-6e11-4f31-92da-655aa82d7e16.jpg)

# To manage godaddy domain name using AWS Route-53 we have to copy nameservers created in Route53 to godaddy.
![27](https://user-images.githubusercontent.com/119833411/242164487-8b58f694-fe61-46b2-af59-8f65bd486637.jpg)

![28](https://user-images.githubusercontent.com/119833411/242164665-6c36a29b-b298-4152-9762-1e59616a5d36.jpg)

![29](https://user-images.githubusercontent.com/119833411/242164988-0e157ab2-3905-4e85-beda-966ee13d43a2.jpg)

![30](https://user-images.githubusercontent.com/119833411/242165168-407a7e28-2112-4101-a7f6-c6125e6d396c.jpg)

![31](https://user-images.githubusercontent.com/119833411/242165616-5ac70a2e-a0cb-4b3c-92de-f93e1471e676.jpg)

![32](https://user-images.githubusercontent.com/119833411/242165728-30d4b61a-330f-4783-affb-9eda907ccf62.jpg)

**Now you can manage your domain from Route-53 by creating records in it.**

### Now lets create a simple record in Route-53.
![33](https://user-images.githubusercontent.com/119833411/242166423-da1e2d75-49be-4311-a2ae-81c791d38da3.jpg)

### Select "Alias", Route traffic to "S3 website endpoint" and select location of your s3 bucket.
![34](https://user-images.githubusercontent.com/119833411/242167474-469075cc-182a-49ac-a797-cbb92a350dee.jpg)

### Now you can access your website on your domain learnwithsampath.world 
![34](https://user-images.githubusercontent.com/119833411/242168105-b4965d51-35db-4e55-87ec-3855536d4831.jpg)

**But it's still "Not secure" its http://learnwithsampath.world/  not https://learnwithsampath.world/**

### Now lets make it using AWS Certificate Manager, go to AWS Certificate Manager console.

### Note: You have to select the AWS North Virginia region(US-East-1). As CloudFront recognizes only this region as it's ACM certificates.

![35](https://user-images.githubusercontent.com/119833411/242169673-45dc927e-f202-4515-8178-4fe3eae5ce8d.jpg)

### Select Certificate type "Request a public certificate" and click on next.
![36](https://user-images.githubusercontent.com/119833411/242170248-57c1cc2b-267b-4392-a25a-68c39e036ad0.jpg)

### Provide your Domain names, select a Validation method rest all are fine click on request.
![37](https://user-images.githubusercontent.com/119833411/242171434-2d422bf7-5d8a-4175-8d40-4a5decc8844a.jpg)

![38](https://user-images.githubusercontent.com/119833411/242171760-02f2c69a-bad2-45df-901d-1ea8b1580c5f.jpg)

### Once you click request, you need to add the given record in Route 53 by clicking on Create records in Route 53.
![39](https://user-images.githubusercontent.com/119833411/242175198-a340ac7d-a86e-4e33-83d2-d1f0d0497d4c.jpg)

![40](https://user-images.githubusercontent.com/119833411/242175460-1eb0d149-d59b-4fc5-b59f-ca948611c17f.jpg)

### It will take some time for records to be updated for me it took few seconds.
![41](https://user-images.githubusercontent.com/119833411/242175864-db0d8c5e-004e-4486-bb69-bdb0ecfae285.jpg)

![42](https://user-images.githubusercontent.com/119833411/242176452-715a4712-3fdb-4ca8-99a6-c3a3c2779f76.jpg)

### After the certificate is issued, we can set up a CloudFront distribution. Open Amazon CloudFront in aws console and click on "Create a CloudFront distribution"
![43](https://user-images.githubusercontent.com/119833411/242176874-3571daf9-d0d1-43b0-a241-58cb4a34735c.jpg)

### Select Origin domain from drop down. 
![44](https://user-images.githubusercontent.com/119833411/242177397-de406ee5-fdc5-44ce-a61f-a3799f9d6ddf.jpg)

![45](https://user-images.githubusercontent.com/119833411/242177744-ede4baa6-bf49-4c17-9318-5a1adb735486.jpg)

![46](https://user-images.githubusercontent.com/119833411/242178444-cc3f85bf-c590-42d1-90ee-062d547e31df.jpg)

### In Viewer protocol policy select to Redirect HTTP to HTTPS 
![47](https://user-images.githubusercontent.com/119833411/242178619-de493dab-0c87-4227-87bd-431805fb5b5f.jpg)

### Add Custom SSL certificate.
![48](https://user-images.githubusercontent.com/119833411/242179197-91a7ea3c-cfed-4e0c-9679-081e0341561d.jpg)

### Select Web Application Firewall (WAF). You can select as per your requirment.
![50](https://user-images.githubusercontent.com/119833411/242180288-87ffd010-5aed-45ed-b537-cbcb474ea5f6.jpg)

### Mention Default root object and Click on Create distribution .
![49](https://user-images.githubusercontent.com/119833411/242179529-7a123efa-2717-43d4-af1a-6072fea33718.jpg)

![51](https://user-images.githubusercontent.com/119833411/242180681-ddbfe53f-a96f-4ef4-8516-6c7bc6f8092e.jpg)

**It will take few minutes.
### you can check with Distribution domain name.
![52](https://user-images.githubusercontent.com/119833411/242181625-516c84f8-d90f-465b-8b1b-b7633c58fffe.jpg)

![53](https://user-images.githubusercontent.com/119833411/242181910-b38c4b07-23aa-4a08-8999-e5e8b669fbd2.jpg)

### Now we can point our domain to Distribution domain name in Route 53.
**Previously we have created A-record in Route 53 pointing to "S3 website endpoint" you delete/edit that A-record to point to Distribution domain name and save record.**

![55](https://user-images.githubusercontent.com/119833411/242217147-6c3c1dc6-b2f5-4013-8d4c-b638d6fbcedf.jpg)

**Wait for some time for changes to apply**

### Now you can access the website securely over "https".
![56](https://user-images.githubusercontent.com/119833411/242217926-0add7d25-7340-4801-b616-148285cf1aea.jpg)

**Even you time http://learnwithsampath.world/ also it will redirect to https://learnwithsampath.world/**

## We have successfully Deployed a Static Website to AWS S3 with HTTPS using CloudFront.

**Thank you for reading this post! I hope you found it helpful. If you have any feedback or questions,Please connect with me on LinkedIn at https://www.linkedin.com/in/sampathsivakumar-boddeti-1666b810b/. Your feedback is valuable to me. Thank you!**





