
Geliştirdiğimiz projeler büyüdükçe, performans sorunlarıyla karşılaşıyoruz. Önyüz geliştirme yaparken bu tür problemleri nasıl çözebileceğimizi ve hız kazanacağımızı anlatacağım.

Geçtiğimiz birkaç hafta boyunca web performansı ile ilgili çeşitli araştırmalar yaptım. Web sayfamızın hızlı yüklenmesi ve kullanıcıya düzgün  ve hızlı bir şekilde gelmesinin, önemsememiz gereken bir konu olduğu kesin.

İşe koyulmadan önce, nasıl bir uygulamaya sahip olduğumuzu göz önünde bulundurmalıyız. Sayfanın yavaş açılmasının yüzlerce farklı sebebi olabilir. 

* Belki CDN, geç yanıt veriyordur ?
* Belki kullanıcıya yüklettiğiniz font dosyalarının boyutu çok büyüktür.
* Belki resim dosyalarınızın boyutları gereğinden büyüktür.

PageSpeed Insights, Yslow gibi performans araçlarını biliyorsunuzdur. Web sayfamızı bu araçlardan geçirerek, hangi dosyaların geç yüklendiğini ve bize neler yapmamız gerektiğini,
raporluyor. Ama işler sadece bununla sınırlı değil. Bu test araçlarının düşünmediği bazı noktalarda var. 

- 200x200 boyutunde bir resim göstermemiz gereken yerde, 600x600 boyutunda bir resmi kullanıcıya yüklettiğimiz zamanlar oluyor.
- jQuery, Bootstrap, Font-Awesome gibi kütüphaneleri projeye dahil ettiğimiz zamanlarda, hiç kullanmadığımız yazı tipleri, ikonlar, kod parçaları dosya boyutunu yükselttiği için, kullanıcıya gereksiz yere dosya yükletiyoruz. 
- 1 tane modal açtırmak için, -afedersiniz- "jquery kütüphanesi" kullanmak zorunda mısınız?

Bunlar bugüne kadar karşılaştığım "genel" sorunlar. Bir çoğumuz, projeye işimizi kolaylaştıran kütüphaneleri dahil ederken, uygulamanın performansını ve gelecekte uygulamanın geleceği boyutu gözden kaçırıyor. 
Onları kınıyorum ve onlara laflar hazırladım!

### Gereksiz Verilerden Kurtulun!

10-15 tane ikon kullanmak için, Font-Awesome'a gerçekten ihtiyacımız var mı? 

X kaynağını her sayfamıza dahil ettik, ancak bunu indirip görüntülenme maliyeti, kullaniciya sağladığı değeri karşılıyor mu? Kullanıcıların bu kaynağı kullandığını, ölçüp test ettik mi? Kullanıcılar, Fotoğraf Galerisi için yazdığımız javascript dosyasını, kullanmadığı halde indirmek zorunda mı?

* Hürriyet'in İnternet Sitesi, ziyaretçilerin herhangi bir tıklama yapmadan, haberleri ve fotoğrafları görebilmesi için, anasayfasına bir sürü modül eklemiş. 
Sayfa yüklendiğinde tüm fotoğraflar kullanıcı tarafından göz gezdirilmese bile, bilgisayarına indirilecek. 

* Her kullanıcının sayfanıza gelip, en aşağıda bulunan slider'ı kullandığını, orada yüklettiğiniz en kötü ihtiamli düşünüp, 200kb'lık 5-6 fotoğraftan oluşan galeriyi kullandığını ölçtünüz mü?  Ziyaretçi tarafından hiçbir zaman görüntülenmeyecek gereksiz kaynakları indirterek, yüksek bir maliyete sebep oluyorsunuz.

*B Sitesi, web sayfasını Mobil Tarayıcılar ile uyumlu yapmak için, -afedersiniz- Boootstrap Framework'ünü projesine dahil etmiştir. 126KB CSS ve 29KB JS, toplamda 155kb'lık bir veriyi boş yere kullanıcıya yükletmeye başlamıştır. Oysa 15 dakika içerisinde basit bir grid yazarak veya bularak, bu yükten kurtulabilirdik. 

### Sıkıştırın!

