---
title: Wskaźniki
category: cpp
thumbnail: wskazniki.png
published: 2013-01-09T00:00:00Z
---
**Wskaźniki** są nieodłącznym elementem języka C. W języku C++ także są przydatne i korzystanie z nich ułatwia pracę, jednak w odróżnieniu do C wiele rzeczy da się osiągnąć bez ich użycia. Poprawne **operowanie wskaźnikami** znacznie przyśpiesza **wydajność programu**, zmniejsza zużycie pamięci oraz skraca kod źródłowy.

<!--more-->

## Czym są wskaźniki?

**Wskaźnik** (ang. *pointer*) - typ zmiennej odpowiedzialnej za przechowywanie adresu do innej zmiennej (innego miejsca w pamięci) w obrębie naszej aplikacji.
{: .alert-info} 

**Wskaźnik** może wskazywać na jakąś zmienną, strukturę, tablicę a nawet funkcję. Jest niczym innym jak zmienną, która nie przechowuje wartości, tylko wskazuje na inną zmienną istniejącą w aplikacji. Oto podstawowe operatory niezbędne do operowania wskaźnikami:

- *\** - **operator wyłuskania wartości** zmiennej, na którą wskazuje wskaźnik (wyciąga wartość ze wskaźnika)
- *&* - **operator pobrania adresu** określonej zmiennej

## Po co są wskaźniki?

Wiele początkujących osób zastanawia się - **po co istnieją wskaźniki w C++**? Zanim przeczytasz resztę artykułu, postaram Ci się to przybliżyć.

W języku C++ możemy do funkcji **przekazać dowolną ilość parametrów**. Modyfikując parametry w obrębie ciała funkcji **oryginalne zmienne nie zmienią się**. Przekazując zmienne do funkcji poprzez wartość (czyli w sposób standardowy), tworzymy wewnątrz funkcji ich kopię! Co z tego faktu wynika? **Każda funkcja w C++ może zmodyfikować maksymalnie jedną zmienną**, za pomocą wartości zwracanej *return*.

Korzystając z mojego talentu graficznego zobrazowałem Ci przekazywanie parametrów do funkcji poprzez wartość. Jesteśmy w stanie zmodyfikować wartość zmiennej _a_, ale nie jesteśmy w stanie zmienić innych zmiennych.

{% include parts/postPicture.html page=page img="wskazniki1" %}

Co zrobić, aby jedną funkcją zmodyfikować 3 zmienne na raz? Nie można użyć rozkazu *return* 3 razy, ponieważ każda funkcja zwraca tylko jedną wartość. **W tej sytuacji trzeba skorzystać ze wskaźników**. Jeżeli przekażemy do funkcji jako jej argument **wskaźnik**, wtedy operacje na wskaźniku zmieniają zmienną oryginalną z poza ciała funkcji &#8211; nie operujemy na kopii zmiennej. Dzięki temu, nawet jeżeli funkcja jest typu *void* i nic nie zwraca, możemy **modyfikować wiele zmiennych** z poza ciała funkcji:

{% include parts/postPicture.html page=page img="wskazniki2" %}

## Jak używać wskaźników

**Zmienna wskaźnikowa** (czyli wskaźnik) poprzedzona jest gwiazdką _(*****)_ i przechowuje adres pamięci (a nie wartość) zmiennej , na którą wskazuje.

Deklarując **wskaźnik** postępujemy tak jak ze zwykłymi zmiennymi, jednak nazwę wskaźnika poprzedzamy gwiazdką. **Uwaga! Gwiazdka przed nazwą wskaźnika nie ma związku z operatorem wyłuskania!** <code class="cpp"></code>

	int telefon;    //zmienna liczbowa
	int *wsk;       //zmienna wskaźnikowa typu liczbowego

Tak wygląda tworzenie wskaźnika. Utworzone przez nas zmienne są puste. Zmienna telefon nie zawiera żadnej wartości a wskaźnik _wsk_ nie wskazuje żadnej wartości.

**Bardzo ważne jest** aby nie korzystać ze wskaźnika który **nie wskazuje na żadną zmienną**! Prowadzi to zawsze do błędów i niesie ze sobą **nieprzewidziane konsekwencje** w działaniu programu!
{: .alert-danger} 

