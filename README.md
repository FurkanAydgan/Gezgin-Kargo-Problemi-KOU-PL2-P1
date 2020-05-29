//ProLab2.java(ANA MAIN)

------------------------------------------------------------------------

package prolab2;

import java.awt.Point;
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.File;
import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.Random;
import java.util.Scanner;


public class ProLab2 {

    static sehirler sehir[] = new sehirler[81];
    public static int[] enkisa_yol(int[][] komşu_maliyet, int baslangic) {   // baglangoç ile hangi şehirden başlayacağını ve komşuluk maliyet matrisini enkisa_yol fonksiyonunda aldık
        int uzunluk = komşu_maliyet[0].length;
        int[] mesafe = new int[uzunluk];
        int[] en_kısayol = new int[uzunluk];
        boolean[] ugrandi_Mi = new boolean[uzunluk];

        for (int i = 1; i < uzunluk; i++) {
            en_kısayol[i] = Integer.MAX_VALUE;                                     //hepsine en başta verebileceğim kadar en büyük değeri verip uğranmamış olarak işaretledik
            ugrandi_Mi[i] = false;
        }
        en_kısayol[baslangic] = 0;                                                 // ve büyük değer atadıktan sonra başlangıç olarak referans aldığım plakanın dizisine 0 değerini atadık
        int[] komsuluk_dizisi = new int[uzunluk];
        komsuluk_dizisi[baslangic] = -1;                                           // fonksiyona hangi şehir gönderiliyorsa dizinin o indexine -1 değeri atıyoruz

        for (int i = 2; i < uzunluk; i++) {                                       // 2den başlayarak uzunluk yani 82ye kadar for dönecek
            int sehir_index =  -1;
            int referans = Integer.MAX_VALUE;
            for (int j = 1; j < uzunluk; j++) {
                if (!ugrandi_Mi[j] && en_kısayol[j] < referans) {                 // buradaki ifte uğranmamış olacak ve sonsuzdan değerden daha küçük bir değer arayana kadar kontrol edecek ardından sonsuz değer değiştikten sonra en kısa gidebilecek mesafeyi buluyor
                    sehir_index = j;
                    referans = en_kısayol[j];
                }
            }

            ugrandi_Mi[sehir_index] = true;

            for (int k = 1; k < uzunluk; k++) {
                int komsu_mesafe = komşu_maliyet[sehir_index][k];                //gelen şehir için komşu maliyet matrisinde komsu_mesafeye atama işlemi yapıyoruz

                if (komsu_mesafe > 0 && ((referans + komsu_mesafe) < en_kısayol[k])) // eğer komşı_mesafe matisinden 0dan farklı yani komşusu ise kontrol ediliyor
                {
                    komsuluk_dizisi[k] = sehir_index;                              //fonksiyona ilk gelen şehirden komşusu olan diğer şehirlerin dizisini oluşturuyor
                    en_kısayol[k] = referans + komsu_mesafe;                        //fonksiyona ilk gelen şehirden komşusu olan diğer şehirlerin maliyetini oluşturuyor
                }
            }
        }
        for (int i = 1; i < 82; i++) {
            mesafe[i] = komsuluk_dizisi[i];
        }
        return mesafe;
    }

    public static int[] maliyet(int[][] komşu_maliyet, int baslangic) {
        int uzunluk = komşu_maliyet[0].length;
        int[] mesafe = new int[uzunluk];
        int[] en_kısayol = new int[uzunluk];
        boolean[] ugrandi_Mi = new boolean[uzunluk];

        for (int i = 1; i < uzunluk; i++) {
            en_kısayol[i] = Integer.MAX_VALUE;                          //hepsine en başta verebileceğim kadar en büyük değeri verip uğranmamış olarak işaretledik
            ugrandi_Mi[i] = false;
        }
        en_kısayol[baslangic] = 0;                                      // ve büyük değer atadıktan sonra başlangıç olarak referans aldığım plakanın dizisine 0 değerini atadık
        int[] komsuluk_dizisi = new int[uzunluk];
        komsuluk_dizisi[baslangic] =  -1;                               // fonksiyona hangi şehir gönderiliyorsa dizinin o indexine -1 değeri atıyoruz

        for (int i = 2; i < uzunluk; i++) {                            // 2den başlayarak uzunluk yani 82ye kadar for dönecek
            int sehir_index = -1;
            int referans = Integer.MAX_VALUE;
            for (int j = 1; j < uzunluk; j++) {
                if (!ugrandi_Mi[j] && en_kısayol[j] < referans) {   // buradaki ifte uğranmamış olacak ve sonsuzdan değerden daha küçük bir değer arayana kadar kontrol edecek ardından sonsuz değer değiştikten sonra en kısa gidebilecek mesafeyi buluyor
                    sehir_index = j;
                    referans = en_kısayol[j];
                }
            }

            ugrandi_Mi[sehir_index] = true;

            for (int k = 1; k < uzunluk; k++) {
                int komsu_mesafe = komşu_maliyet[sehir_index][k];                //gelen şehir için komşu maliyet matrisinde komsu_mesafeye atama işlemi yapıyoruz

                if (komsu_mesafe > 0 && ((referans + komsu_mesafe) < en_kısayol[k])) // eğer komşı_mesafe matisinden 0dan farklı yani komşusu ise kontrol ediliyor
                {
                    komsuluk_dizisi[k] = sehir_index;                              //fonksiyona ilk gelen şehirden komşusu olan diğer şehirlerin dizisini oluşturuyor
                    en_kısayol[k] = referans + komsu_mesafe;                        //fonksiyona ilk gelen şehirden komşusu olan diğer şehirlerin maliyetini oluşturuyor
                }
            }
        }
        return en_kısayol;
    }