![](http://llcdn.listelist.com/listeliststatic/2013/05/Metrob%C3%BCs-s%C4%B1k%C4%B1%C5%9F%C4%B1k.jpg)

Gereksiz dosyalarımızı projeden çıkardıktan sonraki adim, tarayıcının indirmesi gereken kaynakların boyutunu en aza indirmetir. Kaynak türüne (metin, resimler, yazı tipleri vs.) bağlı olarak birbirinden farklı çeşitli teknikler bulunmaktadır.

GZIP, metin içeren dosyalarda bize bu konuda yardımcı oluyor. CSS, Javascript, HTML dosyalarımızı sunucuda gzip ile sıkıştırabiliriz. Tüm modern tarayıcılar GZIP sıkıştırmasını desteklemektedir. Sunucunuzun GZIP sıkıştırmasını çalıştıracak şekilde yapılandırılması gerekmektedir. Bu konuda hizmet aldığınız firma size yardımcı olacaktır. [Basit](http://stackoverflow.com/questions/2666120/how-can-i-gzip-my-javascript-and-css-files) birkaç konfigürasyon ile sizde yapabilirsiniz.

<table class="table-4"><colgroup><col span="1"><col span="1"><col span="1"><col span="1"></colgroup><thead><tr><th>Kitaplik</th><th>Boyut</th><th>Sikistirilmis boyut</th><th>Sikistirma orani</th></tr></thead><tbody><tr><td data-th="kitaplik">jquery-1.11.0.js</td><td data-th="boyut">276 KB</td><td data-th="sikistirilmis">82 KB</td><td data-th="tasarruflar">%70</td></tr><tr><td data-th="kitaplik">jquery-1.11.0.min.js</td><td data-th="boyut">94 KB</td><td data-th="sikistirilmis">33 KB</td><td data-th="tasarruflar">%65</td></tr><tr><td data-th="kitaplik">angular-1.2.15.js</td><td data-th="boyut">729 KB</td><td data-th="sikistirilmis">182 KB</td><td data-th="tasarruflar">%75</td></tr><tr><td data-th="kitaplik">angular-1.2.15.min.js</td><td data-th="boyut">101 KB</td><td data-th="sikistirilmis">37 KB</td><td data-th="tasarruflar">%63</td></tr><tr><td data-th="kitaplik">bootstrap-3.1.1.css</td><td data-th="boyut">118 KB</td><td data-th="sikistirilmis">18 KB</td><td data-th="tasarruflar">%85</td></tr><tr><td data-th="kitaplik">bootstrap-3.1.1.min.css</td><td data-th="boyut">98 KB</td><td data-th="sikistirilmis">17 KB</td><td data-th="tasarruflar">%83</td></tr><tr><td data-th="kitaplik">foundation-5.css</td><td data-th="boyut">186 KB</td><td data-th="sikistirilmis">22 KB</td><td data-th="tasarruflar">%88</td></tr><tr><td data-th="kitaplik">foundation-5.min.css</td><td data-th="boyut">146 KB</td><td data-th="sikistirilmis">18 KB</td><td data-th="tasarruflar">%88</td></tr></tbody></table>

### Kullanıcıyı Unutmayın! 
Eğer kullanıcılar uygulamanıza fotoğraf gönderiyorsa, onları sıkıştırmayı kesinlikle unutmayın. Özellikle emlak, ilan içerikli bir uygulamanız var ise, anasayfanızın büyük bir kısmını irili ufaklı fotoğraflar kaplayacaktır. 
En azından 30-40 adet fotoğraf anasayfada bulunacaktır. 

Fotoğrafları kullanıcılar yüklerken, sıkıştırmayı ve istediğiniz çözünürlüklerde resize etmeyi unutmayın. Tavsiyem mobil versiyonlar içinde ayrı bir fotoğraf oluşturmanız. Zira 320px çözünürlüğe sahip bir telefonda, 800px genişliğinde bir fotoğrafı yükletmeniz bir manası yok. Fotoğraflarınızın boyutuna her ne kadar güveniyorsanız güvenin, yinede düşük çözünürlüklü ekranlar için fotoğraflarınızı resize edin.

Bu konuda https://kraken.io/ size cüzi bir ücret karşılığında yardımcı olacaktır. Bu işlemi backend tarafında, ücretsiz kütüphaneler ile halledebilirsiniz. Büyük çoğunluğu kalite ve boyut konusunda düşürme yapıyor diye biliyorum. 

https://www.reddit.com/r/Android/comments/1x5ct6/image_compression_on_instagram/ şurada bu konu hakkında, son derece feyizli bir sohbet bulunuyor. Okumanızı tavsiye ederim.







