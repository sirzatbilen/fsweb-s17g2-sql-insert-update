# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

# Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

## Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#Tablolar
`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

# Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

    1) ÖRNEK SORU: Yazar tablosunu KEMAL UYUMAZ isimli yazarı ekleyin.
    insert into yazar (yazarad,yazarsoyad) values ("KEMAL","UYUMAZ")


    2) Biyografi türünü tür tablosuna ekleyiniz.

    insert into tur (turadi) values ("biyografi")

    3) 10A sınıfı olan ÇAĞLAR ÜZÜMCÜ isimli erkek, sınıfı 9B olan LEYLA ALAGÖZ isimli kız ve sınıfı 11C olan Ayşe Bektaş isimli kız öğrencileri tek sorguda ekleyin.
    insert into ogrenci (ograd,ogrsoyad,cinsiyet,sinif) values ("ÇAĞLAR","ÜZÜMCÜ","E","10A"),("LEYLA","ALAGÖZ","K","9B"),("Ayşe","Bektaş","K","11C")


    4) Öğrenci tablosundaki rastgele bir öğrenciyi yazarlar tablosuna yazar olarak ekleyiniz.
    	INSERT INTO yazar (yazarad, yazarsoyad)
    	SELECT ograd, ogrsoyad FROM ogrenci
    	ORDER BY RAND() LIMIT 1;

    5) Öğrenci numarası 10 ile 30 arasındaki öğrencileri yazar olarak ekleyiniz.
    		INSERT INTO yazar (yazarad, yazarsoyad)
    	SELECT ograd, ogrsoyad FROM ogrenci
    	where ogrno between 10 and 30


    6) Nurettin Belek isimli yazarı ekleyip yazar numarasını yazdırınız.
    (Not: Otomatik arttırmada son arttırılan değer @@IDENTITY değişkeni içinde tutulur.)
    insert into yazar(yazarad,yazarsoyad) values('Nurettin','Belek');
    select @@IDENTITY AS "Yeni Yazar Numarası"


    7) 10B sınıfındaki öğrenci numarası 3 olan öğrenciyi 10C sınıfına geçirin.
    update ogrenci set sinif="10C" where ogrno = 3

    8) 9A sınıfındaki tüm öğrencileri 10A sınıfına aktarın
    SET SQL_SAFE_UPDATES=0;
    UPDATE ogrenci
    SET sinif = '10A'
    WHERE trim(sinif) = '9A';

    9) Tüm öğrencilerin puanını 5 puan arttırın.
    update ogrenci set puan = puan+5

    10) 25 numaralı yazarı silin.
    delete from yazar where yazarno=25;


    11) Doğum tarihi null olan öğrencileri listeleyin. (insert sorgusu ile girilen 3 öğrenci listelenecektir)
    select * from ogrenci
    where dtarih is null

    12) Doğum tarihi null olan öğrencileri silin.
    delete from ogrenci
    where dtarih is null


    13) Kitap tablosunda adı a ile başlayan kitapların puanlarını 2 artırın.
    update kitap set puan = puan +2
    where kitapadi like "a%"

    14) Kişisel Gelişim isimli bir tür oluşturun.
    insert into tur (turadi) values ("Kişisel Gelişim")

    15) Kitap tablosundaki Başarı Rehberi kitabının türünü bu tür ile değiştirin.
    update kitap set turno = 9
    where kitapadi = "Başari Rehberi"

    16) Öğrenci tablosunu kontrol etmek amaçlı tüm öğrencileri görüntüleyen "ogrencilistesi" adında bir prosedür oluşturun.
    	CREATE DEFINER=`root`@`localhost` PROCEDURE `ogrencilistesi`()
    	BEGIN
    	select * from ogrenci;
    	END

    17) Öğrenci tablosuna yeni öğrenci eklemek için "ekle" adında bir prosedür oluşturun.
    CREATE DEFINER=`root`@`localhost` PROCEDURE `ekle`(in
    ograd varchar(50),
    ogrsoyad varchar(50),
    sinif varchar(20),
    dtarih date,
    cinsiyet varchar(10),
    puan varchar(10))

    BEGIN
    insert into ogrenci (ograd,ogrsoyad,cinsiyet,dtarih,sinif,puan)
    values (ograd,ogrsoyad,cinsiyet,dtarih,sinif,puan);
    END

    18) Öğrenci noya göre öğrenci silebilmeyi sağlayan "sil" adında bir prosedür oluşturun.
    CREATE DEFINER=`root`@`localhost` PROCEDURE `sil`(in ono int)
    BEGIN
    delete from ogrenci where ono = ogrno;
    END


    19) Öğrenci numarasını kullanarak kolay bir biçimde öğrencinin sınıfını değiştirebileceğimiz bir prosedür oluşturun.
    CREATE DEFINER=`root`@`localhost` PROCEDURE `sinifdegistir`(in o_no int,o_sinif varchar(25))
    BEGIN
    update ogrenci set sinif=o_sinif where ogrno=o_no;
    END

    20) Öğrenci adı ve soyadını "Ad Soyad" olarak birleştirip, ad soyada göre kolayca arama yapmayı sağlayan bir prosedür yazın.
    CREATE DEFINER=`root`@`localhost` PROCEDURE `searchad`(in search varchar(25))
    BEGIN
    select * from ogrenci
    where concat(ograd," ",ogrsoyad)  like concat("%",search,"%");
    END

    21) Daha önceden oluşturduğunu tüm prosedürleri silin.
    DROP PROCEDURE IF EXISTS procedure_name;


    #Esnek görevler (Esnek görevlerin hepsini Select in Select ile gerçekleştirmeniz beklenmektedir.)
    22) Select in select yöntemiyle dram türündeki kitapları listeleyiniz.
	 SELECT tur.turadi,kitap.kitapadi FROM kitap,tur
	WHERE kitap.turno=tur.turno
	and tur.turadi=(SELECT tur.turadi FROM tur WHERE tur.turadi like '%dram%');

    23) Adı e harfi ile başlayan yazarların kitaplarını listeleyin.
	

    24) Kitap okumayan öğrencileri listeleyiniz.


    25) Okunmayan kitapları listeleyiniz


    26) Mayıs ayında okunmayan kitapları listeleyiniz.
