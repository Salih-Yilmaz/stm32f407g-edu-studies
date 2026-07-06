\# 01 - LED Blink



Bu çalışma, STM32F407G Discovery kartı üzerinde ilk GPIO output uygulamasıdır.



\## Amaç



Kart üzerindeki LED'i belirli aralıklarla yakıp söndürerek STM32CubeIDE, proje oluşturma, kod yükleme ve temel GPIO kullanımını öğrenmek.



\## Öğrenilecek Konular



\- STM32CubeIDE proje oluşturma

\- GPIO Output

\- HAL\_GPIO\_WritePin

\- HAL\_Delay

\- main.c yapısı

\- Build ve Debug/Run işlemleri



\## Donanım



\- STM32F407G Discovery Board

\- USB kablosu

## İlk Uygulama Sonucu



Karpuz.go EDU001 blinkLED projesi STM32CubeIDE içerisine import edildi. Proje build edildi ve STM32F407G Discovery karta yüklendi. Kod çalıştırıldığında kart üzerindeki mavi LED'in yanıp söndüğü gözlemlendi.



\## Kodun Temel Mantığı



```c

while (1)

{

HAL\_GPIO\_TogglePin(GPIOD, GPIO\_PIN\_15);

HAL\_Delay(500);

MX\_USB\_HOST\_Process();

}

```



Bu kodda `HAL\_GPIO\_TogglePin(GPIOD, GPIO\_PIN\_15)` fonksiyonu GPIOD portundaki 15 numaralı pinin durumunu tersine çevirir. Bu pine bağlı olan mavi LED yanıyorsa söner, sönükse yanar.



`HAL\_Delay(500)` fonksiyonu 500 ms bekleme oluşturur. Delay değeri değiştirildiğinde LED'in yanıp sönme hızı da değişir.



\## Yapılan Deneyler



\* `HAL\_Delay(500)` ile LED yaklaşık yarım saniyede bir durum değiştirdi.

\* `HAL\_Delay(5000)` ile LED'in çok daha yavaş, yaklaşık 5 saniyede bir durum değiştirdiği gözlemlendi.



\## Öğrenilenler



\* STM32CubeIDE üzerinde hazır bir proje import edildi.

\* Proje build edildi.

\* Debug/Run ile STM32F407G karta kod yüklendi.

\* `while(1)` sonsuz döngüsünün gömülü sistemlerde ana çalışma mantığı olduğu görüldü.

\* GPIO pininin yazılım ile kontrol edilerek fiziksel LED'in davranışının değiştirilebildiği gözlemlendi.



