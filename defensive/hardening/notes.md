# Utwardzanie systemu operacyjnego

# Set UID root
Programy ktore maja bit set user ID i program nalezy do uzytkownika roota sa bardzo niebezpieczne, poniewaz nie wazne kto odpala program, program wykonuje sie z uprawnieniami roota, ktory moze zrobic wszystko. Wobec powyzszego nalezy minimalizowac liczbe programow SUID root. Nalezy sprawdzic jakie programy sa uruchomione i te ktore nie sa niezbedne nalezy usunac.

# Lokalizacja plikow SUID/GUID
- find / -perm -u+s -user root -type f -print 2>/dev/null
- find / -perm -g+s -group root -type f -print 2>/dev/null
  
Odbieranie bitow suid
- chmod u-s filename
- chmod g-s filename

Minimalny zestaw SUID:
- passwd, ping, traceroute, sudo, su 

# Po instalacji
- zaktualizuj wszystkie pakiety obowiazkowo
- wylacz domyslne, niepotrzebne uslugi
  - nfsd
  - nfsclient
  - sendmail, postfix
  - FTP
  - CUPS
  - inetd, xinetd
- potrzebne uslugi: 
  - SSH
  - zero-conf, avachi (do pobrania informacji na temat sieci)
  
# Przeglad aktywnych uslug
netstat -ntlup (pokazuje jaka usluga na jakim porcie)

# Dzielenie dysku na partycje
Mozna podzielic dysk na partycje i przydzielic im odpowiednie atrybuty.  

cat /etc/fstab (pokazuje jakie flagi sa przydzielone do jakich partycji)

- /dev/sda1 / ext4 (domyslny dysk)
- /dev/sda2 /boot ext4 defaults, nosuid, nodev 1 2 (tylko pliki z ktorych system jest bootowany tylko jadro, nie musi zawierac plikow suid ani plikow urzadzen ktore sie odwoluja do jadra)
- /dev/sda3 /home ext4 defaults, nosuid, nodev 0 1 2 (bez suid i bez plikow urzadzen!) 
- /dev/sda4 swap swap 
- /tmp tmpfs nosuid, nodev 0 0
- /dev/sda5 /mnt/virtual ext4 defaults, nosuid, nodev, noexec 1 2 (zeby dac flage noexec to trzeba byc pewnym ze tam nie bedzie plikow wykonywalnych!)

# Lokalna zapora sieciowa
- ustawic maksymalna liczbe logowan na ssh (np maksymalnie 8 logowan w ciagu 5 minut)
- ipv6 jest od razu podniesiony do gory, ale lokalnie
- nalezy tez wdrozyc zapore sieciowa na ipv6 aby nasz komputer nie byl widoczny w sieci lokalnej! 
- po wdrozeniu tej konfiguracja zadna usluga poprzez ipv6 nie bedzie dostepna z zewnatrz na naszym komputerze
  - ip6tables -P INPUT DROP
  - ip6tables -P FORWARD DROP
  - ip6tables -P OUTPUT ACCEPT
  - ip6tables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
  - ip6tables -p ipv6-icmp -j ACCEPT
  - ip6tables -A INPUT -i lo -j ACCEPT

# Izolacja procesow sieciowych
Nie mozemy czasem wylaczyc wszystkich portow bo musza te uslugi dzialac. Jesli ktos przejmie nasza usluge to on laduje w naszym systemie operacyjnym z takim identyfikatorem jaki ta usluga posiadala. Oczywistym jest, ze takie uslugi nie powinny chodzic jako root! Jesli ktos przejmie usluge ktora pracuje z przywilejami root to jest juz pozamiatane. 

## chroot (syscall, metoda) 
dla danego procesu mozna zmienic wierzcholek systemu plikow. Mozna wskazac procesowi ktory system plikow jest jego wierzcholkiem. Wtedy nawet jesli nasz intruz sie eskaluje do roota to nam nie wyskoczy poza ten sztuczny system plikow. W pewnych wypadkach jest to omijalne. Nawet jesli nasz intruz wejdzie do chroota i w jakis sposob wejdzie na konto prawdziwego roota to bedzie mogl wyskoczyc. 

## docker / wirtualizacja
Do izolacji procesow sieciowych od systemu nikt nie bedzie stosowal wirtualizacji bo jest za ciezka. Jesli mam serwer ktory ma kilka uslug na sobie to za duzo by bylo tej roboty z maszynami wirtualnymi. Zatem korzysta sie z dockera.  
Wirtualne maszyny maja swoje wlasne jadra i systemy operacyjne.  

## docker
Zloty srodek. Jednak troche ciezko moze to byc zastosowac. Dla krytycznych systemow gdy chcemy odseparowac proces od reszty systemu to warto zastosowac dockera.  
Dockery maja wspolne jadro systemowe, ale maja swoje wlasne biblioteki. Kazdy w dockerze ma indywidualny zestaw systemu plikow ktory widzi, wraz z bibliotekami itd. Zdockeryzowane procesy sa traktowane jak kazde inne procesy i to jest szybkie. W dockerze mamy jedno jadro i wiele odrebnych systemow plikow (podobne do chroot). Docker korzysta z innych mechanizmow kernela do izolacji procesow niz chroot (sa bezpieczniejsze). W chroot mamy wspolne jadro procesu chrootowanego i glownego i wspolny dostep do jadra. Proces chroot'owany widzi inne procesy uruchomione w normalny sposob. W dockerze mamy nowe API w kernelu i zdockeryzowany proces nie widzi innych procesow z innych dockerow i z maszyny hosta! 

## ostatnia metoda 
Kazdy proces sieciowy powinien pracowac z innym identyfikatorem sieciowym, jest to bardzo stary pomysl. Kiedys robilo sie to recznie: procesy sie uruchamialy jako root, a potem z jakims innym uzytkownikiem. A aktualnie teraz kazda usluga sieciowa ma swojego dedykowanego uzytkownika i jak ktos przejmie usluge to wtedy bedzie pracowal jako uzytkownik uslugi, ktory nie ma mozliwosci dostepu do plikow.  
Tylko procesy z identyfikatorem 0 (root) moga pracowac na portach wyzszych niz 1024. Jednak da sie to obejsc, mozemy uruchomic proces, zwiazac sie z portem o niskim numerze a nastepnie zmienic uzytkownika na ktorym pracuje. (bind, setuid). Zwykle domyslne zachowanie!

# Utwardzanie jadra
Ustawianie zmiennych w /etc/sysctl.conf  
/etc/sysctl.conf (rozne ustawienia dla jadra tak aby pracowal bezpieczniej, ale niekonieczne szybciej i latwiej)

## TCP/IP
- net.ipv4.conf.all.log_martians(exp: 1) (czyli na interfejsie na ktorym adresacja jest okreslona pojawil sie pakiet ktorego tam byc nie powinno - bedziemy logowac takie pakiety dzieki tej opcji)
- net.ipv4.conf.all.tcp_syncookies(exp: 1) (obrona przed atakiem DDOS, czyli bronimy sie przed spamem 3-way handshake)
- net.ipv4.conf.all.log_martians(exp: 1)

## Inne zmienne
- fs.suid_dumpables(exp: 0)
- kernel.dmesg_restrict(exp: 1) (mozna zablokowac logowanie bezposrednio z jadra systemowego, mozna ograniczyc go tylko do roota)

## Patche
Mozna jadro wzmocnic ale to wyjatkowo juz zaawansowane (dla osob ktore bardzo sie staraja). 
- GRSecurity
