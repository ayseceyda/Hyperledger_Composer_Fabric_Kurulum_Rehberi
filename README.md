# Hyperledger Composer ve Fabric Kurulum Rehberi #

Merhaba, hazırsanız kuruluma başlıyoruz. 
Ana Kaynak : https://hyperledger.github.io/composer/unstable/installing/installing-index.html

Sistem öncelikle bir tool olan Composer ı, ardından framework Fabric i kurmamıza izin veriyor. Composer kurulumuna başlamadan önce **ön koşullar** sağlanmalıdır. Bu sayfada okulumuz makinelerinin çoğunun **UBUNTU 16.04** kullandığını varsayarak bu dağıtım için kurulum yapacağız.

içerik
-----------
[Ön Koşulların Kurulumu](https://github.com/MSKU-BcRG/Hyperledger_Composer_Fabric_Kurulum_Rehberi#%C3%96n-ko%C5%9Fullar%C4%B1n-kurulumu)

Ön Koşulların Kurulumu
-----------------------

Ön Koşullar: Ubuntu Linux 14.04 / 16.04 LTS (ikisi de 64-bit) İşletim Sistemi, Docker Engine: 17.03 veya üzeri versiyon, Docker-Compose:1.8 veya üzeri versiyon, Node: 8.9 veya üzeri versiyon (versiyon 9 desteklenmiyor), npm: v5.x, git: 2.9.x veya üzeri versiyon, Python: 2.7.x versiyon, bir kod editörü (tercihe bağlı).
** ileride kullanacağımız bir komut bize ön koşulları indirecek fakat makineden makineye farklı hatalar alınabiliyor. Hata düzeyini minimalize etmek için siz de ön koşulları şimdiden manuel olarak indirebilirsiniz. (örneğin` sudo apt-get install docker.io ` vb.)

Verilen komutları -sırasıyla- girin:
* `curl -O https://hyperledger.github.io/composer/unstable/prereqs-ubuntu.sh`
* `chmod u+x prereqs-ubuntu.sh`
* `./prereqs-ubuntu.sh`     //otomatik biçimde ön koşulları indirir.

" "PROGRAM_ADI" --version " komutuyla versiyonlarınızın doğru olduğundan emin olun. İşleminizden sonra `sudo apt-get update` yapabilirsiniz.

Geliştirme Ortamının İndirilmesi
---------------------------------

Sıra geliştirme ortamını indirmekte. Bileşenleri indirmek için verilen komutları -sırasıyla- girin:
** Not: Bu aşamalarda aldığınız herhangi bir hatayı göz ardı etmemeniz tavsiye edilir. Hata ileriki aşamalarda tekrarlayacaktır. Çözümler için issue lara bakabilirsiniz. ** Not: npm, `su` ya da `sudo` ile kullanılamaz.

* `npm install -g composer-cli`
* `npm install -g composer-rest-server`
* `npm install -g generator-hyperledger-composer`
* `npm install -g yo`
* `npm install -g composer-playground`

Hyperledger Fabric Kurulumu
------------------------------

* `mkdir ~/fabric-dev-servers && cd ~/fabric-dev-servers` //fabric-dev-servers klasörü açıp içine giriyoruz. Bu klasör ana klasörümüz oldu.
* `curl -O https://raw.githubusercontent.com/hyperledger/composer-tools/master/packages/fabric-dev-servers/fabric-dev-servers.tar.gz`
* `tar -xvf fabric-dev-servers.tar.gz`
* `./downloadFabric.sh` // Fabric kurulumu burada başlıyor.
* ~/fabric-dev-servers klasörü içerisinde olduğunuzdan emin olarak `./startFabric.sh `komutunu girin. Bu aşamada eksik bir kurulum yaptıysanız hata alabilirsiniz, çözüm için issue lara bakınız.
* `composer-playground`  // Bu komut sizi *8080* portunda lokal tarayıcınıza bağlayacaktır. Playground u lokal olarak kullanabilirsiniz.

![playground](https://user-images.githubusercontent.com/29989590/42679442-9f277a78-868a-11e8-89de-7df184ef5fe2.png)

** Bu aşamada aldığınız ve çözümünü bulamadığınız herhangi bir hata için, kurulumu kaldırıp yenilemeyi deneyebilirsiniz, bakınız : https://hyperledger.github.io/composer/unstable/installing/development-tools.html

Geliştiriciler İçin
----------------------

Artık geliştiriciler için olan kısma gelmiş bulunuyoruz. cd komutu ile ~/fabric-dev-servers içinde bulunduğunuzdan emin olduktan sonra, ` yo hyperledger-composer:businessnetwork ` komutunu girin. Ağ adına **tutorial-network** diyebilirsiniz.
** Verilen ağ adları, kart adları gibi değerlerin kolaylık sağlaması açısından **aynı girilmesi** tavsiye ediliyor, aksi halde her komutu kendi network, kart, klasör adınıza göre düzenlemek zorunda kalabilirsiniz.

* Editör açıklaması, ismi, emaili kısımlarını serbestçe doldurabilirsiniz.
* Lisans için **Apache-2.0** ı seçin.
* Ağ adınızı **org.example.mynetwork** olarak girin.
* Boş bir ağ oluşturup oluşturmamak konusunda ne istediğinizi sorduğunda **hayır(n)** seçin.
// Böylece iş ağınızın yapısını oluşturdunuz.

İş ağı tanımlamak için verilen adımları uygulayın:

* ~/fabric-dev-servers/tutorial-network/models içerisinden " **org.example.mynetwork.cto** " dosyasının içeriğinin tamamını silip yerine repo da verilen dosyayı yapıştırın, kaydedin.
* ~/fabric-dev-servers/tutorial-network/lib içerisindeki " **logic.js** " dosyasının içeriğinin tamamını silip yerine repo da verilen dosyayı yapıştırın, kaydedin.
* ~/fabric-dev-servers/tutorial-network içerisindeki " **permissions.acl** " dosyasının içeriğinin tamamını silip yerine repo da verilen dosyayı yapıştırın, kaydedin.

İş ağını dağıtmak için verilen adımları uygulayın:

* ~/fabric-dev-servers/tutorial-network içinde olduğunuzdan emin olarak `composer archive create -t dir -n . `komutunu girin. Bu durumda sonu **@0.0.1.bna** ile biten zip, klasörünüzün içerisinde olmalı.
* cd .. ile çıkarak ~/fabric-dev-servers klasörüne geri dönüyoruz. Burada ` ./createPeerAdminCard.sh `komutunu giriyoruz.
* Ardından tutorial-network e cd girerek ` composer network install --card PeerAdmin@hlfv1 --archiveFile tutorial-network@0.0.1.bna ` komutunu giriyoruz ve sırasıyla devam ediyoruz:
* `composer network start --networkName tutorial-network --networkVersion 0.0.1 --networkAdmin admin --networkAdminEnrollSecret adminpw --card PeerAdmin@hlfv1 --file networkadmin.card`
* `composer card import --file networkadmin.card`
* `composer network ping --card admin@tutorial-network`  // komutlarını giriyoruz. Bu komutlarla iş ağımızı dağıtmış olduk.

Composer-Rest-Server
--------------------------

~/fabric-dev-servers/tutorial-network içerisinden ` composer-rest-server `komutunu girince açılan kısma aşağıda verilen değerleri giriniz:
* Kart adı olarak: **admin@tutorial-network**
* **Never use Namespaces**
* Generated API: **No** ; TLS Security: **No** ; Event Publication: **Yes**
// Böylelikle API yi iş ağına ve blok zincirine bağlamış olduk.

![rest](https://user-images.githubusercontent.com/29989590/42679402-78852758-868a-11e8-8d09-9ab434040fb2.png)

Angular-App Kurulumu
-----------------------

Bir Angular 4 uygulaması edinebilmek için ~/fabric-dev-servers/tutorial-network içerisinden ` yo hyperledger-composer:angular` komutunu giriniz.

* Çalışan iş ağına bağlanmak istediğinizi sorduğunda cevabı **Evet(y)** olarak girin.
* Proje adı, açıklama, editör adı, emaili serbestçe girebilirsiniz.
* Business Network Kart sorduğunda **admin@tutorial-network** ü giriniz.
* **Connect to an existing REST API** seçeneğini seçiniz.
* REST Server adresi olarak **http://localhost** ; sunucu portu olarak ise **3000** i girin.
* **Namespaces are not used** seçeneğini işaretleyin.

Uygulamayı başlatmak için ~/fabric-dev-servers/tutorial-network/angular-app dizinine girin. Ardından `npm start` dediğinizde uygulama yüklenecek ve **4200** portuna bağlanacaktır.

![angular](https://user-images.githubusercontent.com/29989590/42679936-3a54a0ba-868c-11e8-949d-a35f559621d1.png)

** Eğer lokal bağlantıları tarayıcınız açmazsa işe yarayabilecek portlar:
REST-Server-API : **3000**, 
Angular-App : **4200**, 
Composer-Playground : **8080**

** Eğer lokalde çalışıyorsanız, yaptığınız değişiklikler **kaydedilmeyecek** ve uygulamayı her seferinde yeniden başlatacaksınız. Bunun için bir sunucuya bağlanmanızı tavsiye ediyoruz. Angular-App veya Composer-Playground u başlatmak için baştaki koşullar geçerliyken (Angular için `npm start` ve Playground için `composer-playground`) Composer-Rest-Server a her seferinde iş ağı tanımlamak istemiyorsanız, geçerli komutu ~/fabric-dev-servers/tutorial-network içerisindeyken girin `composer-rest-server -c admin@tutorial-network -n never -w true`

* Kuruluma, Composer-Rest-Server a **Queries(Sorgular)** ekleyerek devam edeceğiz, yakında buradan ulaşabilirsiniz.
