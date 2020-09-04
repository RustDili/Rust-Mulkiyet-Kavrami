### Hafıza Güvenliği
Bir programlama dili olarak Rust'ın önemini tartışmadan önce, bellek güvenliğinin aslında ne anlama geldiğini anlamak önemidir. Tecrübelerini sistem programcılığına uygun olmayan yahut çöp toplama mekanizmasına sahip diller aracılığıyla edinmiş programcılar için Rust'un temel özelliklerini ayırt etmek zor olabilir.

Will Crichton'ın, **Rust' ta Bellek Güvenliği: C ile Bir Örnek Çalışma​** adlı önemli makalesinde belirtildiği gibi: *“Hafıza güvenliği, kullanılan işaretçilerin daima doğru tür/boyutta tahsis edilen geçerli hafızaya işaret ettiği bir programlama özelliğidir. Güvensiz hafızaya sahip bir program, hatalarına bağlı olarak teknik olmayan çıktılar üretebileceği ya da kendiliğinden çökebileceğinden, hafıza güvenliği bir doğruluk sorunudur.”* 
Bu ifadeden de anlaşılacağı gibi, uygulamada **hafıza güvenliği sağlamadan** kod yazmaya izin veren programlama dillerinin var olduğunu anlıyoruz. Hafıza güvenliği sunmayan programların ise aşağıdaki türden sorunlara neden oldukları sır değil:

* **Sarkan İşaretçiler:** Geçersiz ya da silinmiş verileri gösteren işaretçiler. (Bu türden sorunlarla karşılaşıldığında verinin bellekte nasıl depolandığına bakılması mantıklı olacaktır. [Daha fazla bilgi için](https://stackoverflow.com/questions/17997228/what-is-a-dangling-pointer)

* **Çift Boşaltma:** Aynı hafıza bölgesini iki kere boşaltmaya çalışarak *tanımsız davranışlara* yol açmak [Daha fazla bilgi için](https://stackoverflow.com/questions/21057393/what-does-double-free-mean)

**Sarkan İşaretçiler** kavramını izah edebilmek için aşağıdaki `D` kodunun hafızada nasıl temsil edildiğini inceleyelim.

```d
string s = "Have a nice day";
```

Dizgi olarak ilklendirilen bir değişkenin bellekte kullandığı `stack` ve `heap` bölümleri aşağıdakine benzer biçimde gösterilir.

```bash
                      buffer
                    /   capacity
                  /   /    length
                /   /    /
            +–––+––––+––––+
stack frame │ • │ 16 │ 15 │ <– s
            +–│–+––––+––––+
              │
            [–│––––––––––––––––––––––––– capacity ––––––––––––––––––––––––––]
              │
            +–V–+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+
        heap │ H │ a │ v │ e │   │ a │   │ n │ i │ c │ e │   │ d │ a │ y │   │
            +–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+–––+

            [––––––––––––––––––––––––– length ––––––––––––––––––––––––––]
```

`Heap` ve `stack` kavramlarının ne olduğuna gelmeden önce, `stack` üzerinde depolanan verinin üç kelimeden oluşan sabit boyutlu bir `string` nesnesi olduğunu söylememiz gerekir. Altındaki alanlar ise asıl verileri, arabellek kapasitesini, metnin uzunluğunu tutan ve heap tarafından ayrılan arabellek için bir işaretçidir. Başka bir deyişle arabelleğinin sahibi **s**  adlı `string` nesnesidir. Bu nedenle nesne program tarafından yok edildiğinde, string boyutu kadar tamponlanmış olan belleği de türünün kendi yıkıcısı tarafından serbest bırakılacaktır. Ara belleğin geri verilmesine rağmen, aynı tampon belleğe referans veren başka işaretçiler de olabilir. Nesnenin yıkılmasıyla boşaltılan ve artık hafızada bulunmayan bu tampon belleğe halen işaret etmekte olan bu işaretçilere **Sarkan İşaretçi**ler  denir.

`D` ve `Golang` gibi yeni nesil diller bu tür sorunların üstesinden, bünyelerinde bulundurdukları çöp toplayıcı mekanizmalar sayesinde gelirler. Bu mekanizmalar ise, programın işletilmesi sırasında daha fazla kullanılmayan ve hafızaya geri verilmesi gereken her şeyi, tespit ederek sisteme geri verimekle yükümlüdürler. Çöp toplama mekanizmalarına sahip programlama dilleri her ne kadar şirin görünseler de, gerçekleştirdikleri işlemler programın çalışma zamanı ve performansını etkiler.

Rust hafıza güvenliği sorunlarının üstesinden çöp toplama mekanizması kullanmak yerine, hafıza güvenliğini garanti altına alan **mülkiyet** ve **borçlanma** sistematiğini kullanarak geliyor. O nedenle **Rust’ta hafıza güvenliği** demekle, *derleyicinin güvensiz kod yazılmasına izin vermeyeceği* anlatılmak istenir.
