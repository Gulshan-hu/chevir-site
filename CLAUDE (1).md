# Chevir — chevirapp.com

Tək səhifəlik landing page. AZ və EN tam paralel. Bu fayl layihənin **nə** olduğunu deyir;
GSAP və taste skill-ləri **necə** yazılacağını deyir. İkisi ziddiyyət təşkil edərsə, bu fayl üstündür.

## Məhsul

Chevir Azərbaycan jest dili (AJD) ilə danışıq Azərbaycan dili arasında real vaxtda **ikitərəfli**
tərcümə qurur. Əvvəlcə API kimi çıxır, sonra mobil tətbiq. Auditoriya: dövlət qurumları, banklar,
xəstəxanalar (pilot tərəfdaş axtarırıq), qrant baxıcıları, və kar icma.

Saytın bir işi var: **pilot tərəfdaş müraciəti almaq**. Qalan hər şey bu məqsədə xidmət edir.

## Vizual istiqamət

Referanslar: fixaplan.com, midu.design, rabenrifaie.com — hər üçü Framer-də qurulub,
ilk ikisi eyni studiyanın (Midu) işidir.

Götürdüyümüz: **yumşaq və sakit premium**. Geniş boşluq, iri məhsul görüntüləri,
yuvarcıq formalar, bir dənə gradient/shader anı, sticky bölmələr.

Götürmədiyimiz: fon musiqisi (rabenrifaie işlədir — kar auditoriya üçün qurulan saytda
qəbuledilməzdir), aqressiv brutalizm, sıx qəzet layoutu.

## Rəng

```
--ink        #1A1817   əsas mətn
--paper      #F1F0EE   fon (KREM DEYİL — soyuq neytral)
--signed     #D6006C   jest kanalı (loqodan)
--spoken     #0088B0   səs kanalı (loqodan)
--muted      #6E6965
```

**Kanal məntiqi — saytın əsas qaydası.** Loqo iki əks istiqamətli qövsdür; bu, bütün saytın
qrammatikasıdır. Kar vətəndaşın kanalına aid hər şey magenta, eşidən tərəfin kanalına aid hər şey
mavi. Neytral məzmun ink. Bu iki rəng dekorasiya deyil, naviqasiya sistemidir — təsadüfi işlətmə.

Tünd rejim `prefers-color-scheme` ilə dəstəklənir.

## Tipoqrafiya

Bir və ya iki ailə, artıq yox. Yumşaq premium üçün geometrik/qrotesk uyğundur, serif yox.

**Kritik:** şrift Azərbaycan hərflərini dəstəkləməlidir. Seçməzdən əvvəl mütləq yoxla:
`ə Ə ğ Ğ ı İ ş Ş ç Ç ö Ö ü Ü`. `ə` (schwa, U+0259) ən çox əskik olan hərfdir və
onsuz sayt oxunmur. Namizədlər: Inter Tight, Manrope, Onest, Geist. Fontshare şriftlərini
(Satoshi, General Sans) yalnız `ə`-ni gördükdən sonra işlət.

Ölçü: `clamp()` ilə axıcı şkala. Sətir uzunluğu 80 simvoldan az.

## Texniki stek

```
lenis      hamar scroll — hər şeyin əsası
gsap       + ScrollTrigger, SplitText (indi pulsuzdur)
three      hero-dakı 3D əl nöqtə buludunda (dinamik import, yalnız orada yüklənir).
           prefers-reduced-motion və ya WebGL dəstəyi yoxdursa, heç yüklənmir —
           statik logo.svg-ə enir.
```

Lenis-i ScrollTrigger ilə sinxronlaşdır (README-dəki rəsmi pattern: `lenis.on('scroll',
ScrollTrigger.update)` + `gsap.ticker.add`, `lagSmoothing(0)`).

React işlədirsənsə `useGSAP` hook-u və `gsap.context()` cleanup-ı məcburidir.

Navbar həmişə tünd rejimdədir, çünki bütün bölmələr tünddür. Açıq fonlu bölmə
əlavə olunarsa, navbar keçid məntiqi yenidən qurulmalıdır.

Deploy: Vercel. Domen: chevirapp.com.

## Motion spesifikasiyası

Beş adlandırılmış an. Bunlardan artığı yoxdur — hər bölməyə effekt əlavə etmə.

**1. Preloader.** Loqonun iki qövsü əks istiqamətdə öz-özünü çəkir (stroke-dashoffset),
sonra mərkəzi nöqtə, sonra wordmark. 0→100 sayğac. Maksimum 3.5 saniyə. Bir dəfə,
sessiyada təkrarlanmır.