Rozbudujmy powyższy przykład:

	int telefon = 12345;    //zmienna liczbowa
	int *wsk = &telefon;    //wskaźnik wsk zawiera adres zmiennej telefon

Za pomocą operatora pobrania adresu *(&)* pobraliśmy adres zmiennej telefon. Adres zmiennej został przypisany wskaźnikowi _wsk_. Pamiętaj że gwiazdka przed nazwą wskaźnika to nie operator wyłuskania! Chcąc wyświetlić wartość wskaźnika posłużymy się operatorem wyłuskania czyli gwiazdką *(*)*.

	#include <iostream>

	using namespace std;

	int main()
	{
	    int telefon = 12345;    //zmienna liczbowa
	    int *wsk = &telefon;    //wskaźnik wsk zawiera adres zmiennej telefon
	    
	    cout << *wsk << endl;
	    
	    return 0;
	}

Powyższy przykład wyświetli na ekranie wartość zmiennej telefon. Przed wyświetleniem wskaźnika został użyty operator wyłuskania. **Pobiera on wartość zmiennej spod adresu ze zmiennej wskaźnikowej**. Bez użycia operatora wyłuskania, została by wyświetlona wartość **zmiennej wskaźnikowej** *wsk* czyli adres zmiennej *telefon*:

	int telefon = 12345;         //zmienna liczbowa
	int *wsk = &telefon;         //przypisanie wskaźnikowi adresu zmiennej telefon
											         
	cout << *wsk << endl;        //wyświetlenie wyłuskanej wartości wskaźnika (12345)
	cout << wsk << endl;         //wyświetlenie adresu zmiennej telefon
	cout << &wsk << endl;        //wyświetlenie adresu wskaźnika
	cout << &telefon << endl;    //wyświetlenie adresu zmiennej telefon

Używając *sizeof* na **wskaźniku** nie mozna zapomnieć o operatorze wyłuskania. W przeciwnym wypadku wynikiem zawsze będzie ta sama wartość, ponieważ każda **zmienna wskaźnikowa** ma ten sam rozmiar w pamięci.

Rzeczą normalną są **wskaźniki do wskaźników**. Przy ich wyświetlaniu ważna jest ilość operatorów wyłuskania. Jeżeli do wskaźnika drugiego stopnia użyjemy jednego operatora wyłuskania wtedy otrzymamy adres wskaźnika a nie wartość zmiennej na którą wskazują.

	int telefon = 12345;    //zmienna liczbowa
	int *wsk = &telefon;    //wskaźnik wsk zawiera adres zmiennej telefon
	int **wsk2 = &wsk;      //wskaźnik wsk2 zawiera adres wskaźnika wsk
	
	cout << **wsk2 << endl;

Wskaźniki poddają się rzutowaniu typów. W dowolnym momencie możemy zmienić typ zmiennej wskaźnikowej np. z *int* na *long*.

Wartość wskaźnika (zmiennej na którą wskazuje) możemy modyfikować bez uzycia nazwy zmiennej:

	int telefon = 12345;    //zmienna liczbowa
	int *wsk = &telefon;    //wskaźnik wsk zawiera adres zmiennej telefon
	
	*wsk = 666;
	
	cout << *wsk << endl;

### Pusty wskaźnik 

Przed zapisaniem wartości do wskaźnika, czyli zapisaniu wartości do zmiennej na którą wskazuje wskaźnik, należy się upewnić, że wskaźnik nie jest pusty.

**Totalnie błędna** byłaby sytuacja, kiedy chcemy nadać zmiennej wskaźnikowej określoną wartość, bez wcześniejszego przypisania wskaźnika do zmiennej.
{: .alert-danger} 

	int *wsk;
	*wsk = 666; //źle!!

### Wskaźniki generyczne

Zwykłe wskaźniki używane są aby wskazywać na określony obiekt określonego typu, np zmienna tekstowa, zmienna liczbowa czy struktura. **Wskaźniki generyczne** dają nam możliwość wskazywania na określony obszar pamięci aplikacji bez ustalania typu wskaźnika. Oznacza to że wskaźnik generyczny będzie miał typ _void_.

	int telefon = 12345;    //zmienna liczbowa
	void *wsk = &telefon;   //wskaźnik generyczny wsk typu void

