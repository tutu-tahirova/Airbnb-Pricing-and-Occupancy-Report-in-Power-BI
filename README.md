Airbnb Seattle - Power BI Hesabat Sənədləşməsi
1. Məlumatların Təmizlənməsi və Power Query Qərarları
Qiymət Sütununun Təmizlənməsi: Həm listings, həm də calendar cədvəllərindəki price sütunlarından $ və `, işarələri silindi və məlumatlar onluq (Decimal) formatına çevrildi.

calendar.csv faylında available = "f" ifadəsi elanın mövcud olmadığını (yəni REZERV EDİLDİYİNİ - BOOKED) göstərir. Əksinə, available = "t" elanın boş olduğunu bildirir.
Doluluq dərəcəsini düzgün hesablamaq üçün xüsusi IsBooked sütunu yaradıldı: available = "f" olduqda 1, available = "t" olduqda isə 0 təyin edildi.

Sütunların Silinməsi: Məlumat həcmini optimallaşdırmaq və modelin məhsuldarlığını artırmaq üçün listings cədvəlindən ehtiyac olunmayan mətn təsviri sütunları silindi.

2. Məlumat Modeli və DAX Məntiqi
Ulduz Sxemi Strukturu (Star Schema):

Ölçü Cədvəli: listings (3818 sətr)

Tarix Ölçüsü: CALENDAR() vasitəsilə yaradılmış və rəsmi Tarix Cədvəli olaraq işarələnmiş Date cədvəli.

Xronoloji Sıralama: Qrafiklərin əlifba sırası ilə düzülməsinin qarşısını almaq üçün Month Name sütunu Month Number ilə sıralanacaq şəkildə tənzimləndi.

Median Qiymət Yanaşmasının İzahı (MEDIANX)
Fakt cədvəli üzərində sadə MEDIAN(calendar[price]) funksiyasının istifadəsi bütün gündəlik qeydlər üzrə medianı hesablayır və nəticəni daha çox rezervasiya tarixi olan elanların xeyrinə təhrif edir. 
Əsl elan səviyyəsində median qiyməti əks etdirmək üçün aşağıdakı üsuldan istifadə edildi:MEDIANX(VALUES(listings[id]), CALCULATE(AVERAGE(listings[price])))
Bu formula əvvəlcə hər bir elan üzrə orta qiyməti hesablayır, daha sonra isə fərqli elanlar arasındakı medianı qiymətləndirir.

3. İnvestor Nəticələri (Investor Insights)
Ən Bahalı və Ən Ucuz Məhəllələr: Southeast Magnolia, Portage Bay və Westlake $200+-dən yuxarı olmaqla ən yüksək median qiymətlərə sahibdir, Rainier Beach və Delridge isə $70–$85 arası qiymətlərlə ən büdcəyə uyğun ərazilərdir.

İnvestisiya Hədəfləri : Queen Anne və Ballard məhəllələri median qiymətdən yuxarı göstəriciləri və ardıcıl olaraq yüksək doluluq dərəcələrini birləşdirərək əmlak alışı üçün əsas hədəflər təşkil edir.

Mövsümi Pik Nöqtə: Doluluğu ən yüksək aylar Yanvar və İyul ayları, ən aşağı olanlar isə mart, noyabr və dekabr aylarıdır.

Otaq Növlərinin Effektivliyi: Bütöv mənzillər/evlər (Entire homes/apartments) Özəl otaqlar (Private, ~$70) və Paylaşılan otaqlarla (Shared, ~$40) müqayisədə ən yüksək gəlir potensialını (~$150 median qiymət) təmin edir.

Anında Rezervasiya (Instant Bookable) Təsiri: Anında rezervasiya funksiyasının aktiv olması median siyahı qiymətində endirim tələb etmədən doluluq faizini bir qədər yaxşılaşdırır.
