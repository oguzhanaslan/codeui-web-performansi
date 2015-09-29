
Geliştirdiğimiz projeler büyüdükçe, performans sorunlarıyla karşılaşıyoruz. Önyüz geliştirme yaparken bu tür problemleri nasıl çözebileceğimizi ve hız kazanacağımızı anlatacağım.

Geçtiğimiz birkaç hafta boyunca web performansı ile ilgili çeşitli araştırmalar yaptım. Web sayfamızın hızlı yüklenmesi ve kullanıcıya düzgün  ve hızlı bir şekilde gelmesinin, önemsememiz gereken bir konu olduğu kesin.

İşe koyulmadan önce, nasıl bir uygulamaya sahip olduğumuzu, göz önünde bulundurmalıyız. Sayfanın yavaş açılmasının yüzlerce farklı sebebi olabilir. 

* Belki CDN, geç yanıt veriyordur ?
* Belki kullanıcıya yüklettiğiniz font dosyalarının boyutu çok büyüktür.
* Belki resim dosyalarınızın boyutları gereğinden büyüktür.

PageSpeed Insights, Yslow gibi performans araçlarını biliyorsunuzdur. Web sayfamızı bu araçlardan geçirerek, hangi dosyaların geç yüklendiğini ve bize neler yapmamız gerektiğini raporluyor. 
Ama işler sadece bununla sınırlı değil. Bu test araçlarının düşünmediği bazı noktalarda var. Bize sunduğu sadece neler yapmamız gerektiğine dair şeyler. Bende onlardan farklı birşey anlatmayacağım aslında.

Bugüne kadar çalıştığım şirketlerde, hizmet aldığımız SEO firmalarının genelde Pagespeed ve benzeri araçlara çok önem verdiğini gördüm. Fakat işin geri kalan tarafından hiçbiri haberdar değil. Bu konuda teknik yeterlilikleri pek yok. 

Bunlar bugüne kadar karşılaştığım "genel" sorunlar. Bir çoğumuz, projeye işimizi kolaylaştıran kütüphaneleri dahil ederken, uygulamanın performansını ve ileride geleceği boyutu gözden kaçırıyor. Benim tavsiyem "basit" diye nitelendirebileceğiniz işler için kütüphane kullanmamanız yönünde. Burada "jquery" popüler olduğu için seçtiğim bir örnekti. Kullanmamaktan ziyade, o işi daha kazançlı bir şekilde nasıl üretebiliyorsanız, o yolda ilerlemelisiniz.

- 200x200 boyutunde bir resim göstermemiz gereken yerde, 600x600 boyutunda bir resmi kullanıcıya yüklettiğimiz zamanlar oluyor.
- jQuery, Bootstrap, Font-Awesome gibi kütüphaneleri projeye dahil ettiğimiz zamanlarda, hiç kullanmadığımız yazı tipleri, ikonlar, kod parçaları dosya boyutunu yükselttiği için, kullanıcıya gereksiz yere dosya yükletiyoruz. 
- 1 tane modal açtırmak için, -afedersiniz- "jquery ve jquery kütüphanesi" kullanmak zorunda mıyız?

Onları kınıyorum ve onlara laflar hazırladım!

### Gereksiz Verilerden Kurtulun!

10-15 tane ikon kullanmak için, Font-Awesome'a gerçekten ihtiyacımız var mı? 

X kaynağını her sayfamıza dahil ettik. Ama bunun indirilip görüntülenme maliyeti, kullaniciya sağladığı değeri karşılıyor mu? Kullanıcıların bu kaynağı kullandığına dair bir test yaptık mı? Orada bulunan 12 tane ürün resmi gerçekten gerekli mi? Kullanıcı o içeriği gerçekten kullanıyor mu? 
Sayfanın en altında bulunan, Fotoğraf Galerisi için yazdığımız javascript dosyasını kullanıcıya yükletmek zorunda mıyız? Hatta o galeri için fancybox gibi ıvır zıvır bir sürü javascript ve css dosyasını orada yükletmek çok maliyetli değil mi? Belki 1000 kişiden 20 kişi orayı kullanacak.

* X bir haber sitesi, ziyaretçilerin herhangi bir tıklama yapmadan, haberleri ve fotoğrafları görebilmesi için, anasayfasına bir sürü slider eklemiş. 
Sayfa yüklendiğinde tüm fotoğraflar kullanıcı tarafından göz gezdirilmese bile, bilgisayarına indirilecek. 

* Her kullanıcının sayfanıza gelip, en aşağıda bulunan slider'ı kullandığını, orada yüklettiğiniz en kötü ihtiamli düşünüp, 200kb'lık 5-6 fotoğraftan oluşan galeriyi kullandığını ölçtünüz mü?  Ziyaretçi tarafından hiçbir zaman görüntülenmeyecek gereksiz kaynakları indirterek, yüksek bir maliyete sebep oluyorsunuz.