Przed wykonaniem operacji na wskaźniku generycznym i przed jego wyświetleniem, musimy określić wielkość na jaką wskazuje. W powyższym przykładzie jest to _int_: <code class="cpp"></code>

	#include <iostream>
	
	using namespace std;
	
	int main()
	{
	    int telefon = 12345;    //zmienna liczbowa
	    void *wsk = &telefon;   //wskaźnik generyczny wsk typu void
	    
	    *(int*)wsk = 666;
	    
	    cout << *(int*)wsk << endl;
	    
	    return 0;
	}

### Wskaźniki i funkcje

W języku C++ przekazujemy argumenty do funkcji poprzez tzw. **przekazywanie przez wartość**. W języku C oraz C++ możemy przekazywać wartości do funkcji poprzez **przekazywanie przez wskaźnik**. Wskaźnik będzie wtedy argumentem funkcji.

	#include <iostream>
	
	using namespace std;
	
	// funkcja przyjmuje jako argument wskaźnik
	void zwieksz_liczbe (int *liczba)
	{
	    *liczba+= 5;
	}
	
	int main()
	{
	    int numerek = 5;
	    int *wsk = &numerek;
	    
	    zwieksz_liczbe(wsk); //przekazujemy wskaźnik (bez operatorów)
	    
	    cout << numerek << endl;
	    
	    zwieksz_liczbe(&numerek); //przekazujemy bezpośrednio adres zmiennej (operator &)
	    
	    cout << numerek << endl;
	    
	    return 0;
	}

Należy zwrócić uwagę, że do funkcji której argumentem jest wskaźnik, przekazujemy adres zmiennej za pomocą operatora pobrania adresu _(&)_ a nie samą wartość.

Więcej o **przekazywaniu wskaźników** do funkcji dowiesz się w artykule: [przekazywanie argumentów do funkcji C++](/cpp/przekazywanie-argumentow-do-funkcji-c/).

### Wskaźniki i tablice

Tablice są sciśle związane ze wskaźnikami. Nazwa tablicy to wskaźnik na jej pierwszy element. Oznacza to że możemy wyświetlić pierwszy element tablicy umieszczając operator wyłuskania przed jej nazwą:

	int tablica[5] = {1, 2, 3, 4, 5};
	cout << *tablica << endl;

Możemy także tworzyć wskaźniki do określonych elementów tablicy:

	int tablica[5] = {1, 2, 3, 4, 5};
	int *wsk = &tablica[3];
	
	cout << *wsk << endl;

Stringi w C to tablica charów. Tworząc wskaźnik będzie to już wskaźnik do wskaźnika a nie zwykły wskaźnik.

	char* wyraz = "test";
	char **wsk = &wyraz;

Często spotykane są także tablice wskaźników. Ich tworzenie jest proste i analogiczne do innych typów zmiennych:

	#include <iostream>
	
	using namespace std;
	
	int main()
	{
	    int liczba1 = 1, liczba2 = 2, liczba3 = 3;
	    int *wsk[3];
	    
	    wsk[0] = &liczba1;
	    wsk[1] = &liczba2;
	    wsk[2] = &liczba3;
	    
	    cout << *wsk[1] << endl;
	    return 0;
	}

## Dynamiczna alokacja pamięci

Często podczas działania programu pojawia się problem **dynamicznego zarządzania pamięcią**. Problem fajnie można zaprezentować na przykładzie tablicy o różnej ilości elementów. Z pomocą przychodzą **wskaźniki** oraz operatory _new_ i _delete_.

Najpierw **tworzymy wskaźnik** określonego typu a następnie zmienną o dowolnej wielkości. Potem przydzielamy wskaźnikowi adres nowej zmiennej utworzonej na stercie:

	#include <iostream>
	
	using namespace std;
	
	int main()
	{
	    int *zmienna;
	    zmienna = new int[3];
	    zmienna[0] = 111;
	    
	    cout << *zmienna << endl;
	    
	    delete zmienna;
	    
	    return 0;
	}

W C odpowiednikami _new_ i _delete_ są operatory _malloc_ i _free_. Można używać operatorów C jak i C++, należy jednak pamiętać aby ich nie mieszać! (malloc + delete oraz new + free)!

**Dynamiczne tworzenie zmiennych** daje nam duże możliwości. Ważne jest aby usuwać je kiedy są niepotrzebne. W przeciwnym wypadku istnieje szansa wystąpienia błędu _memory leak_.