**2. Hero.** "Çox sağ ol" videosu. Başlıq SplitText ilə söz-söz qalxır, hərf-hərf yox
(uzun Azərbaycan sözlərində hərf-hərf oxunuşu pozur). Aşağıda "scroll to explore" işarəsi.
Loqonun yerində 3D əl nöqtə buludu (Three.js, MediaPipe 21-nöqtə sxemi) yavaş fırlanır və
jest pozaları arasında keçir; kursor/toxunma ilə orbit edilə bilər. Ekrandan çıxanda
render dayanır. reduced-motion və ya WebGL yoxdursa, statik logo.svg qalır.

**3. Pipeline (sticky).** Ən vacib an. Sol tərəfdə görüntü pin olunur, sağda dörd addım
scroll olunur; hər addıma çatanda görüntü dəyişir və nömrənin rəngi kanal rənginə keçir
(01-02 magenta, 03-04 mavi). Fixa-dakı feature bölməsi kimi.

**4. Loop diaqramı.** Bölmə pin olunur, iki ox scroll ilə əks istiqamətlərdə çəkilir
(scrub: true). Bu, məhsulun bütün fikridir — ona görə tək dayanan an olsun.

**5. Shader.** Magenta-mavi gradient, yumşaq, yavaş. Yalnız "Bizə nə lazımdır" bölməsinin
fonunda. Hero-da yox — mobil cihazlarda ağırdır və auditoriyamızın çoxu zəif telefondan girir.

Ümumi qaydalar: yalnız `transform` və `opacity` animasiya olunur. Keçid müddəti 0.4–0.8s,
`power2.out`. Scroll-scrub olanlarda `scrub: 1` (0.5-dən az sərt görünür).

## Əlçatanlıq — güzəşt yoxdur

Bu, kar insanlar üçün qurulan məhsuldur. Aşağıdakılar estetik seçim deyil, tələbdir.

- `prefers-reduced-motion: reduce` seçilibsə **bütün** animasiya dayanır, Lenis söndürülür,
  səhifə tam funksional qalır.
- Heç bir məlumat yalnız animasiya vasitəsilə ötürülmür. Animasiya söndürüləndə məzmun tam olur.
- Video səssizdir, avtomatik oynayır, `playsinline`, və hər birinin yanında mətn izahı var.
- Fon musiqisi və səsli effekt yoxdur.
- Klaviatura naviqasiyası və görünən fokus halqası işləyir. Lenis anchor linkləri pozmamalıdır.
- Kontrast WCAG AA. Magenta `#D6006C` ağ fonda kiçik mətn üçün kifayət etmir — yalnız
  iri başlıq və qrafik element kimi işlət.

## Məzmun qaydaları

- Slayd 12 və 14-dəki 3D avatar və ekran yazısı **bizim deyil** (related work). Sayta salma.
- Avatar hələ hazır deyil. "Hazırlanır" kimi açıq göstər, gizlətmə və ya hazır kimi təqdim etmə.
- Büdcə rəqəmləri, SWOT-un zəif tərəflər sütunu və 150 000 AZN saytda yoxdur. Tələb yalnız
  "pilot tərəfdaş axtarırıq"dır.
- AZ və EN bərabər hüquqludur. EN "tərcümə" kimi görünməməlidir.
- Terminologiya: "kar" sözü işlədilmir. Həmişə "eşitmə məhdudiyyətli" yazılır.
  Bu, komandanın rəsmi qərarıdır.

## Qadağan siyahısı

Bunlar AI görünüşünün mənbəyidir:

