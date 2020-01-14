# Xamarin.Forms

## Shell

### Úvod

Zdroj: https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/introduction

[Príklad - Xaminals](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms Shell znižuje zložitosť vývoja mobilných aplikácií poskytovaním základných funkcií, ktoré väčšina mobilných aplikácií vyžaduje, vrátane:
* Jedno miesto na opísanie vizuálnej hierarchie aplikácie.
* Bežná skúsenosť používateľa navigácie.
* Navigačná schéma založená na URI, ktorá umožňuje navigáciu na ľubovoľnú stránku v aplikácii.
* Integrovaný obslužný program vyhľadávania.

Aplikácie Shell navyše profitujú zo zvýšenej rýchlosti vykresľovania a zníženej spotreby pamäte.

Existujúce aplikácie môžu využívať Shell a okamžite profitovať z vylepšenia navigácie, výkonu a rozšíriteľnosti.

#### Platform support

Xamarin.Forms Shell je plne k dispozícii pre iOS a Android, ale iba čiastočne je k dispozícii na platforme Universal Windows Platform (UWP). Okrem toho `Shell` v súčasnosti experimentuje na `UWP` a môže sa použiť iba pridaním nasledujúceho riadku kódu do triedy `App` vo vašom projekte UWP, skôr ako zavoláte `Forms.Init`:

````csharp
global::Xamarin.Forms.Forms.SetFlags("Shell_UWP_Experimental");
````

#### Skúsenosť s navigáciou v prostredí

`Shell` poskytuje premyslený zážitok z navigácie založený na flyouts a taboch. Najvyššou úrovňou navigácie v aplikácii `Shell` je v závislosti od požiadaviek navigácie aplikácie flyout alebo spodná lišta - tabs. Nasledujúci príklad zobrazuje aplikáciu, v ktorej je najvyššou úrovňou navigácie flyout:

![Flyout](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/introduction-images/flyout.png)

Výsledkom výberu rozbaľovacej položky bude spodná karta - tab, ktorá predstavuje vybratú a zobrazenú položku:

![Selected Menu Item](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/introduction-images/monkeys.png)

POZN: Ak nie je rozbaľovacia ponuka otvorená, dolná lišta kariet sa môže považovať za najvyššiu úroveň navigácie v aplikácii.

Každá karta zobrazuje stránku `ContentPage`. Ak však spodná karta obsahuje viac ako jednu stránku, stránky sa dajú navigovať pomocou horného panela kariet:

![Top Tabs](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/introduction-images/cats.png)

Na každej karte je možné navigovať ďalšie objekty `ContentPage`:

![ContentPage](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/introduction-images/cat-details.png)

### Vytvorte aplikáciu Xamarin.Forms Shell

Zdroj: https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/create

[Príklad - Xaminals](https://docs.microsoft.com/en-us/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Proces vytvorenia aplikácie Xamarin.Forms Shell je nasledujúci:

1. Vytvorte novú aplikáciu Xamarin.Forms alebo otvorte existujúcu aplikáciu, ktorú chcete prekonvertovať na aplikáciu Shell.
2. Pridajte do projektu zdieľaného kódu súbor XAML, ktorý zdedí z triedy `Shell`. Viac informácií nájdete v časti [Podtrieda triedy Shell](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/create#subclass-the-shell-class).
3. Nastavte property `[MainPage](https://docs.microsoft.com/en-us/dotnet/api/xamarin.forms.application.mainpage#Xamarin_Forms_Application_MainPage)` triedy App aplikácie na podtriedený objekt `Shell`. Ďalšie informácie nájdete v téme [Bootstrap the Shell](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/create#bootstrap-the-shell-application).
Opíšte vizuálnu hierarchiu aplikácie v podtriede triedy `Shell`. Ďalšie informácie nájdete v časti [Opíšte vizuálnu hierarchiu aplikácie](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/create#describe-the-visual-hierarchy-of-the-application).

#### Podtrieda triedy Shell

Prvým krokom pri vytváraní aplikácie Xamarin.Forms Shell je pridanie súboru `XAML` do projektu zdieľaného kódu, ktorý zdedí z triedy `Shell`. Tento súbor možno pomenovať akokoľvek, ale odporúča sa `AppShell` - konvencia. Nasledujúci príklad kódu zobrazuje novovytvorený súbor `AppShell.xaml`:

````xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell">

</Shell>
````

Nasledujúci príklad zobrazuje súbor "code-behind", `AppShell.xaml.cs`:
````csharp
using Xamarin.Forms;

namespace Xaminals
{
    public partial class AppShell : Shell
    {
        public AppShell()
        {
            InitializeComponent();
        }
    }
}
````

#### Prepojenie aplikácie so Shell

After creating the XAML file that subclasses the `Shell` object, the MainPage property of the App class should be set to the subclassed Shell object:

````csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
````
V tomto príklade je trieda `AppShell` súbor XAML, ktorý pochádza z triedy `Shell`.

POZN:
Keď sa vytvára prázdna aplikácia `Shell`, jej spustenie bude mať za následok hádzanie `InvalidOperationException` výnimky.

#### Opíšte vizuálnu hierarchiu aplikácie

Posledným krokom pri vytváraní aplikácie Xamarin.Forms Shell je opísať vizuálnu hierarchiu aplikácie v podtriede triedy `Shell`. Podtriedená trieda Shell pozostáva z troch hlavných hierarchických objektov:

* `FlyoutItem` alebo `TabBar`. `FlyoutItem` predstavuje jednu alebo viac položiek v bočnej lište a mal by sa použiť, keď vzor navigácie pre aplikáciu obsahuje bočnú lištu. `TabBar` predstavuje dolnú lištu kariet a mala by sa používať, keď sa navigačný vzorec aplikácie začína spodnými kartami. Každý objekt `FlyoutItem` alebo `TabBar` je potomkom objektu `Shell`.
* Karta, ktorá predstavuje zoskupený obsah, je navigovateľná spodnými kartami. Každý objekt `Tab` je potomkom objektu `FlyoutItem` alebo objektu `TabBar`.
* `ShellContent`, predstavuje objekty `ContentPage` vo vašej aplikácii. Každý objekt `ShellContent` je potomkom objektu `Tab`. Ak je na karte prítomných viac ako jeden objekt `ShellContent`, objekty budú navigovateľné hornými kartami.
* Žiadny z týchto objektov nepredstavuje žiadne používateľské rozhranie, ale skôr organizáciu vizuálnej hierarchie aplikácie. `Shell` tieto objekty vezme a vytvorí pre obsah navigačné používateľské rozhranie.

Nasledujúci XAML zobrazuje príklad podtriedy triedy Shell:

````xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
````

Pri spustení táto XAML zobrazí stránku `CatsPage`, pretože je to prvá položka obsahu deklarovaná v podtriede triedy `Shell`:

![CatsPage](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/create-images/cats.png)

Po stlačení ikony hamburgera alebo prejdení zľava sa zobrazí rozbaľovacia ponuka (bočná lišta):

![Flyout](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/create-images/flyout-reduced.png)

**POZOR:**

V aplikácii Shell sa pri spustení aplikácie vytvorí každá obsahová stránka, ktorá je potomkom objektu `ShellContent`. Pridanie ďalších objektov `ShellContent` pomocou tohto prístupu bude mať za následok vytvorenie ďalších stránok počas spúšťania aplikácie, čo môže viesť ku pomalému spusteniu. `Shell` je však tiež schopná vytvárať stránky na požiadanie v reakcii na navigáciu. Viac informácií nájdete v príručke [Efektívne načítanie stránky](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/tabs#efficient-page-loading) v príručke [Xamarin.Forms Shell Tabs](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/shell/tabs).

