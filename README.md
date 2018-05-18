# MES Fikirleri

## Protokoller ve standartlar:
- B2MML.Net nerelerde kullanılmalı? MESA repo'sunda tartışma açtım, bekliyorum. Bence Uygulamanın ilgili class'ları tarafından kalıtılabilir.
- M2M iletişimde mevcut UDP tabanlı iletişim sistemi yerine endüstri standartları mı kullanılmalı yoksa genel kavramlar mı? Genel kullanılırsa MQTT uygun. Başlangıç için ve ücretsiz olduğu için iyi.
- Endüstriyel standartlar denenecekse OMG standardı DDS uygulanabilir. Bu konuda Rx4DDS.Net adında bir kütüphane var. MQTT gibi pub-sub olduğu için ölçeklenebilir. Rx olduğu için (Observer-observable olsa da) yüksek performans sağlar. RTI ücretli ancak açık kaynaklar için farklı işlem görülebiliyor.
- DDS yerine SOA mimarili OPC UA da kullanılabilir. Son hali pub-sub'ı da destekliyormuş.  Ayrıca ikisi birlikte de kullanılabilir. (Legacy ve .NET Standard için iki ayrı repo var.)
- OPC UA/DDS Gateway yayımlandı, geçişlilik standart biçimde mümkün
- Farklı katmanlarda farklı sistemler uygulanabilir (IIRA: machine, unit, site, inter-site vs.). Bu durumda en üst katmanda SOA bir yapı kurulabilir. SOA içinde okunurluk için JSON, performans için protobuf, ZeroFormatter gibi seçenekler de düşünülebilir.
- Diğer yazılımlarla entegrasyonda, mesajlaşma formatı EDI'dan bahsetmişti. Edifabric ile pek çok standarda göre yazmak mümkün. AS4 gibi bir web servis ayağı da var. Nerelerde kullanılıyor?
- KPI-ML diye bir başka MESA standardı var. Otomasyon sistemleri için KPI'ları içeren ISO 22400 standardını gerçekleyen bir XSD şeması. Bunu uygulama için bir benchmark olarak kullanmak mümkün. Bunun da kütüphanesi var.
- Netmap, FreeBSD için yazılmış, windows portu olan bir kütüphane. Ağ üzerinde hızlı ve büyük veri alışverişi için işletim sistemini bypass edip doğrudan ethernet kartı üzerinden işlem yapmayı sağlıyor. C# için wrapper yapılıp dahil edilebilir. 
- En üst katmanda RabbitMQ (MassTransit ile) ve/veya Kafka (Logstash) kullanılabilir mi?
- OpenStack ya da ServiceStack gibi bir altyapı sistemi temize çekebilir.
- CQRS? Read ağırlıklı domain için makul.

## Güvenlik:
- DDS Security: DDS güvenliği ile ilgili standart var. PKI uygulanıyor.
- Uygulama güvenliğine yönelik mimarinin ve kodların kontrolünü elden geçirebiliriz.

## Veri Analizi:
- Akan tüm verileri alıp Timescale DB'ye aktaran bir servis yazmak analiz için kullanılacak veriyi sağlayabilir.
- Loglama işini her bir uygulama/cihaz/node kendi yapıp gerektiğinde çekip almak mı daha iyi yoksa tüm log işi için bir log toplayıcı servise paslamak mı daha iyi? Bu durumda log için de bir pub-sub yapıp her node'u publisher, logger'ı da subscriber kabul etmek makul gibi.

## Endüstri 4.0 Framework'leri:
- RAMI 4.0 SOA altyapılı bir sistem öneriyor. Daha açık ve uygulamaya yönelik.
- IIC ürünü olan IIRA birkaç farklı mimariyi öneriyor. Daha çok sürece yönelik.
- Her birinin ayrı kullanıcısı olacak ve birlikte çalışılabilirlik meselesi ortaya çıkacaktır.
- OPC UA'dan DDS bus'ına veri gönderen ve alan bir gateway yazma yahut DDS bus'ından OPC UA uygulamasının pub-sub bus'ına veri gönderme denenebilir.

## Dotnet:
- Rx kullanıldığı için veri yapılarında DynamicData kütüphanesi kullanılması gerekebilir.
- SOA kısmında Quartz.net, Hangfire ya da FluentScheduler gibi bir görev zamanlayıcı, Polly veritabanı hata kontrolü,  Config.Net gibi bir konfigürasyon kontrolü, Wexflow gibi bir workflow , vs kullanılabilir.
- Yazılımı sanal makineler üzerinde kuracak infrastructure as a code yapısı düşünülebilir (Docker?).