    static int faktoriyel(int sayi) {
        int carpim = 1;
        for (int i = 1; i <= sayi; i++) {
            carpim *= i;
        }
        return carpim;
    }

    static int plaka_bul(String s) {
        int p = 0;
        for (p = 0; p < 81; p++) {

            if (sehir[p].sehir_adi.equals(s)) {
                break;
            }
        }
        return sehir[p].plaka_kodu;
    }
    public static void main(String[] args) {
        Point[] points = new Point[82];
        String line;
        String[] bol;
        int[] plaka;
        int temp, sayac = 0;
        int i = 0, k = 0;
        int[][] komsu_maliyet = new int[82][82];
        String path = System.getProperty("user.dir") + "\\Koordinatlar.txt";
        try {
            FileReader dosya2 = new FileReader(path);
            BufferedReader oku1 = new BufferedReader(dosya2);
            i = 1;
            while (oku1.ready()) {
                String[] split = oku1.readLine().split(",");
                int x = Integer.parseInt(split[0]);
                int y = Integer.parseInt(split[1]);
                points[i++] = new Point(x, y);
            }
        } catch (final Exception e) {
            e.printStackTrace();
        }
        i = 0;
        try {
            String path1 = System.getProperty("user.dir") + "/sehirler.txt";
            FileReader dosya = new FileReader(path1);
            BufferedReader oku = new BufferedReader(dosya);
            line = oku.readLine();
            while (line != null) {
                bol = line.split(",");
                sehir[i] = new sehirler(bol.length - 2);                          // Şehir adında yeni bir class oluşturduk ve oluşturduğumuz anda classın parametreli constructorına komsu sayısını attık
                sehir[i].sehir_adi = bol[1];                                      // okuduğuz text şehrin adını classtaki şehrin adına yazdım
                sehir[i].plaka_kodu = Integer.parseInt(bol[0]);                  // aynı şekilde plakasını da
                for (int j = 2; j < bol.length; j++) {
                    sehir[i].komsu[k] = bol[j];                                  // bu adımda ise atanan şehrin komşularının isimlerini tuttuk
                    k++;
                }
                k = 0;
                i++;
                line = oku.readLine();
            }

        dosya.close();
        } catch (IOException e) {
            System.out.println("dosya bulunamadi");
        }
        try {
            String path2 = System.getProperty("user.dir") + "/mesafeler.txt";
            FileReader dosya1 = new FileReader(path2);
            BufferedReader oku = new BufferedReader(dosya1);
            line = oku.readLine();
            while (line != null) {
                bol = line.split(",");
                for (i = 0; i < sehir[sayac].komsu.length; i++) {                 // for 81 şehir için tuttuğumuz komsu sayısına dönerek maliyetlerini şehir classındaki maliyetlerine atarak her şehir için özdeşleştirdik

                    sehir[sayac].komsu_maliyet[i] = Integer.parseInt(bol[i + 1]);

                }
                sayac++;
                line = oku.readLine();
            }
        dosya1.close();
        } catch (IOException e) {
            System.out.println("dosya bulunamadi");
        }

        for (i = 0; i < 81; i++) {
            for (int l = 0; l < sehir[i].komsu.length; l++) {
                temp = plaka_bul(sehir[i].komsu[l]);                              // bu adımda plaka_bul fonksiyonu yaparak komşu şehirlerin plakasını aldık
                komsu_maliyet[i + 1][temp] = sehir[i].komsu_maliyet[l];          //aldığız plakaları komşu matris yaparak onlara atadım
            }
        }
        sayac = 0;
        Random r = new Random();
        Scanner scan = new Scanner(System.in);
        System.out.println("kac adet sehir gireceksiniz(MAX 8 şehirde çalışır)");
        int sehir_sayisi = scan.nextInt() + 1;
        int[] sehirler = new int[sehir_sayisi + 1];
        int[][] komsu_dizi = new int[sehir_sayisi + 1][82];
        int[] en_kisayol = new int[82];
        int sayac1 = 0;
        int[] dizi = new int[82];
        int[][] maliyet = new int[sehir_sayisi + 1][82];
        System.out.println("Sehirlerin plaka kodlarını giriniz(Kocaeli başlangıç şehir olarak belirlenmiştir.Girilmesine gerek yoktur):");
        sehirler[1] = 41;
        for (i = 2; i <= sehir_sayisi; i++) {
            sehirler[i] = scan.nextInt();
        }
        System.out.println();
        for (int kl = 1; kl <= sehir_sayisi; kl++) {
            en_kisayol = enkisa_yol(komsu_maliyet, sehirler[kl]);                        // enkisa_yol fonksiyona şehirleri göndererek onların hangi şehire komşu olduklarını dizi değişkene atayıp 3 alt satırda o dönen diziyi komsu_dizisine atıyoruz

            dizi = maliyet(komsu_maliyet, sehirler[kl]);                         // maliyet fonksiyonnuna gönderilen şehirlerin diğer şehirlere olan mesafelerini tutuyor bir alt satırda da maliyet dizisine kopyalıyoruz
            System.arraycopy(dizi, 0, maliyet[kl], 0, 82);
            System.arraycopy(en_kisayol, 0, komsu_dizi[kl], 0, 82);

        }
        int j = 1;
        Point[] point = new Point[sehir_sayisi + 1];                             // x ve y noktalarını tutacak bir point nesnesi tanımladık
        String rakamlar = "";
        int[] degisken = new int[faktoriyel(sehir_sayisi)];
        int[] gezilecek_sehir = new int[11];
        int sehir_syisi = sehir_sayisi;
        int sayi_tut;
        Harita hm = new Harita();
        int dizi_indx = 0, sayi = 0;
        List<Integer>[] array;
        array = new List[faktoriyel(sehir_sayisi)];
        for (int l = 0; l < faktoriyel(sehir_sayisi); l++) {
            array[l] = new ArrayList<Integer>();
        }
        long start = System.currentTimeMillis();
        int abs;
        if (sehir_sayisi == 9) {
            abs = 123456789;
        } else {
            abs = (int) Math.pow(10, sehir_sayisi - 1);
        }
        for (i = abs; i < (int) Math.pow(10, sehir_sayisi - 1) * 2; i++) {
            String string = "";
            string += i;                                                                                             // for da artan i değerini her seferinde string değişkenine ekliyoruz
            char[] sirala = string.toCharArray();
            Arrays.sort(sirala);                                                                                   // diziye çevrilmiş olan String burda sıralıyoruz 15342 gibi sayıyı 12345 olarak sıralayıp aşağıda referans olarak verdiğim değerle karşılaştırıyoruz hepsi birbirinden farklı mı diye 
            for (j = 1; j <= sehir_sayisi; j++) {
                rakamlar += j;                                                                                     // bu kısımda kullanici kaç şehir girdi ise ona göre rakamları alıyoruz karşılaştırma yaparken referans olarak
            }                                                                                                     //örnek veriyorum kullanıcı 5 tane şehir girecekse 12345 sayılarını alıyoruz
            if (String.valueOf(sirala).equals(rakamlar)) {                                                        // karşılaştırma yapılıyor
                sayi_tut = i;
                while (0 < i) {
                    gezilecek_sehir[sehir_syisi] = i % 10;                                                        // burda eğer şart sağlanır bütün sayılar birbinden farklı oluyorsa o sayıların modunu alarak gezilecek şehirlere aktarıyoruz
                    i = i / 10;
                    sehir_syisi--;
                }
                int sehir_plaka;
                for (i = 1; i < sehir_sayisi; i++) {
                    sehir_plaka = sehirler[gezilecek_sehir[i]];                                                  //kullanıcıdan aldığız şehirlerin plakalarını sehir_plaka değişkenine atıyoruz yukarı da kontrol işleminde doğru olanları yani 15234 şartı sağlıyorsa ilk önce 1 sonra 5 diye alıyoruz
                    do {

                        if (sehir_plaka != -1) {
                            array[dizi_indx].add(sehir_plaka);                                                   //eğer plaka -1 değil ise arraylist e atıyoruz ilk plakayı
                        }

                        sehir_plaka = komsu_dizi[gezilecek_sehir[i + 1]][sehir_plaka];                           // yukarı da oluşturduğumuz komşu dizisinde gezilecek şehir i+1 diyerek olası kombinasyonlar da kocaelinden sonra hangi geliyorsa onu şehir plakasına atıyoruz örnek vermek gerekirse ilkten kocaelinden adanaya gidilecek o yolları arraylist e atıyorum
                                                                                                                 //komsu_dizi[1][41]=54 eşit kocaelinin komşusu olduğu için ardından komsu_dizi[1][54]=14 gibi böyle devam ediyor komsu_dizi[1][1] olduğu vakit -1 dönüp çıkıyor 
                    } while (sehir_plaka != -1);
                    degisken[dizi_indx] += maliyet[gezilecek_sehir[i]][sehirler[gezilecek_sehir[i + 1]]];         //bu sırada da maliyetleri toplayıp bütün olası maliyetleri atıyorum değişken dizisine kullanıcı 5 şehir girdiyse 120 farklı maliyet barındırıyor
                }
                sehir_plaka = sehirler[gezilecek_sehir[i]];                                                       // gittiğim en son yerdeki plakayı alıyoruz ve sehir_plaka da tutuyoruz kocaelinden->adana->afyon gitti afyonun plakasını burda tutuyoruz
                do {

                    if (sehir_plaka != -1) {
                        array[dizi_indx].add(sehir_plaka);
                    }
                    sehir_plaka = komsu_dizi[gezilecek_sehir[1]][sehir_plaka];                                   //burda örnek verdiğim o afyonun plakasını matrisin sütun kısmına yazıyoruz gezilecek şehir de kocaelini tuttuğu için en son ki birleştirme yani afyondan kocaeline olan yolu da bu kısımda yazıyor

                } while (sehir_plaka != -1);
                degisken[dizi_indx] += maliyet[gezilecek_sehir[i]][41];                   //son olarak afyondan kocaeline olan maliyete de ekliyoruz
                dizi_indx++;
                i = sayi_tut;
            }                                                                             //değişkenler bir sonraki for için hazır olsun diye başlangıçtaki değerleri veriyoruz

            rakamlar = "";
            sehir_syisi = sehir_sayisi;
        }
        File dosya_1 = new File("Output.txt");
        ArrayList<Integer> buffer = new ArrayList<Integer>();
        for (i = 0; i < faktoriyel(sehir_sayisi - 1) - 1; i++) {                          //yukarıda almış olduğuz kullanıcı 5 tane şehir girdiyse o şehirlerin maliyetlerini küçükten büyüğe doğru sıralama işlemi yapıyoruz
            for (j = i + 1; j < faktoriyel(sehir_sayisi - 1); j++) {
                if (degisken[i] > degisken[j]) {
                    int tmp3 = degisken[i];
                    degisken[i] = degisken[j];                                             //burada maliyetleri sıralarken
                    degisken[j] = tmp3;
                    buffer = (ArrayList<Integer>) array[i];                               // bu kısımda ise yolları sıralıyor maliyetlerle aynı şekilde ilerlesin diye
                    array[i] = array[j];
                    array[j] = buffer;
                }
            }

        }
        long end = System.currentTimeMillis();
        long cikan_sonuc=end-start;
        System.out.println("Algoritanın tamamlanma süresi: "+(double)cikan_sonuc/1000);
        System.out.println("\n\n");
        Point[][] nokta = new Point[5][sehir_sayisi + 1];                                  //harita da çizeceğiz noktalar için yeni bir point nesnesi oluşturuyoruz
        if (sehir_sayisi <= 2) {
            try {
                FileWriter dosya_yaz = new FileWriter(dosya_1);
                BufferedWriter writer = new BufferedWriter(dosya_yaz);

                for (i = 0; i < 1; i++) {
                    writer.write("(En Kısa Yol: ");
                    writer.write(String.valueOf(degisken[i]));
                    writer.write(")");
                    System.out.print("(En Kısa Yol: " + degisken[i] + ")");
                    for (j = 0; j < array[i].size(); j++) {                             //bu kısımda ise yazma işlemini hem dosyaya hemde ekrana yazdırıyorum maliyetleriyle birlikte
                        writer.write(String.valueOf(array[i].get(j)));
                        writer.write("  ");
                        System.out.print(" " + array[i].get(j));
                    }
                    writer.write("\n");
                    System.out.println("");
                }
                writer.close();
            } catch (IOException e) {
                System.out.println("Dosya yazma işlemi başarısız  " + e);
            }
        } else if (sehir_sayisi == 3) {
            try {
                FileWriter dosya_yaz = new FileWriter(dosya_1);
                BufferedWriter writer = new BufferedWriter(dosya_yaz);

                for (i = 0; i < 2; i++) {
                    writer.write("(En Kısa Yol: ");
                    writer.write(String.valueOf(degisken[i]));
                    writer.write(")");
                    System.out.print("(En Kısa Yol: " + degisken[i] + ")");
                    for (j = 0; j < array[i].size(); j++) {                             //bu kısımda ise yazma işlemini hem dosyaya hemde ekrana yazdırıyorum maliyetleriyle birlikte
                        writer.write(String.valueOf(array[i].get(j)));
                        writer.write("  ");
                        System.out.print(" " + array[i].get(j));
                    }
                    writer.write("\n");
                    System.out.println("");
                }
                writer.close();
            } catch (IOException e) {
                System.out.println("Dosya yazma işlemi başarısız  " + e);
            }
        } else {
            try {
                FileWriter dosya_yaz = new FileWriter(dosya_1);
                BufferedWriter writer = new BufferedWriter(dosya_yaz);

                for (i = 0; i < 5; i++) {
                    writer.write("(En Kısa Yol: ");
                    writer.write(String.valueOf(degisken[i]));
                    writer.write(")");
                    System.out.print("(En Kısa Yol: " + degisken[i] + ")");
                    for (j = 0; j < array[i].size(); j++) {                             //bu kısımda ise yazma işlemini hem dosyaya hemde ekrana yazdırıyorum maliyetleriyle birlikte
                        writer.write(String.valueOf(array[i].get(j)));
                        writer.write("  ");
                        System.out.print(" " + array[i].get(j));
                    }
                    writer.write("\n");
                    System.out.println("");
                }
                writer.close();
            } catch (IOException e) {
                System.out.println("Dosya yazma işlemi başarısız  " + e);
            }
        }

        if (sehir_sayisi <= 3) {
            for (i = 0; i < sehir_sayisi - 1; i++) {
                sayi = 0;

                nokta[i][0] = new Point(points[41].x, points[41].y);
                for (j = 0; j < array[i].size() - 1; j++) {
                    if (array[i].get(j) == array[i].get(j + 1)) //5 tane yol içinde dolaşıyorum bir şehire gelip o şehirden çıkarken plakasını 2 kere tekrar edene bakıyorum yani o şehre uğranmış mı diye referans alıyorum
                    {
                        sayi++;
                        nokta[0][sayi] = new Point(points[array[i].get(j)].x, points[array[i].get(j)].y);          //eğer o şehre uğrandıysa kendim oluşturduğum koordinatlar txtden okuduğum koordinatları noktalar değişkenine atıyorum             
                    }
                }
                nokta[0][++sayi] = new Point(points[41].x, points[41].y);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                }
                hm.nokta(nokta, 1, sehir_sayisi + 1);
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                }
                hm.run();

                System.out.println("");
            }

        } else {
            sayi = 0;
            nokta[0][0] = new Point(points[41].x, points[41].y);
            for (j = 0; j < array[0].size() - 1; j++) {
                if (array[0].get(j) == array[0].get(j + 1)) //5 tane yol içinde dolaşıyorum bir şehire gelip o şehirden çıkarken plakasını 2 kere tekrar edene bakıyorum yani o şehre uğranmış mı diye referans alıyorum
                {

                    sayi++;
                    nokta[0][sayi] = new Point(points[array[0].get(j)].x, points[array[0].get(j)].y);          //eğer o şehre uğrandıysa kendim oluşturduğum koordinatlar txtden okuduğum koordinatları noktalar değişkenine atıyorum

                }
            }
            nokta[0][++sayi] = new Point(points[41].x, points[41].y);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            hm.nokta(nokta, 1, sehir_sayisi + 1);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            hm.run();

            System.out.println("");
            sayac = 0;
            for (i = 1; i < 5; i++) {
                sayi = 0;
                nokta[sayac][0] = new Point(points[41].x, points[41].y);
                for (j = 0; j < array[i].size() - 1; j++) {
                    if (array[i].get(j) == array[i].get(j + 1)) //5 tane yol içinde dolaşıyorum bir şehire gelip o şehirden çıkarken plakasını 2 kere tekrar edene bakıyorum yani o şehre uğranmış mı diye referans alıyorum
                    {
                        sayi++;
                        nokta[sayac][sayi] = new Point(points[array[i].get(j)].x, points[array[i].get(j)].y);          //eğer o şehre uğrandıysa kendim oluşturduğum koordinatlar txtden okuduğum koordinatları noktalar değişkenine atıyorum

                    }
                }
                sayac++;
                nokta[sayac][++sayi] = new Point(points[41].x, points[41].y);
            }

            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            hm.nokta(nokta, 4, sehir_sayisi + 1);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
            }
            hm.run();
        }

    }

}

