## SSL Bootcamp
> Learning SSL

### SSL Certificate
- SSL: Secure Sockets Layer
- Triggers the “little padlock” or green bar in a browser 
- Uses HTTPS for secure communication
- Certifies the ownership of a public key that’s used for encrypting data sent between a browser and a remote server 
	- I’ll talk more about public keys and the role that they play in cryptography later on.
- For now, just know that that's what it's certifying.
- Primary purpose is encryption

### Certificate Contents
- What’s inside one of these Certificates 
- Organization
- URL
- State, country
- Valid date range
	- Certificates aren't valid forever, they're certified just for a certain amount of time and they're certified by an issuer.
- Issuer
- So, all of this basic information as well as some technical details, is all going to be put into a file, a certificate file, and that's typically a file that ends in .crt or sometimes .cer.
- The content is an encoded version of the information, and it can be decoded.

### Symmetric Cryptography
- The first thing we need to learn about is something called symmetric-key cryptography. The idea is simply that we encrypt data using a password. So we take some data, we take a password, we put it into an algorithm, and what we get back is some encrypted data.
- We can then later come back and decrypt that data by taking the same password, putting it into an algorithm with our encrypted data, and what we get back is something that is decrypted, or readable. So the name symmetric key is a fancy way of saying that it uses the same key for both, both for encryption and for decryption.
- And our key is the password, and it works very much like a key would on a normal lock. 
- We can open it up, we can put data inside of it, and then we can close it and lock it with the key. Then it's encrypted and it stays locked and the only way to unlock it is by using that same key to open the door and get the data back out. That's symmetric-key cryptography.

### Public-Key Cryptography
- public-key cryptography works a little differently. It's also named asymmetric-key cryptography
Unlike symmetric key where it’s just one key, Instead, we have a pair of mathematically linked numbers.
- I won’t go into the math of how those two keys are generated, just know that at the end, you end up with two numbers that are linked together and we refer to them as the public key and the private key.
As the name suggests, the private key should always be kept secret and secure.
- But the public key can be shared widely. We don't care who has it.
The advantage of having two linked keys that are not the same, is that then data can be encrypted by the public, using our public key that's widely available,and it can only be decrypted using our private key, which only we have.
- Let's take that locked box example again. Let's imagine now that we have a box with two locks on it. On one side we have a lock for the public key, on the other side is a lock for the private key. Now the public side is unlocked. We can open it and the public can put a piece of data inside, and then they can lock it using our public key. Once it's locked, even another public key can't be used to open it again. Now it stays locked and we can submit it, even over the Internet.

### SSL Handshake
- Now that we understand public-key cryptography let’s talk about handshakes
The basic idea is that we're gonna use public-key cryptography in order to communicate. So let's walk through the steps.
- First, a browser is gonna send a request to a secure server. The server's gonna say hey, I see you have a secure request,let me send you back my SSL certificate, and I'm gonna send you back my public key and other data about my identity.
- The browser's gonna take a look at that and it's gonna decide whether that public key is something that it can trust. And it's gonna do that by confirming that the certificate is valid. 
If the browser decides that this is something that we can trust, then it can use that public key to encrypt a very long password and send it to the server.
- Then that can be transmitted securely, over the Internet, the server can then decrypt that data using its private key in order to retrieve that very long password.
Now, here’s the important part.
- The server and the browser now both possess the same password. Meaning they can use symmetric-key cryptography in future communications.

### So why do it that way? 
- Why switch over to symmetric-key cryptography when we were doing so well with the public-key cryptography? 
- Well, First it's because it allows us to have the benefits of both technologies. Public-key cryptography is great for being able to communicate privately in public, but the the algorithms that do that are quite slow. 
- So we use public key in order to swap a password, and then we switch over to symmetric key in order to get the speed advantage.
So there’s a one way hand shake, and a mutual handshake

### Certificate Authorities
- Certificate authorities are entities that issue digital certificates
- The way it works is that we're gonna go to one of these CAs, we'll provide them with some information about our organization.
- we're gonna tell them what URL we're gonna be using, then we're gonna give them a public key. And then sometimes we'll have to pay a fee as well. They'll then in turn validate that public key and give us back a certificate certifying the ownership of that public key. It's similar to notarizing an identity.
= The idea is that we have a trusted third party that's going to vouch for this public key. 

### Certificate Types
- The most common type of certificate is a single domain certificate. This would be a certificate where a public key is certified to belong to a single website 
- If we wanted it to apply to multiple subdomains, for example, app.edmamerto.com or sqlproxy.edmamerto.com, then we would need to get a wildcard certificate. It's exactly the same, it just allows us to have different subdomains.
- There's a third variation on this which is the multi-domain certificate. it's the same kind of certificate, but we're allowed to have one public key and one certificate that's able to be used for multiple domains. edmamerto.com, edmamertostg.com, 
There's a fourth variation on this which is the SAN certificate. That stands for Subject Alternative Name. Similar to multi-domain certificates, but they're primarily used Office communication environments.

### Validation Levels
- All three would use the same public key and the same type of encryption. What is different is the effort that the certificate authority makes to validate the ownership of that public key. Simply put, they charge more money to do more work in validating that an owner is who they are.
- Domain validation is the most common, and they're only certifying that the public key and the website domain name are related. Typically, the way this is done is an automatic email is issued to the website owner. So, it'll send it to whoever says that they own this website, and if they can receive that email and respond to it, then that's proof of ownership enough. 
- Organization validation has the same kind of validation as domain validation, but it also confirms the authenticity of the organization by checking business databases for articles of incorporation and confirming the physical address of the business. 
- For Extended validation, an actual person contacts the business at a published phone number. They may even speak to several people thereit just checks to make sure that someone answers the phone and says yes, this is the right business. 
	- One of the perks of having extended validation, though, is that many browsers will display it differently. They'll put a nice, big green bar at the top. That can make them look much more trustworthy.
	- The problem with this is Hackers started buying extended validation because they realized it makes websites look more trustworthy than they actually are, and for that reason, a lot of the browser makers have stopped doing that.