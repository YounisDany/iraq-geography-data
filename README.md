# IRAQ Geography Data (بيانات جغرافية للعراق)

قاعدة بيانات شاملة للمحافظات، الأقضية، والنواحي/القرى في العراق، متوفرة بصيغة JSON وملف JavaScript جاهز للاستخدام عبر CDN.

## ماذا تقدم هذه المكتبة؟

*   **ملف بيانات (JSON) منظّم:** يحتوي على هيكل واضح للمحافظات، الأقضية، والنواحي/القرى.
*   **ملف JS جاهز للاستخدام عبر CDN:** يمكن تضمينه بسهولة في أي مشروع ويب.
*   **سهولة الدمج:** مصمم ليكون سهل الاستخدام للمطورين والباحثين.

## هيكل البيانات (الـ Schema)

تتبع البيانات الهيكل التالي:

```json
[
  {
    "id": "01",
    "name": "اسم المحافظة",
    "districts": [
      {
        "id": "01-01",
        "name": "اسم القضاء",
        "villages": [
          {"id":"01-01-001","name":"اسم الناحية/القرية"}
        ]
      }
    ]
  }
]
```

*   `id`: معرف فريد (string) لكل محافظة، قضاء، أو ناحية/قرية لتسهيل البحث والربط.
*   `name`: الاسم العربي للمحافظة، القضاء، أو الناحية/القرية.
*   `districts`: مصفوفة تحتوي على الأقضية التابعة للمحافظة.
*   `villages`: مصفوفة تحتوي على النواحي/القرى التابعة للقضاء.

## استخدام سريع

يمكنك تضمين ملف JavaScript المصغر مباشرة في مشروعك عبر CDN (سيتم توفير الرابط لاحقًا عند النشر).

```html
<!-- مثال لتضمين الملف بعد نشره على CDN -->
<script src="https://cdn.jsdelivr.net/gh/younisdany/iraq-geography-data/data/iraq_data.min.js"></script>
<script>
  // بعد التحميل، ستكون البيانات متاحة عبر المتغير العام IRAQ_GEOGRAPHY
  console.log("عدد المحافظات:", IRAQ_GEOGRAPHY.length);
  IRAQ_GEOGRAPHY.forEach(province => console.log(province.name));
</script>
```

أو إذا كنت تفضل استخدام ملف JSON مباشرة:

```javascript
fetch("https://cdn.jsdelivr.net/gh/younisdany/iraq-geography-data/data/iraq_data.json")
  .then(response => response.json())
  .then(data => {
    console.log(data);
  });
```

## أمثلة دوال بحث/فلترة جاهزة (JavaScript)

بافتراض أن `IRAQ_GEOGRAPHY` محمّلة:

```javascript
// البحث عن محافظة بالاسم
function findProvinceByName(name) {
  return IRAQ_GEOGRAPHY.find(p => p.name === name) || null;
}

// الحصول على الأقضية التابعة لمحافظة معينة
function getDistrictsByProvinceId(provinceId) {
  const province = IRAQ_GEOGRAPHY.find(p => p.id === provinceId);
  return province ? province.districts : [];
}

// البحث عن النواحي/القرى بكلمة مفتاحية
function searchVillages(keyword) {
  const results = [];
  IRAQ_GEOGRAPHY.forEach(province => {
    province.districts.forEach(district => {
      district.villages.forEach(village => {
        if (village.name.includes(keyword)) {
          results.push({
            province: province.name,
            district: district.name,
            village: village.name
          });
        }
      });
    });
  });
  return results;
}
```

## المساهمات

نرحب بالمساهمات لتحسين هذه المكتبة وإضافة المزيد من البيانات أو تصحيحها. يرجى فتح `Issue` أو إرسال `Pull Request` على مستودع GitHub.

## الترخيص

هذه المكتبة مرخصة تحت ترخيص MIT.