------------------------------------------------------------------------

//Harita.java Jframe oluşturulan class


package prolab2;


import java.awt.Graphics;
import java.awt.Color;
import java.awt.Point;


public class Harita extends javax.swing.JFrame {

    public static Point[][] points;
    public static int degisken = 0;
    public static int N_satir;
    public Harita() {
         initComponents();

    }
    public static void nokta(Point point[][], int satir,int sutun) {
        N_satir=satir;
        points = new Point[satir][sutun];
        for(int j=0;j<satir;j++)
        {
            for (int i = 0; i < sutun; i++) {
           points[j][i] = new Point(point[j][i].x, point[j][i].y);
        }
        }      
        degisken = sutun;      
    }


    @SuppressWarnings("unchecked")
    // <editor-fold defaultstate="collapsed" desc="Generated Code">                          
    private void initComponents() {

        popupMenu1 = new java.awt.PopupMenu();
        popupMenu2 = new java.awt.PopupMenu();
        jLabel2 = new javax.swing.JLabel();

        popupMenu1.setLabel("popupMenu1");

        popupMenu2.setLabel("popupMenu2");

        setDefaultCloseOperation(javax.swing.WindowConstants.EXIT_ON_CLOSE);

        jLabel2.setIcon(new javax.swing.ImageIcon(getClass().getResource("/prolab2/4lturkiye-siyasi-haritasi2.png"))); // NOI18N
        jLabel2.setText("jLabel2");
        jLabel2.addMouseListener(new java.awt.event.MouseAdapter() {
            public void mouseClicked(java.awt.event.MouseEvent evt) {
                jLabel2MouseClicked(evt);
            }
        });

        javax.swing.GroupLayout layout = new javax.swing.GroupLayout(getContentPane());
        getContentPane().setLayout(layout);
        layout.setHorizontalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addComponent(jLabel2, javax.swing.GroupLayout.PREFERRED_SIZE, 1004, javax.swing.GroupLayout.PREFERRED_SIZE)
        );
        layout.setVerticalGroup(
            layout.createParallelGroup(javax.swing.GroupLayout.Alignment.LEADING)
            .addComponent(jLabel2)
        );

