# MES / ERP hakkında

## Protokoller ve standartlar:
- [B2MML.Net](https://github.com/jpdillingham/B2MML.NET) nerelerde kullanılmalı? MESA repo'sunda [tartışma](https://github.com/MESAInternational/B2MML-BatchML/issues/13) açtım, bekliyorum. Bence uygulamanın ilgili class'ları tarafından kalıtılabilir.
- M2M iletişimde mevcut UDP tabanlı iletişim sistemi yerine endüstri standartları mı kullanılmalı yoksa genel kavramlar mı? Genel kullanılırsa MQTT uygun. Başlangıç için ve ücretsiz olduğu için makul.
- Endüstriyel standartlar denenecekse OMG standardı [DDS](http://portals.omg.org/dds/) uygulanabilir. Bu konuda [Rx4DDS.Net](https://github.com/rticommunity/rticonnextdds-reactive) adında bir kütüphane var. MQTT gibi pub-sub olduğu için ölçeklenebilir. Rx olduğu için (Observer-observable olsa da) yüksek performans sağlar. RTI ücretli ancak açık kaynaklar için farklı işlem görülebiliyor.
- DDS yerine SOA mimarili [OPC UA](https://opcfoundation.org/about/opc-technologies/opc-ua/) da kullanılabilir. Son hali pub-sub'ı da destekliyormuş.  Ayrıca ikisi birlikte de kullanılabilir. ([Legacy](https://github.com/OPCFoundation/UA-.NET-Legacy) ve [.NET Standard](https://github.com/OPCFoundation/UA-.NETStandard) için iki ayrı repo var.)
- OPC UA/DDS Gateway standardı [yayımlandı](https://www.rti.com/blog/announcing-the-opc-ua-dds-gateway-standard), geçişlilik standart biçimde mümkün
- Farklı katmanlarda farklı sistemler uygulanabilir ([IIRA](https://www.iiconsortium.org/IIRA.htm): machine, unit, site, inter-site vs.). Bu durumda en üst katmanda SOA bir yapı kurulabilir. SOA içinde okunurluk için JSON, performans için [protobuf](https://github.com/mgravell/protobuf-net), [ZeroFormatter](https://github.com/neuecc/ZeroFormatter) gibi seçenekler de düşünülebilir.
- Diğer yazılımlarla entegrasyonda, mesajlaşma formatı EDI'dan bahsetmişti. [Edifabric](https://www.edifabric.com/) ile pek çok standarda göre yazmak mümkün. AS4 gibi bir web servis ayağı da var. Nerelerde kullanılıyor? Gerekirse benzer bir modül ile EDI dokümanlarını .Net objesine ya da JSON'a çevirme işi yapılabilir. 
- Yukarıda saydığım standartlar, diğer uygulamalarla iletişim sağlamak üzere çeşitli adapter'lar ile gerçeklenmeli. Uygulama kendi içinde test edilip en uygun bulunan yollarla iletişimini sağlamalı.
- [KPI-ML](https://github.com/MESAInternational/KPI-ML) diye bir başka MESA standardı var. Otomasyon sistemleri için KPI'ları içeren ISO 22400 standardını gerçekleyen bir XSD şeması. Bunu uygulama için bir benchmark olarak kullanmak mümkün. Yani planlanan ve gerçekleşen arasında bir köprü olarak KPI'ları kontrol mekanizmasının merkezine almak makul olabilir. Bunun da [kütüphanesi](https://github.com/jpdillingham/KPI-ML.NET) var.
- [Netmap](https://github.com/luigirizzo/netmap), FreeBSD için yazılmış, windows portu olan bir servis. Kernel için servis şeklinde yazılmış ancak sonradan port edildiği için doğrudan kullanımı zor. Ağ üzerinde hızlı ve büyük verilerin alışverişi için ([örnek](https://www.bbc.co.uk/rd/blog/2018-04-high-speed-networking-open-source-kernel-bypass)) işletim sistemini bypass edip doğrudan ethernet kartı üzerinden işlem yapmayı sağlıyor. C# için interop ile bir wrapper yapılıp dahil edilebilir.
- Message queue veya bus kullanılabilir yerlerde [RabbitMQ](https://www.rabbitmq.com/) ([MassTransit](http://masstransit-project.com/) ile) ve/veya [Kafka](https://kafka.apache.org/) ([Logstash](https://www.elastic.co/products/logstash) ile) kullanılabilir mi?
- OpenStack ya da ServiceStack gibi bir altyapı sistemi temize çekebilir.
- Mimari tasarım nasıl olmalı? [Örnek 1](https://github.com/donnemartin/system-design-primer)
- UI için kullanılan _feature toggling_ sistemin geneli için düşünülebilir. Her müşteri farklı versiyonun farklı modüllerden oluşan bir paketini kullanacaktır.

## Güvenlik:
- [DDS Security](https://github.com/omg-dds/dds-security): DDS güvenliği ile ilgili standart var. PKI uygulanıyor.
- Uygulama güvenliğine yönelik mimarinin ve kodların kontrolünü elden geçirebiliriz.

## Veri Analizi:
- Akan tüm verileri alıp [Timescale DB](https://www.timescale.com/)'ye aktaran bir servis yazmak analiz için kullanılacak veriyi sağlayabilir. Event Bus'tan da akış sağlanabilir.
- Bir noktada Hadoop'a ihtiyaç duyulabilir. Ama nerede?

## Endüstri 4.0 Framework'leri:
- [RAMI 4.0](https://www.plattform-i40.de/I40/Redaktion/EN/Downloads/Publikation/rami40-an-introduction.html) SOA altyapılı bir sistem öneriyor. Daha açık ve uygulamaya yönelik.
- IIC ürünü olan [IIRA](https://www.iiconsortium.org/IIRA.htm) birkaç farklı mimariyi öneriyor. Daha çok sürece yönelik.
- Her birinin ayrı kullanıcısı olacak ve birlikte çalışılabilirlik meselesi ortaya çıkacaktır.
- OPC UA'dan DDS bus'ına veri gönderen ve alan bir gateway yazma yahut DDS bus'ından OPC UA uygulamasının pub-sub bus'ına veri gönderme denenebilir.

## Dotnet:
- Rx kullanıldığı için veri yapılarında [DynamicData](https://github.com/RolandPheasant/DynamicData) kütüphanesi kullanılması gerekebilir.
- SOA kısmında [Quartz.Net](https://github.com/quartznet/quartznet), [Hangfire](https://github.com/HangfireIO/Hangfire) ya da [FluentScheduler](https://github.com/fluentscheduler/FluentScheduler/) gibi bir görev zamanlayıcı, [Polly](https://github.com/App-vNext/Polly) veritabanı akış kontrolü,  [Config.Net](https://github.com/aloneguid/config) gibi bir konfigürasyon kontrolü, [Wexflow](https://github.com/aelassas/Wexflow/) gibi bir workflow , vs kullanılabilir.
- Yazılımı sanal makineler üzerinde kuracak infrastructure as a code yapısı düşünülebilir (Docker?).

## İçerik
- ITIL ve IT4IT gibi çözümler endüstriyel sorunlara da çözümleri içeriyor olabilir. ITIL pratiklerinin büyük bir kısmını gerçekleyen projeler ([1](https://github.com/glpi-project/glpi), [2](https://sourceforge.net/projects/itop/)) denenebilir. İlgili metinlerle uygulamayı kıyaslayıp spesifik problemlere önerilen çözümlerin uygulanma biçimlerini görmek faydalı olabilir.
- ITIL, tüm IT servislerini kapsıyor. Uygulamalar bazen sadece help desk ticketing alanını kapsayabiliyor. Ticari örnekleri incelemek gerekebilir: ServiceNow, BMC Software vs.

## İş Modeli:
- SaaS + On-premise: Jenerik ihtiyaçlar SaaS, spesifik, metale yakın olanlar on-premise vs.
- [Jobs to be Done](https://jtbd.info/): Kullanıcı gerçekte ne istiyor. Ne ifade ettiği değil, niyeti önemli. Çözmeye çalıştığı sorun kendi ifade ettiği gereksinimden daha önemli.
- Bu konuda sektörden birkaç kişiyle uzun uzun mülakat gerekebilir.
- Rakiplerle benchmark nasıl yapılabilir?