* B Sitesi, web sayfasını Mobil Tarayıcılar ile uyumlu yapmak için, Boootstrap Framework'ünü sayfasına dahil etmiştir. 126KB CSS ve 29KB JS, toplamda 155kb'lık bir veriyi boş yere kullanıcıya yükletmeye başlamıştır. 
Oysa 15 dakika içerisinde basit bir grid yazarak veya bularak, bu yükten kurtulabilirdik. Veya Bootstrap'te kullanmadığı component'leri silerek kullanabilirdi. Nasıl olsa çalışıyor...

### Dosyaları Birleştirin
HTTP Request'lerini azaltmak için çağırdığınız CSS ve Javascript dosyalarını tek bir dosyaya sıkıştırın. 
İmaj dosyalarını olabildiğince CSS Sprite tekniğini kullanarak çağırın. 

Eğer ikonların boyutları çok büyükse, benim tavsiyem sprite yerine ikonları font'a çevirerek kullanmanız. Bu sayede istediğiniz boyutta ve renkte ikonları kullanabiliyorsunuz. Performanstan ziyade, kullanım kolaylığı oluyor. 
[Fontastic](http://app.fontastic.me)'in servisini kullanarak, [Hobium](https://www.hobium.com) için, çizdiğimiz vektörel ikonları, font yaparak kullanmıştım. 
 
Bu işlemleri hızlıca yapmak için; Gulp, Grunt gibi araçları kullanabilirsiniz. Development yaparken Sprite gibi iş gücü gerektiren işleri otomatik olarak yaptırarak zaman kazanabilirsiniz. Eğer hala bu araçları kullanmıyorsanız, kullanın. 

http://burakcan.me/grunt-birakin-isleri-o-halletsin/
http://sonsuzdongu.com/blog/grunt-ile-frontend-islerinizi-otomatize-edin
http://tolga.gezginis.com/gulp-ile-frontend-islerinizi-yonetin.html


### Kullanıcıyı Unutmayın! 

-![](http://www.ids.com.tr/IDS-Media/image/firsat.jpg)

Eğer kullanıcılar uygulamanıza fotoğraf gönderiyorsa, onları sıkıştırmayı kesinlikle unutmayın. Özellikle emlak, ilan içerikli bir uygulamanız var ise, anasayfanızın büyük bir kısmını irili ufaklı fotoğraflar kaplayacaktır. 
En azından 30-40 adet fotoğraf anasayfada bulunacaktır. 

Fotoğrafları kullanıcılar yüklerken, sıkıştırmayı ve istediğiniz çözünürlüklerde resize etmeyi unutmayın. Tavsiyem mobil versiyonlar içinde ayrı bir fotoğraf oluşturmanız. Zira 320px çözünürlüğe sahip bir telefonda, 800px genişliğinde bir fotoğrafı yükletmeniz bir manası yok. Fotoğraflarınızın boyutuna her ne kadar güveniyorsanız güvenin, yinede düşük çözünürlüklü ekranlar için fotoğraflarınızı resize edin.

Bu konuda https://kraken.io/ size cüzi bir ücret karşılığında yardımcı olacaktır. Bu işlemi backend tarafında, ücretsiz kütüphaneler ile halledebilirsiniz. Büyük çoğunluğu kalite ve boyut konusunda düşürme yapıyor diye biliyorum. 

https://www.reddit.com/r/Android/comments/1x5ct6/image_compression_on_instagram/ şurada bu konu hakkında, son derece feyizli bir sohbet bulunuyor. Okumanızı tavsiye ederim.

### Tarayıcı Önbellekleme

![](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/images/http-cache-hierarchy.png)

    Statik kaynaklar için HTTP üstbilgilerinde bir süre sonu tarihi veya maksimum ömür ayarlamak, tarayıcıya, ağ üzerindeki kaynakları değil yerel diskte önceden indirilmiş olanları yüklemesini söyler. 

Kullanıcılar sayfanızı ziyaret ettikten sonra, tekrardan geri dönüş yapacaklardır. Bu geri dönüş esnasında, tarayıcından indirdiği dosyaları, tekrardan indirmesine gerek yok. Bu yüzden statik dosyalarınızı önbelleğe almalısınız. Bu sayed, daha sonraki girişlerde, sayfanız kullanıcının ekranında daha hızlı açılacaktır.

### Sıkıştırın!

![](http://llcdn.listelist.com/listeliststatic/2013/05/Metrob%C3%BCs-s%C4%B1k%C4%B1%C5%9F%C4%B1k.jpg)

Gereksiz dosyalarımızı projeden çıkardıktan sonraki adim, tarayıcının indirmesi gereken kaynakların boyutunu en aza indirmektir. Kaynak türüne (metin, resimler, yazı tipleri vs.) bağlı olarak birbirinden farklı çeşitli teknikler bulunuyor.

GZIP, metin içeren dosyalarda bize bu konuda yardımcı oluyor. CSS, Javascript, HTML dosyalarımızı sunucuda gzip ile sıkıştırabiliriz. Tüm modern tarayıcılar GZIP sıkıştırmasını desteklemektedir. Sunucunuzun GZIP sıkıştırmasını çalıştıracak şekilde yapılandırılması gerekmektedir. Bu konuda hizmet aldığınız firma size yardımcı olacaktır. [Basit](http://stackoverflow.com/questions/2666120/how-can-i-gzip-my-javascript-and-css-files) birkaç konfigürasyon ile sizde yapabilirsiniz.

Popüler kütüphanelerin GZIP ile sıkıştırıldıktan sonraki boyutları:
<table class="table-4"><colgroup><col span="1"><col span="1"><col span="1"><col span="1"></colgroup><thead><tr><th>Kitaplik</th><th>Boyut</th><th>Sikistirilmis boyut</th><th>Sikistirma orani</th></tr></thead><tbody><tr><td data-th="kitaplik">jquery-1.11.0.js</td><td data-th="boyut">276 KB</td><td data-th="sikistirilmis">82 KB</td><td data-th="tasarruflar">%70</td></tr><tr><td data-th="kitaplik">jquery-1.11.0.min.js</td><td data-th="boyut">94 KB</td><td data-th="sikistirilmis">33 KB</td><td data-th="tasarruflar">%65</td></tr><tr><td data-th="kitaplik">angular-1.2.15.js</td><td data-th="boyut">729 KB</td><td data-th="sikistirilmis">182 KB</td><td data-th="tasarruflar">%75</td></tr><tr><td data-th="kitaplik">angular-1.2.15.min.js</td><td data-th="boyut">101 KB</td><td data-th="sikistirilmis">37 KB</td><td data-th="tasarruflar">%63</td></tr><tr><td data-th="kitaplik">bootstrap-3.1.1.css</td><td data-th="boyut">118 KB</td><td data-th="sikistirilmis">18 KB</td><td data-th="tasarruflar">%85</td></tr><tr><td data-th="kitaplik">bootstrap-3.1.1.min.css</td><td data-th="boyut">98 KB</td><td data-th="sikistirilmis">17 KB</td><td data-th="tasarruflar">%83</td></tr><tr><td data-th="kitaplik">foundation-5.css</td><td data-th="boyut">186 KB</td><td data-th="sikistirilmis">22 KB</td><td data-th="tasarruflar">%88</td></tr><tr><td data-th="kitaplik">foundation-5.min.css</td><td data-th="boyut">146 KB</td><td data-th="sikistirilmis">18 KB</td><td data-th="tasarruflar">%88</td></tr></tbody></table>

#### Steam İnceleme

İncelemek için, kendime  2 tane oyun sitesi seçtim. 
Biri [Steam](store.steampowered.com) diğeri ise [Digigame](http://www.digigame.com.tr)

![](http://oi59.tinypic.com/1id85l.jpg)

2.55 saniyede yükleniyor ve 96 request var. 

Steam gördüğünüz üzere herşeyi düşünmüş durumda. Giriş ekranında bizi karşılayan slider, sağa doğru scroll ettiği sürece yeni resimleri çağırıyor. Sayfa ilk çalıştığında herşeyi yüklemiyor.
Yine aşağıda bulunan içerikler içinde aynısı geçerli. O kısma geldiğimiz zaman imajları yüklemeye başlıyor. 

Network penceresi açıp, sizde benim izlediğim yolu izleyin. Göreceksiniz ki kullanıcıya istemediği hiçbirşeyi yükletmiyor. Eğer siz onu kullanırsanız, o resim dosyalarını yüklüyorsunuz.

#### Digigame İnceleme
![](http://oi58.tinypic.com/b62how.jpg)
Aslında Steam'in karşısına çıkarabilecek örnek bulmakta zorlandım. Fakat şans eseri [Digigame](http://www.digigame.com.tr) adında bir oyun sitesi buldum. 
Oldukça yüksek boyutu olduğu için, tam benim aradığım örnekti.

10.48 saniyede yükleniyor ve 7.9mb boyutunda. 814 Request bulunuyor. Bu rakamları Steam ile karşılaştırdığımızda performans için yapacağımız bu basit işlemlerin ne kadar önemli olduğunu görüyoruz. 

Digigame, ne yapmamamız gerektiği konusunda bize gerekli dersleri veriyor.

* Resimler optimize edilmemiş.
* Sunucu oldukça yavaş.
* Herşey "pat" diye yükleniyor.
* Statik dosyalar birleştirilmemiş.
* * Ürün fotoğrafları 100-300kb arasında değişiyor.
* 1 MB'lık background görseli ile, Fedon'un kırdığı tabaklar kadar olmasada, kendince şov yapıyor.

![](http://img-s1.onedio.com/id-534ed52e5ed65e5814faa9ba/rev-2/raw/s-c02611fc554c7a7fea5e96325a1027ebb3c0a30c.jpg)

### Alman Olun!

Almanlık sıkı disiplin gerektiren bir mevzudur. Bizlere düşen görev ise, yaptığımız işlerin kullanıcılar tarafından kullanıldığını unutmayıp, disiplinli bir şekilde çalışarak, iyi bir Alman olmaktır :)

![](http://pbs.twimg.com/media/BsDIXntCEAA0XHL.jpg)