- Hər bölmədə fade-up reveal
- Eyni radius və eyni boz kölgəli identik kartlar
- ALL-CAPS eyebrow etiketləri başlıqların üstündə
- Düymə mətnində `→`
- Krem fon (#F4F1EA və ya yaxını)
- Dekorativ gradient yuyuntusu
- Başlıqda tək sözü fərqli rəngə boyamaq
- Ardıcıllıq olmayan məzmunda 01/02/03 nömrələri
- Emoji ikonlar

## QA

Kod yazdıqdan sonra Playwright ilə 1440px və 390px-də screenshot çək və özün bax:

**Yoxlama zamanı yalnız xəta axtarma.** Konsolda xəta yoxdur, horizontal scroll yoxdur
kimi keçidlər kifayət deyil — layout tamamilə pozuq olub yenə də "təmiz" nəticə verə bilər.
Hər bölmənin ekran şəklini çək və görünüşünə bax:

- Konteyner düzgündürmü — məzmun kənara yapışmayıb, yan boşluqlar varmı
- Sütunlar həqiqətən sütundurmu (grid/flex işləyir, hamısı bir sütuna düşməyib)
- Bölmələr arası və bölmə daxili boşluqlar mövcud bölmələrlə eyni ritmdədirmi
- Media (video, şəkil) öz ölçüsündədirmi — bulanıq, uzadılmış və ya boş yerli deyilmi
- Yatay scroll varmı
- Mətn qutudan daşırmı, xüsusilə uzun Azərbaycan sözlərində
- `ə ğ ı İ ş ç ö ü` düzgün render olunurmu
- Tünd rejimdə kontrast qalırmı
- `prefers-reduced-motion` açıq olanda səhifə tam işləyirmi
- Lighthouse: mobil performans 80-dən yuxarı

## İş qaydaları

### Əvvəlcə plan, sonra kod
Heç bir tapşırığa birbaşa kod yazmaqla başlama. Əvvəlcə qısa plan təqdim et:
- Mövcud kodda nə tapdın, problemin səbəbi nədir
- Hansı faylları dəyişəcəksən və nə edəcəksən
- Hansı yanaşmanı seçdin və niyə, alternativ varsa qeyd et
- Silinməsi təklif olunan kodun başqa yerdə işlədilib-işlədilmədiyi

Planı təsdiq gözlə. Yalnız təsdiqdən sonra kod yaz.
Bu qayda hər dəfə keçərlidir, hətta tapşırıq sadə görünsə belə.

### Kreditə qənaət
Sessiya limiti var, kredit vaxtından əvvəl bitməməlidir.
- Yalnız tapşırığa aid faylları oxu. Hansı faylı oxumaq lazım olduğu aydın
  deyilsə, kor-koranə açmaq əvəzinə soruş.
- Yalnız dəyişdirdiyin hissəni yoxla. Bütün saytı hər dəfə yenidən test etmə.
- Playwright yoxlamasını minimumda saxla: adətən iki ölçü kifayətdir.
  Tam QA yalnız açıq tələb olunanda.
- Xülasələri qısa yaz. Uğurla işləyən hər detalı sadalama, yalnız
  nəyi dəyişdiyini, hansı problemi tapdığını və nəyə diqqət lazım olduğunu yaz.

### Təmiz kod
- Ölü kod qalmasın: çağırılmayan funksiya, qaytarılmayan dəyəri destructure
  edən sətir, işlədilməyən import və ya CSS sinfi.
- Dəyişiklikdən sonra həmin modulun başqa modullarla əlaqəsini yoxla —
  bir tərəfi dəyişib digərini köhnə saxlama.
- Bir fayl həddindən artıq böyüyürsə (təxminən 300 sətirdən çox),
  bölünməsini təklif et, amma icazəsiz bölmə.
- Təkrarlanan dəyərləri (breakpoint, rəng, müddət) sabitə çıxar.

### Model və kredit səmərəliliyi
- Bu layihə Claude Sonnet ilə işlənir. Sonnet üçün effektiv işləmə tərzi:
  aydın, konkret tapşırıqlar qəbul et; qeyri-müəyyənlik olanda təxmin
  etmə, soruş.
- Fayl oxuma minimal olsun. Tapşırıqda hansı fayllar göstərilibsə,
  yalnız onları aç. Əlaqəli fayl lazım olduğunu düşünürsənsə, əvvəlcə
  bunu tapşırığı verənə de və icazə istə, özbaşına açma.
- Hər tapşırıqdan əvvəl qısa plan ver (bu, artıq əsas qaydadır).
  Planı yalnız zəruri qədər ətraflı yaz — hər sətri əsaslandırmaq
  lazım deyil, əsas qərarları izah et.
- Yoxlama mərhələsini tapşırığın həcminə uyğunlaşdır: kiçik düzəliş
  üçün bir-iki ölçüdə yoxlama kifayətdir, böyük struktur dəyişikliyi
  tam yoxlama tələb edir. Hər tapşırıqda avtomatik tam QA (bütün ölçülər,
  bütün rejimlər) etmə — yalnız açıq tələb olunanda.
- Xülasələr qısa olsun: nə dəyişdi, nə tapıldı (əgər bug varsa), nəyə
  diqqət lazımdır. Uğurla işləyən hər detalı sadalama.

### Nə vaxt soruşmaq, nə vaxt qərar vermək
- Aydın texniki seçim (dəyişən adı, fayl strukturu, hansı GSAP metodu)
  — özün qərar ver, soruşma.
- Dizayn və məzmun qərarı (rəng, mətn, layout istiqaməti) — tapşırıqda
  aydın deyilsə, soruş.
- Tapşırığın icazə verdiyi fayl siyahısından kənara çıxmaq lazım gələndə
  — mütləq soruş, özbaşına genişləndirmə.
- Silinməsi planlaşdırılan kodun başqa yerdə işlədilib-işlədilmədiyi
  aydın deyilsə — soruş, təxmin etmə.
- Uzun sürən və ya çətin alınan iş (bir saatdan çox, dəfələrlə uğursuz
  cəhd) — dayan və vəziyyəti bildir, sadələşdirilmiş alternativ təklif et.
