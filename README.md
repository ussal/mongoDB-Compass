# MongoDB Compass ile Local MongoDB Kullanıcı İşlemleri
-------------
MongoDB kurulumunu local'e kurmuş olduğunuzu varsayarak ilerliyorum.
MONGOSH, Veritabanında kullanıcı işlemlerini yapacağımız konsol uygulaması.
MONGOSH'a MongoDB Compass ile veritabanına bağlantıktan sonra ekranın altından erişebilirsiniz.

## Veritabanı Kullanıcıları Listeleme
``` >use admin //admin veritabanında işlem yapacağımızı bildiriyoruz.```

``` >db.getUsers() // admin veritabanının kullanıcıları listele```
 
## Veritabanı Kullanıcı Oluşturma
```
 >db.createUser(
 {
   user: "newUserName", //Kullanıcı Adı
   pwd: "password123",  //Parola
   roles: [
      { 
      role: "readWrite", //Kullanıcı Yetkisi (okuma ve yazma)
      db: "admin" //Yetkileneceği Veritabanı Adı
      },
      { 
      role: "read", //Kullanıcı Yetkisi (sadece okuma) 
      db: "config" //Yetkileneceği Veritabanı Adı
      }
   ]
 }
)
```
## Veritabanı Kullanıcı Rol Seçenekleri
- read
- readWrite
- dbAdmin
- userAdmin
- clusterAdmin
- readAnyDatabase
- readWriteAnyDatabase
- userAdminAnyDatabase
- dbAdminAnyDatabase
Detaylar için: https://www.mongodb.com/docs/manual/reference/built-in-roles/#database-user-roles

## Kullanıcı Güncelleme
```
db.updateUser("newUserName",{
  roles: [
    { role: "userAdminAnyDatabase", db: "admin" } 
  ]
})
```
## Kullanıcı Silme
``` >db.removeUser("newUserName") ```
veya
``` >db.dropUser("newUserName") ```


# MongoDB Veritabana Kullanıcı ile Bağlanma

Her ne kadar veritabanına kullanıcı oluşturmuş olsakta veritabanına bağlanırken kullanıcı adı şifre girmeden giriş yapabiliyoruz.
## Kullanıcı adı ve şifre ile girişi aktif etmek için 
C:\Program Files\MongoDB\Server\6.0\bin\mongod.cfg dosyasını düzenlememiz gerek.
```
security:
  authorization: enabled
```
## MongoDB Server Değişikliliği Algılaması İçin Restart
Kodunu mongod.cfg içerisine yazdıktan sonra server'ın bunu algılayabilmesi için hizmeti yeniden başlatmanız gerek.

# MongoDB Dışarıya Dışarıya Açma
Bunu yapabilmek içinde yine aynı ayar dosyasını düzenlememiz gerekiyor.

C:\Program Files\MongoDB\Server\6.0\bin\mongod.cfg

Dosya içerisindeki kod alanında bindIp yerini 0.0.0.0 olarak ayarlayın.
Serverın algıyalabilmesi için  dosyayı kaydedip hizmeti tekrar restart atmanız gerekir.
```
net:
  port: 27017
  bindIp: 0.0.0.0
```
Geriye yapmanız gerek 27017 portunu güvenlik durundan dışarıya açmak kalacaktır.

# MongoDB Collection'da Tüm Dokümanlara Yeni Property Ekleme
Veritabanımızı seçelim.

` use admin `

UpdateMany komutu ile isActive:true özelliğini myCollection koleksiyonumdaki dokümanlara ekleyelim.

` db.myCollection.updateMany( { }, { $set: { isActive: true } } ) `
