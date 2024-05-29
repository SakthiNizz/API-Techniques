## API-Techniques
Here are some tips/tricks/techniques of API VAPT

#IDOR:
1. If IDs pass in the plain text in URL/Request, observe and change it -> we may get some sensitive information.
2. If changes are refused, then intercept and observe the request, if the JWT token passes, decode and change ID then send -> if the signature isn’t verified then we may get some sensitive information.
3. If signature is verified, then we need to decode the signature part to create our own JWT token to get details. This can be done by Password cracking tools like - John the Ripper, hashcat, etc., that can help to decode and find the secret in the signature.
4. Sometimes ID and user details will be displayed in response, that ID may help to get sensitive information.

#Review JS to find Hidden endpoints:
1. Check source pages, from that we may get end point{keywords} of the web/API, pass that keyword in URL/request -> if URL isn't restricted then we may get some sensitive information.
2. If URL is restricted, then there are chances of having header and cookie verification, ie., some endpoint may access only by using such cookie/header, if you find any cookie and token in the source page then try to send it along with the request -> we may get some sensitive information.

#Login as admin:
#Reset option:
1. Register a new account and look for a password reset functionality. If the password reset link is shared in the response, try changing the known admin's password using that link. -> we may get admin control.
2. If no link is shared in the response, check the version of API, changing that version like, v2 to v1 -> we may get that reset link to take admin control.
3. If you see some restricted endpoints, try changing the versions passing in the requesting -> like changing V2 to V1, and see if the restricted pages are accessible. 

#Sign-in option:
1. Create an account with the name of a known admin mail ID, you will get an error like username exists -> try with the same mail ID by adding an extra domain like @sample.com along with the known admin mail, it will accept only when client and server side is not validated. -> admin@hacker.com@sample.com
2. If @ is validated, then try it with some other special character like (. , $ % etc). -> admin@hacker.com.sample.com
3. If special characters are validated, then try it with alphabets -> admin@hacker(a)com
4. If both the client and server side are validated, make a try with case sensitive -> admin@hac(K)er.com
5. If you see any hashed passwords in source pages/response/darkweb, try to decrypt it or intercept the request then send that hashed password along with it -> we may get admin control.
6. If the request does not accept a hashed password, then look into the source pages, we may get to know the method/code that is used for encryption and decryption, if the hashed password is encrypted then decrypt and use it -> we may get admin control.

#Payment:
1. When we try to make a payment, observe the link in the URL/request with the post-success payment link, and copy and send the URL for successful payment without paying. 
2. Intercept the payment request and observe it, if the amount passes then change it to 0 and send it.
3. If the amount is not passed but the quantity is passed, then change the quantity of the product to 0, 0.001, or some negative values.
4. If we have a voucher discount, observe the request while applying. Still, it is not validated properly on the server side, so we send the same request for multiple times to get multiple discounts, stop when we reach the minimum amount, and make payment.
5. If the server side is validated with cookies, try it on the client side by applying the same voucher again and again for the same product.
