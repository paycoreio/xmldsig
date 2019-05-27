### It is fork of https://github.com/greenter/xmldsig, with minor changes to sign requests to UPC.

# XmlDSig - Greenter

This library is used to sign requests according to UPC rules.

The certificate in .PEM is required, you can use the following example to [convert the .PFX certificate to the required format](https://github.com/paycoreio/xmldsig/blob/master/CONVERT.md).


## Install

Install using Composer from [packagist](https://packagist.org/packages/paycoreio/xmldsig).  

```bash
composer require renay/xmldsig
```

## Example
```php

use Greenter\XMLSecLibs\Sunat\SignedXml;

require 'vendor/autoload.php';

$xmlData = file_get_contents('path-dir/20600995805-01-F001-1.xml');
$certPath = 'path-dir/SFSCert.pem'; // Convertir pfx to pem 

$signer = new SignedXml();
$signer->setCertificateFromFile($certPath); // You can also use $signer->setCertificate ($certData);

$xmlSigned = $signer->signXml($xmlData);

header('Content-Type: text/xml');
echo $xmlSigned;
```

**Resultado:**  

Before:
```xml
<ext:UBLExtensions>
    <ext:UBLExtension>
        <ext:ExtensionContent></ext:ExtensionContent>
    </ext:UBLExtension>
</ext:UBLExtensions>
```

After:
```xml
<ext:UBLExtensions>
    <ext:UBLExtension>
        <ext:ExtensionContent>
            <ds:Signature Id="SignIMM">
                <ds:SignedInfo>
                    <ds:CanonicalizationMethod Algorithm="http://www.w3.org/TR/2001/REC-xml-c14n-20010315"/>
                    <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>
                    <ds:Reference URI="">
                    <ds:Transforms>
                        <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
                    </ds:Transforms>
                    <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>
                    <ds:DigestValue>IwJuNQGQaHmmm3iv2jj8JDv70Ow=</ds:DigestValue>
                    </ds:Reference>
                </ds:SignedInfo>
                <ds:SignatureValue>
                nLaghokzMNrmrfPnbIg9b........wzZ2CgLTVjWQUAQ4wDAYDVQQIEwVNYWluZTE1UiLFwZXXXPUlf2o=
                </ds:SignatureValue>
                <ds:KeyInfo>
                    <ds:X509Data>
                        <ds:X509Certificate>
                        MIIFhzCCA3OgAwI......MIIEVDCCAzygAwIBAgIJAPTrkMJbCOr1MA0GCSqGSIb3DQEBBQUAMHkxCzAJBgNVBAYTAlVTVQQIEwVNYWluZTEgMOiRJ00nE=
                        </ds:X509Certificate>
                    </ds:X509Data>
                </ds:KeyInfo>
            </ds:Signature>
        </ext:ExtensionContent>
    </ext:UBLExtension>
</ext:UBLExtensions>
```
