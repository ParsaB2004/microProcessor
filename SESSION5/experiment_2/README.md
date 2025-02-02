## آزمایش شماره دو : سنسور دنده عقب 💡

### هدف 🎯

در این آزمایش به دنبال آن هستیم که از طریق ماژول <strong>اولتراسونیک</strong> که در آزمایش قبلی با آن آشنا شدیم یک سنسور مشابه سنسور دنده عقب خودرو بسازیم.

---

### مواد و وسایل مورد نیاز 🛠️

<div align="right">
<table>
<thead>
<tr>
<th>ردیف</th><th>قطعه</th><th>تعداد</th>
</tr>
</thead>
<tbody align="center">
<tr>
<td>1</td><td>بردبورد</td><td>یک عدد</td>
</tr>
<tr>
<td>2</td><td>برد آردوینو UNO</td><td>یک عدد</td>
</tr>
<tr>
<td>3</td><td>سیم جامپر</td><td>هشت عدد</td>
</tr>
<tr>
<td>4</td><td>کابل USB</td><td>یک عدد</td>
</tr>
<tr>
<td>5</td><td>دیود نوری</td><td>یک عدد</td>
</tr>
<tr>
<td>6</td><td>سنسور اولتراسونیک</td><td>یک عدد</td>
</tr>
<tr>
<td>7</td><td>مقاومت 220 اهمی</td><td>یک عدد</td>
</tr>
</tbody>
</table>
</div>

---

### توضیحات کلی 📝

سنسور دنده عقب در خودروها نوعی هشدار دهنده می باشد به صورتی که در هنگام دنده عقب رفتن خودرو به طور اتوماتیک فعال می شود و اگر که فاصله خودرو تا یک مانع که در پشت آن قرار دارد از یک اندازه مشخصی کمتر شود ( حدودا یک متر ) سنسور شروع به هشدار دادن از طریق صدا می کند به صورتی که هر چقدر خودرو به مانع نزدیک تر شود شدت این آلارم بیشتر خواهد شد.  
ما نیز در مدار یک LED قرار می دهیم و می خواهیم در محدوده بین 4 تا 30 سانتی متری از اولتراسونیک هر مانعی قرار گرفت LED روشن شود البته به شکلی که هر اندازه جسم به سنسور نزدیک تر شد شدت روشنایی نور LED بیشتر شود و بلعکس.

---

### سورس کد 💻

```cpp
int trig = 10;
int echo = 9;
int duration = 0;
int distance = 0;
int led = 11;
int pwmLed = 0;

void setup() {
  pinMode(trig, OUTPUT);
  pinMode(echo, INPUT);
  pinMode(led, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  digitalWrite(trig, LOW);
  delayMicroseconds(2);
  digitalWrite(trig, HIGH);
  delayMicroseconds(10);
  digitalWrite(trig, LOW);

  duration = pulseIn(echo, HIGH);
  distance = (duration / 2) * 0.0343;

  analogWrite(led, 0);
  if (distance >= 4 && distance <= 30) {
    pwmLed = map(distance, 4, 30, 255, 0);
    analogWrite(led, pwmLed);
    Serial.println(distance);
    delay(100);
    }

    Serial.println(distance);
    delay(100);
}
```

---

### توصیف کد های برنامه 🧑🏻‍💻

در تابع setup، حالت پایه های trig و echo اولتراسونیک را تنظیم می کنیم همچنین یک پایه خروجی برای LED نیز تعیین می کنیم. سریال مانیتور را هم با نرخ تبادل داده 9600 بیت بر ثانیه راه اندازی می کنیم.  
در تابع loop، از طریق پایه خروجی trig ماژول اولتراسونیک امواج فراصوت به محیط می فرستد. سپس پایه echo که در واقع همان گیرنده می باشد این امواج را دریافت کرده و از طریق متد pulseIn فاصله بین زمان رفت و برگشت محاسبه شده و ما نیز خروجی این متد را در متغیر duration نگهداری می کنیم. سپس از طریق فرمول ریاضی و بر اساس زمان رفت و برگشت، فاصله از اشیا را محاسبه می کنیم و آن را در متغیر distance نگه می داریم.  
در دستور if شرط به صورتی تعیین می شود که اگر مقدار distance بین 4 تا 30 بود بدنه آن اجرا شود. در بدنه شرط به خاطر اینکه خروجی آنالوگ LED بین محدوده 0 تا 255 می باشد پس یکبار با متد map مقدار فاصله را به این محدوده تبدیل می کنیم. در نهایت خروجی حاصل را از طریق متد analogWrite به LED می دهیم. همچنین مقدار distance را بر روی serial نیز چاپ می کنیم تا در هر لحظه فاصله جسم تا سنسور را بر روی سریال مانیتور نیز نمایش دهد.

---

### شرح کارکرد مدار به صورت ویدیویی 🎥

<br>

<div align="center">
<img src="/media/microprocessor_13.gif">
</div>

---

### شماتیک مدار 🗺️

<br>

<div align="center">
<img src="/media/schematic_11.jpg" width="600px" height="300px">
</div>

---

### توضیحات شماتیک مدار ✒️

<ol>
<li>
اتصالات LED
<ul>
<li>پایه منفی به Gnd از طریق مقاومت 220 اهمی</li>
<li>پایه مثبت به پین 11 آردوینو</li>
</ul>
</li>
<li>
اتصالات اولتراسونیک
<ul>
<li>پایه 1 به Vcc</li>
<li>پایه 2 ( trig ) به پین 10 آردوینو</li>
<li>پایه 3 ( echo ) به پین 9 آردوینو</li>
<li>پایه 4 به Gnd</li>
</ul>
</li>
</ol>

---

### نتیجه گیری 👀

از جمله کاربرد های پر استفاده تکنولوژی اولتراسونیک ساخت سیستم های جلوگیری از برخورد با مانع می باشد البته باید بتوانیم تاخیر عملکرد سنسور را تا حد امکان کاهش دهیم که بتواند نسبت به لحظه ای ترین تغییرات واکنش نشان دهد و پایداری و اطمینان پذیری آن بالا باشد.
