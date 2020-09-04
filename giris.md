## Rust Programlama Dilinin Mülkiyet Kavramı Üzerine

### Giriş

Rust programlama dilini diğer dillerden ayıran özelliklerin başında ownership, borrowing ve lifetime
yani dilimizdeki karşılıklarıyla; **mülkiyet**​, **borçlanma​** ve **yaşam süresi**​ olan kavramlar geliyor.
Dilin temelini oluşturan bu üç kavramın, bu dili kullanan programcılar için geleneksel programlama
yaklaşımını tamamen değiştirdiğini ifade edebiliriz. Öyle ki; diğer programlama dillerinin bakış
açılarına hakim olan programcıların mantıklı bulduğu kodlar Rust ile denendiğinde genellikle
çalışmaz.

Rust dilinde mülkiyet, öylesine önemli bir kavramdır ki, bu kavramı mümkün olduğunca erken
anlamak, derleyici hatalarını değerlendirmek ve dili daha çabuk kavramak açısından oldukça
yararlıdır. Borçlanma, yaşam süresi gibi kavramlar ise, verilerin hafızada tutulma şekli ve hafıza güvenliği ile
doğrudan ilişkili olduğundan, Rust'ın doğru ve tutarlı biçimde anlaşılabilmesi bu kavramların derinlemesine
incelenmesi ve öğrenilmesi önemlidir.

Bu döküman Rust programlama dilini tercih eden geliştiricilerin Mülkiyet Kavramını anlamasına yardımcı olacaktır.   
