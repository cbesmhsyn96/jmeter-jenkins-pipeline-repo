Issue:
Due to the CSP (Content Security Policy) restrictions, the report does not render properly and appears without data.

Solution:
The following workaround resolves the issue, but introduces security vulnerabilities:

```
nohup /pathHere/bin/java \
-Dhudson.model.DirectoryBrowserSupport.CSP="default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self' data:;" \
-Dmail.smtp.starttls.enable=true \
-jar /opt/homebrew/opt/jenkins-lts/libexec/jenkins.war \
--httpListenAddress=127.0.0.1 \
--httpPort=8080 > jenkins.log 2>&1 &
```

Introduced vulnerabilities:

script-src 'unsafe-inline'
→ Allows inline JavaScript execution. This is normally blocked to prevent XSS attacks.

style-src 'unsafe-inline'
→ Allows inline CSS. This can also pose security risks.

img-src 'self' data:
→ Allows images embedded as base64 data URIs.

font-src 'self' data:
→ Allows external and base64-encoded fonts.

Security impact:

This CSP configuration is necessary to properly display styling, fonts, and embedded assets in the report.

However, it disables key security protections that CSP is meant to enforce.

>HTML Report Publisher can be used for report integration, but the data does not display correctly in the HTML report.
As a solution, using the command above resolves the display issue, but introduces the aforementioned security vulnerabilities.
For this reason, the approach I prefer is to send a notification email if the test fails after execution on Jenkins (using the Mailer Plugin integrated with Jenkins).

=====================================================================================================================================================

Sorun:
CSP (Content Security Policy - İçerik Güvenlik Politikası) kısıtlamaları nedeniyle rapor düzgün şekilde görüntülenmiyor ve veriler eksik görünüyor.

Çözüm:
Aşağıdaki geçici çözüm sorunu gideriyor, ancak bazı güvenlik açıklarına yol açıyor:
```
nohup /pathHere/bin/java \
-Dhudson.model.DirectoryBrowserSupport.CSP="default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' data:; font-src 'self' data:;" \
-Dmail.smtp.starttls.enable=true \
-jar /opt/homebrew/opt/jenkins-lts/libexec/jenkins.war \
--httpListenAddress=127.0.0.1 \
--httpPort=8080 > jenkins.log 2>&1 &
Oluşan güvenlik açıkları:
```

script-src 'unsafe-inline'
→ Sayfanın satır içi (inline) JavaScript çalıştırmasına izin verir. Bu, genellikle XSS saldırılarına karşı engellenir.

style-src 'unsafe-inline'
→ Sayfanın satır içi CSS çalıştırmasına izin verir. Bu da güvenlik riski oluşturabilir.

img-src 'self' data:
→ Base64 gibi veri URI’leriyle gömülü görsellerin yüklenmesine izin verir.

font-src 'self' data:
→ Harici ve base64 ile kodlanmış fontların yüklenmesine izin verir.

Güvenlik etkisi:

Bu CSP yapılandırması, rapordaki stil, font ve gömülü içeriklerin düzgün görüntülenmesi için gereklidir.

Ancak aynı zamanda CSP’nin sunduğu temel güvenlik önlemlerini devre dışı bırakır.

>HTML Report Publisher ile raporlama entegrasyonu yapılabiliyor fakat, veriler html raporunda düzgün görünmüyor.
Çözüm olarak yukarıdaki komutu kullandığımızda bu sefer yukarıdaki güvenlik açıkları oluşuyor. Bundan dolayı benim kullandığım yöntem
Jenkins tarafında test koştuktan sonra failure olursa bilgi mailinin gönderilmesi(Jenkins' e entegre olan Mailer Plugin ile).