        pack();
    }// </editor-fold>                        

    private void jLabel2MouseClicked(java.awt.event.MouseEvent evt) {                                     

    }                                    
    @Override
    public void paint(Graphics g) {
        super.paintComponents(g);
        if(N_satir==1)
            g.setColor(Color.RED);
        else
            g.setColor(Color.BLACK);
        for(int j=0;j<N_satir;j++)
        {
             for (int i = 0; i < degisken-1; i++) {

            g.drawLine(points[j][i].x, points[j][i].y, points[j][i + 1].x, points[j][i + 1].y);
           
        }
        }
       

    }

   
    public void run() {
        new Harita().setVisible(true);

    }
    // Variables declaration - do not modify                     
    private javax.swing.JLabel jLabel2;
    private java.awt.PopupMenu popupMenu1;
    private java.awt.PopupMenu popupMenu2;
    // End of variables declaration                   
}


------------------------------------------------------------------------

//sehirler.java sehirler classı
package prolab2;


public class sehirler {
    String sehir_adi;
    int plaka_kodu;
    String [] komsu;
    int [] komsu_maliyet;
      public sehirler()
    {
        
    }
    public sehirler(int degisken)
    {
      komsu=new String[degisken];
      komsu_maliyet=new int[degisken];
    }
    
}



