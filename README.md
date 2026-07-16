# Checkpoint 2

LLM API (NVIDIA — DeepSeek V4 Flash) istifadə edərək universitet/ixtisas seçimi üzrə abituriyentlərə kömək edən dəstək botu. Strukturlaşdırılmış system prompt və few-shot nümunələr vasitəsilə botun cavab dairəsi və tərzi idarə olunur.

## Qurulum

1. Virtual mühit yaradın:
   ```bash
   python -m venv venv
   source venv/bin/activate   # Windows: venv\Scripts\activate
   ```

2. Asılılıqları quraşdırın:
   ```bash
   pip install -r requirements.txt
   ```

3. `.env.example` faylını `.env` adı ilə kopyalayın:
   ```bash
   cp .env.example .env
   ```
   Sonra `.env` faylını açıb `NVIDIA_API_KEY` dəyərini öz API key-inizlə əvəz edin. Key-i [build.nvidia.com](https://build.nvidia.com) hesabınızdan əldə edə bilərsiniz.

   ⚠️ `.env` faylı `.gitignore` ilə istisna edilib — real key heç vaxt repository-yə düşmür. Yalnız `.env.example` (placeholder dəyərlə) repo-dadır.

4. Tətbiqi işə salın:
   ```bash
   python main.py
   ```

## Fayl strukturu

```
├── main.py           — əsas skript: system prompt, few-shot nümunələr, API çağırışı
├── .env.example       — API key konfiqurasiyası üçün nümunə (real key yoxdur)
├── .gitignore          — .env faylının commit olunmasının qarşısını alır
└── requirements.txt     — Python asılılıqları (openai, python-dotenv)
```

## Prompt Engineering yanaşması

**System prompt** üç aydın bölmədən ibarətdir:
- **ROL** — botun kimliyi və məqsədi
- **ƏHATƏ DAİRƏSİ / KƏNAR** — bot yalnız ixtisas və universitet sualları ilə məşğul olur, fənn üzrə suallardan (riyaziyyat, fizika və s.) imtina edir
- **QAYDALAR** — uydurma məlumat verməmək, cavab formatı, ton

**Few-shot nümunələr** (3 ədəd), hər sorğudan əvvəl system promptun ardınca modelə göstərilir:
1. Konkret ixtisas sualı → strukturlaşdırılmış, bullet-point formatında cavab
2. Konkret universitet sualı → rəsmi mənbəyə yönləndirmə ilə birlikdə cavab
3. Sahədən kənar (fənn) sualı → nəzakətli imtina nümunəsi

## Nümunə sorğu/cavab log-ları

**Sorğu:** `Kompüter mühəndisliyi ixtisası necədir?`
**Cavab:**
```
Kompüter mühəndisliyi həm proqram təminatı, həm də aparat (hardware)
tərəfini əhatə edən ixtisasdır.

- Nə öyrədilir: proqramlaşdırma, rəqəmsal sistemlər, şəbəkələr,
  gömülü sistemlər (embedded systems)
- İş perspektivi: yüksək tələbat var, həm yerli, həm beynəlxalq bazarda
  iş imkanları genişdir
- Kimə uyğundur: riyaziyyat və məntiqi düşünməyi sevən, texnologiyaya
  maraqlı abituriyentlərə

Hansı universitetdə oxumaq istədiyinizi deyə bilərsinizsə, o universitetə
görə daha konkret məlumat verə bilərəm.
```

**Sorğu:** `AzTU necə universitetdir?`
**Cavab:**
```
Azərbaycan Texniki Universiteti (AzTU) ölkənin ən köklü və tanınmış texniki
ali məktəblərindəndir.
- Güclü tərəfləri: Mühəndislik ixtisasları (kompüter, elektrik, mexanika,
  inşaat və s.) üzrə tədris keyfiyyəti yüksəkdir.
- Kampus və imkanlar: Laboratoriyalar, texniki avadanlıqlar və təcrübə bazası
  mövcuddur.
- Qəbul balları: İxtisasdan asılı olaraq dəyişir. Məsələn, kompüter
  mühəndisliyi kimi populyar ixtisaslarda rəqabət daha yüksəkdir.

Hər hansı konkret ixtisas üzrə bal və ya tədris planı ilə maraqlanırsınızsa,
deyin — daha ətraflı izah edim. Rəsmi bal məlumatı üçün tələ.edu.az saytını
yoxlamağı unutmayın.
```

**Sorğu:** `Kvadrat tənliyi necə həll edim: x^2+5x+6=0?`
**Cavab:**
```
Bu sualı cavablandıra bilmərəm — mən yalnız universitet və ixtisas
seçimi mövzusunda kömək edən botam. Riyazi məsələlər üçün müəlliminizə
və ya fənn üzrə resurslara müraciət etməyinizi tövsiyə edirəm.

Ixtisas və ya universitet seçimi ilə bağlı sualınız varsa, məmnuniyyətlə
kömək edərəm.
```

Üçüncü nümunə botun əhatə dairəsi məhdudiyyətinin praktikada necə işlədiyini göstərir — few-shot nümunəsindəki davranış real sorğuda da qorunur.
