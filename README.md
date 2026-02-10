## **Before Getting Started, Be aware that The Repo is being updated with some notes over time**

## ازيك ياصديقي عامل اي جاهز نبدأ سوا في ال Topic البسيط اللذيذ دا ؟ يلا بينا (●'◡'●)

#### اولا يعني ايه Unit Testing ?

ببساطة كده:

> هو إنك تختبر “وحدة صغيرة جدًا” من الكود — زي **دالة أو ميثود** — لوحدها،  
> عشان تتأكد إنها شغالة صح بغض النظر عن باقي البرنامج , يعني نمسك كل جزء او **ميثود** لوحدها ونعملها Test علي انفراد

يعني مش بتختبر السيستم كله انت حرفيا بتختبر جزء صغير منه بشكل معزول (isolated) تماما 


---
## طب اي انواع ال Unit Testing


احنا عندنا 3 مدارس في الـ Unit Testing في .NET: 

> MSTest — ده الجد الكبير بتاع مايكروسوفت من أيام زمان سهل ومباشر.  
> NUnit — ده اللي جِه بعده وكان الأشهر لفترة طويلة لأنه مرن أكتر.  
> xUnit — ده الجديد والأخف معمول عشان يحل عيوب اللي قبله وده اللي مايكروسوفت نفسها بترشحه حاليًا


### إيه هو XUnit؟

- ال **XUnit** هو واحد من أشهر **Unit Testing Frameworks** في C#
    
- البديل القديم كان **NUnit** أو **MSTest**،
- ولكن XUnit بيديك شوية مميزات حديثة زي دعم **dependency injection** بشكل أفضل.


---

###  التركيبة الأساسية لأي Test في XUnit (AAA Pattern)

كل اختبار بيتكون من 3 خطوات أساسية:

1. **Arrange** → جهز البيانات والبيئة للاختبار
    
2. **Act** → نفذ الحاجة اللي عايز تختبرها
    
3. **Assert** → اتأكد إن النتيجة اللي طلعت صح
    
تعالي نشوف سوا يعني ايه ...

``` csharp
using Xunit;

public class CalculatorTests
{
    [Fact] // دي علامة إن دي Test واحدة
    public void Add_ShouldReturnCorrectSum()
    {
        // Arrange
        var calc = new Calculator();

        // Act
        var result = calc.Add(2, 3);

        // Assert
        Assert.Equal(5, result); // اتأكد إن الناتج = 5
    }
}

```

- `[Fact]` → لما التيست  ثابت ومش محتاج بيانات خارجية.
    
- `[Theory]` → *(هكلمك عليها قدام متقلقش)* لما عايز تختبر Function بأكتر من مجموعة بيانات.  
    بس بص علي المثال بتاعها بردو 

```csharp
[Theory]
[InlineData(2, 3, 5)]
[InlineData(5, 5, 10)]
[InlineData(0, 7, 7)]
public void Add_ShouldWorkForMultipleInputs(int a, int b, int expected)
{
    var calc = new Calculator();
    var result = calc.Add(a, b);
    Assert.Equal(expected, result);
}

```

---

###  أهم الـ Assert اللي هتستخدمها

- `Assert.Equal(expected, actual)` → اتأكد إن الناتج = المتوقع 
    
- `Assert.NotEqual(expected, actual)` → اتأكد إنه مش متساوي
    
- `Assert.True(condition)` → اتأكد إن الشرط True
    
- `Assert.False(condition)` → اتأكد إن الشرط False
    
- `Assert.Throws<ExceptionType>(() => ...)` → اتأكد إن Function رمت Exception معين
    

### تعالي نشوف امثلة اكتر عليهم 

```csharp
 [Fact]
public void Add_TwoNumbers_ReturnsCorrectSum()

 {
     //Arrange
     int x = 2;
     int y = 3;
     //Act

     int z = x + y;

     //Assert
     Assert.Equal(5, z);
 }
 [Fact]
public void Func_WhenInputIsNull_ThrowsArgumentNullException()

 {

     //Arrange

     int? x = null;
     
     //Act
     Action<int?> func = (x) =>
     {
         if (x == null)
         {
             throw new ArgumentNullException(nameof(x));
         }
     };
     //Assert
     Assert.Throws<ArgumentNullException>(()=>func(x));
 }
 [Fact]
public void String_WhenEndsWithSpecificWord_ReturnsTrue()

 {
    //Arrange
     string WelcomeMessage;
     //Act
     WelcomeMessage = "Welcome";
     //Assert
     Assert.EndsWith("come", WelcomeMessage);
 }
[Fact]
public void String_WhenLengthIsFive_ReturnsTrue()
 {
     string name = "ahmed";
     Assert.True(name.Length == 5);
 }
[Fact]
public void String_WhenEmptyOrWhitespace_ShouldFailNotEmptyAssertion()

 {
     string name = " ";
     Assert.NotEmpty(name);
 }
```

### تعالي ناخد اول مثال ونتكلم عليه شوية كدا 
```csharp
 [Fact]
public void Add_TwoNumbers_ReturnsCorrectSum()

 {
     //Arrange
     int x = 2;
     int y = 3;
     //Act

     int z = x + y;

     //Assert
     Assert.Equal(5, z);
 }
```

### 1-  `[Fact]`

- دي **Attribute** من **xUnit** معناها إن **الميثود دي اختبار (Test Method)** وبتتنفذ مرة واحدة بس يعني مش كذا مرة بكذا value (لو مش فاهم يعني اي هتعرف قدام)
    
- من الاخر كدا بتتكتب لما ال Test **مش بياخد أي Parameters** (عشان هنشوف قدام `[Theory]`)

### 2- **Arrange**

- المرحلة الأولى من أي test.
    
- بتجهز فيها كل اللي محتاجه للاختبار.
    
- متغيرات، objects، mocks، inputs...
        

 هنا:

```c
int x = 2; 
int y = 3;
```


يعني بتحضر البيانات اللي هتستخدمها.

---

### 3- **Act**

- المرحلة اللي فيها بتنفذ **العملية اللي عايز تختبرها**.
    
- يعني تشغل الكود اللي المفروض يطلع النتيجة.
    

 هنا:

```c
int z = x + y;
```

العملية اللي بنختبرها هي الجمع.

---

### 4- **Assert**

- المرحلة اللي بتتأكد فيها إن **النتيجة اللي طلعت فعلاً صح**.
    
- لو النتيجة غلط → الTest هيفشل.
    هنا:

```c
Assert.Equal(5, z);
```

بنقارن القيمة اللي طلعها الكود (`z`) بالقيمة المتوقعة (`5`).

 لو الاتنين بيساووا بعض → ال Test ناجح.  
 لو مختلفين → ال Test بيفشل.

---
## تعالي نبص علي دا كمان

```csharp
 [Fact]
public void Func_WhenInputIsNull_ThrowsArgumentNullException()

 {

     //Arrange

     int? x = null;
     
     //Act
     Action<int?> func = (x) =>
     {
         if (x == null)
         {
             throw new ArgumentNullException(nameof(x));
         }
     };
     //Assert
     Assert.Throws<ArgumentNullException>(()=>func(x));
 }
```

### Arrange 
- هنا احنا مجهزين البيانات اللي هنختبر بيها الفنكشن.
    
- ال `x` هنا **nullable int** وقيمته **null** عشان نختبر حالة الفنكشن لما يدخل null 

### Act
- بنعرف **فنكشن مؤقتة (delegate)** اسمها `func` من نوع `Action<int?>`.
 
- لو `x == null` → ترمي **ArgumentNullException** مع اسم المتغير
        
-  لو `x` مش null → ما ترجعش حاجة   

### Assert
- هنقول بقا "لما نشغل `func(x)`، لازم يطلع **ArgumentNullException**."
    
- بما إن `x = null`، الفنكشن فعلاً هيرمي الاستثناء → الاختبار هيعدي
    
- لو الفنكشن ما رمتش Exception → الاختبار هيكسر (fail)

---
#### ببساطة كدا يعني 

| المرحلة | المعنى | بتعمل إيه فيها          |
| ------- | ------ | ----------------------- |
| Arrange | جهّز   | جهّز البيانات والحالة   |
| Act     | نفّذ   | شغّل الكود اللي بتختبره |
| Assert  | تحقق   | قارن النتيجة بالمتوقّع  |

---
### طب واي موضوع التسمية دا ؟ قالك والله في format كويس ممكن نمشي عليه اللي هو :

### الصيغة الأفضل بتكون بالشكل ده:

> **MethodName_Scenario_ExpectedResult**

---

## اقرا الامثلة اللي فوق دي كويس وجربها عندك كدا وروق الكلام 
---

###  مميزات XUnit

- **يشتغل زي الفل مع .NET Core و .NET 5/6/7**  
    يعني هتقدر تشتغل عليه من غير أي مشاكل مع الإصدارات الجديدة من .NET.
    
-  **بيشغل الاختبارات في نفس الوقت (Parallel Testing)** وده بيخلي الـ tests تخلص أسرع بدل ما تمشي واحدة واحدة بيمشي كله في نفس الوقت لكن مش في كل الاوقات ياصديقي بنستخدم ال Parallel Testing بس مننكرش ان دي ميزة قوية مفيدة في اغلب الحالات
    
-  بيدعم الـ Dependency Injection جوه ال **Tests**  
    الـ DI في tests بيخليك **تحكم في كل dependency بسهولة** لو انت عايز تستخدم mock أو نسخة حقيقية وده بيخلي الاختبارات **مرنة وسهلة الكتابة**
    
-  **الـ Syntax بتاعه بسيط وواضح جدا**  
    الكود سهل تتكتبه وسهل تتقراه، ومش معقد خالص *(زي مانت شوفت فوق كدا)*   


------
## We Can Install `FluentAssertion` Package to make testing is more flexible 

- `<PackageReference Include="FluentAssertions" Version="8.8.0" />`

- ## طب اي هي ال Fluent Assertions ؟ 
> هي **Library** في .NET بتخليك تكتب الـ assertions بطريقة **مقروءة** اكتر وبشكل سلس وفوق كل دا فيها كذا ميثود بتسهل عليك عملية التيستنج بحيث تخليك تقارن كذا object (ممكن يكون معقد) بشكل سهل 



```csharp
using FluentAssertions;
using Xunit;

public class ProjectTests
{
    [Fact]
    public void Add_TwoNumbers_ReturnsCorrectSum()
    {
        int x = 5;
        int y = 4;

        int z = x + y;

        z.Should().Be(9);
    }

    [Fact]
    public void Sum_WhenComparedToCloseValue_ShouldBeWithinTolerance()
    {
        int x = 5;
        int y = 4;

        int z = x + y;

        z.Should().BeCloseTo(7, 2, "Sum can vary within ±2 values");
    }

    [Fact]
    public void String_WhenStartsWithALetter_ShouldPass()
    {
        string name = "Ahmed";
        name.Should().StartWith("A");
    }

    [Fact]
    public void String_WhenEndsWithSpecificWord_ShouldPass()
    {
        string name = "Welcome";
        name.Should().EndWith("come");
    }

    [Fact]
    public void String_WhenLengthIsFive_ShouldHaveCorrectLength()
    {
        string name = "ahmed";
        name.Should().HaveLength(5);
    }

    [Fact]
    public void String_WhenHasLengthFiveAndEqualsAhmed_ShouldMatchBothConditions()
    {
        string name = "ahmed";
        name.Should().HaveLength(5).And.Be("ahmed");
    }

    [Fact]
    public void String_WhenNotNullOrWhitespace_ShouldBeValid()
    {
        string name = "Mo Salah";
        name.Should().NotBeNullOrWhiteSpace();
    }

    [Fact]
    public void String_ShouldBeOfTypeString()
    {
        var name = "Ali";
        name.Should().BeOfType<string>();
    }

    [Fact]
    public void Number_WhenOdd_ShouldBeTrueForOddCheck()
    {
        int num = 5;
        bool checking = (num & 1) == 1;
        checking.Should().BeTrue();
    }

    [Fact]
    public void Number_WhenPositive_ShouldBeRecognizedAsPositive()
    {
        int num = 5;
        num.Should().BePositive();
    }
}

```

- هنا انت لقيت الدنيا اسهل شوية واوضح ومش ملعبكة مش كدا ؟ هي دي بقا من مزايا ال **FluentAssertion**
---

## بعد ما فهمنا الفكرة الاساسية منه طب انا ليه بستخدمه مادام انا عارف النتيجة بتاعة كل Test وانا اللي بجبره عليها ؟
>
انت ممكن تعرف النتيجة دلوقتي بس بعد شهر أو بعد ما تعديل زميلك للكود:
> 
> - مين هيعرف لو حصل Error؟
>     
> - الـ Unit Test موجود عشان يقولك: "أيوه فيه حاجة اتغيرت الحل مش صح"
>     

---

#  كمان أسباب استخدام Unit Test حتى لو تعرف النتيجة

1️⃣ **اننا نتأكد من سلوك الكود**: حتى لو النتيجة واضحة دلوقتي  
2️⃣ **الحماية من regressions**: أي تغيير بعد كدا ممكن يكسر الكود  
3️⃣ **توثيق للكود**: الـ tests بتوصف كيفية عمل الكود  
4️⃣ **تسهيل الصيانة**: لما تكبر المشاريع صعب تحكم بالكود كله بدون Tests

---

### يعني نقدر نلخص ونقول :

- **معرفتك بالنتيجة دلوقتي = مش كفاية**
    
- شبكة امان للكود علي المدي البعيد = Unit Test 
    
- كل ما المشروع يكبر أو الفريق يزيد → أهمية الاختبارات تزيد


---
## مقتنعتش مش كدا ؟.. تعالي نشوف مثال 

```csharp
public class Calculator
{
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Multiply(int a, int b)
    {
        return a * b;
    }
}
```

- دول 2 methods واحدة بتجمع وواحدة بتضرب تعالي نعمل عليهم test كدا 
```csharp
using Xunit;

public class CalculatorTests
{
    private readonly Calculator _calculator = new Calculator();

    [Fact]
    public void Add_ReturnsCorrectSum()
    {
        // Arrange
        int a = 2, b = 3;

        // Act
        var result = _calculator.Add(a, b);

        // Assert
        Assert.Equal(5, result); // تأكيد النتيجة
    }

    [Fact]
    public void Multiply_ReturnsCorrectProduct()
    {
        // Arrange
        int a = 2, b = 3;

        // Act
        var result = _calculator.Multiply(a, b);

        // Assert
        Assert.Equal(6, result);
    }
}

```

- ### دلوقتي انت عارف النتيجة والدنيا ماشية زي الفل وعارف انه بيجمع تخيل بقا لو جه زميلك في الشغل حب يوقعك ويخليك عبرة قدام مديرك وراح داخل علي كودك بعد ما خلصته وعمل كدا 

```csharp
public int Add(int a, int b)
{
    return a - b; 
}
```
## "بص الغل عاوز اي يعمنا عاوز اي ياكريستيانو" دلوقتي ال method دي مفروض بتجمع مش بتطرح وانت مخدتش بالك فانت لما جيت تجرب ال test مطلعش صح وطلع كدا

```lua
Add_ReturnsCorrectSum FAILED
Expected: 5
Actual: -1
```

## هنا نقدر نفهم ازاي ال unitTesting بيحافظ عليها للمدي الطويل يعني لو ال method باظت نقدر نرجع ليها ونعدلها وبردو لو حبينا نعرف هي شغاله بنفس ال functionality ولا لأ

---
## تعالي بقا ياصديقي نبص علي ال Mock واي حكايته
### اي هي الفكرة الأساسية بتاعته؟

الـ **Mock** هو أداة بتستخدمها في الاختبارات (زي xUnit) لما ال **service** اللي بتختبرها او الكود عموما بيعتمد على كائنات تانية فيها logic (logic ياعزيزي زي Add , delete ,... مش مجرد Calss فاضي )  (dependencies)

 

بدل ما تستخدم ال object الحقيقية، بتعمل منها نسخة "وهمية" تتحكم في رد فعلها، عشان تقدر تختبر الكود بتاعك لوحده، من غير تأثيرات خارجية يعني من غير ماتكلم داتا بيز او اي External Service وتقوله انا لو ناديت الMethod دي رجع القيمة اللي هقولك عليها (اعمل Mock للـ Behavior ,متعملش Mock للـ Data) (هنفهم المعني اكتر مع امثلة قدام)


---

## بص كدا علي كام Case دول واقرأهم كويس

### 1. لما تكون بتختبر Service بتتعامل مع Database

افترض عندك كلاس اسمه `ProductService` وجواه دالة بتجيب المنتجات من قاعدة البيانات عن طريق `IProductRepository`.

في الحالة دي، مش منطقي وقت الTest إنك فعلا تفتح قاعدة بيانات وتشغل Query.  
السبب:

- ممكن الداتا تتغير، فتبوظ نتيجة ال Test.
    
- ممكن السيرفر يقع.
    
- ال test هيبقى بطيء.
    

الحل → تعمل **Mock** للـ Repository.  
يعني تقول له مثلاً:

> لما حد ينادي `GetAll()`، رجّعلي قائمة منتجات ثابتة أنا محددها (مش هينادي بجد انا اللي هحدد ال response وهو يرجعه لل Test)

كده إنت بتختبر منطق `ProductService` نفسه — هل بيتعامل صح مع النتائج؟ هل بيحوّلها؟ هل بيعمل Validation؟  
من غير ما تدخل الDataBase في الصورة 

---

### 2. لما تكون بتختبر كود بيبعت إيميلات أو رسائل

افترض عندك `NotificationService` فيه دالة `SendEmail()`.  
لو سبتها تبعت إيميلات حقيقية، هتغرق الإيميل بتاعك، وممكن كمان تعمل Spam ونخش في دوامة بقا احنا في غنى عنها

فبدل كده، بتعمل **Mock** للـ EmailSender تخليه يسجل إن الدالة `Send()` اتنادت من غير ما تبعت حاجة فعلا بعد كده تتأكد في ال Test:

> هل فعلاً الكود استدعى SendEmail لما حصل الحدث المطلوب؟

كده تتأكد إن السلوك صح من غير ما يحصل تأثير حقيقي.

---

### 3. لما تكون بتختبر تعامل الكود مع الأخطاء

افترض إن عندك كود بيتصل بـ API خارجي (زي الدفع الإلكتروني) ممكن في الحالة العادية كل حاجة تمشي كويس لكن عايز تختبر:

- إيه اللي يحصل لو الـ API رجع Error؟
    
- أو لو الـ API تأخر في الرد؟
    

لو تستخدم الـ API الحقيقي، صعب تعمل السيناريو ده.  
لكن بالـ Mock، تقدر ببساطة تقول:

> لما أنده على `SendPayment()`, ارمي Exception.

وبعدين تشوف: هل الكود بيتعامل صح؟ بيطلع Error مناسب؟ بيعمل Retry؟  
ده اختبار فعلي بس محكوم.

---

### 4. لما تكون بتختبر Controller أو Endpoint

افترض إن عندك `AccountController` في ASP.NET Core بيتعامل مع `IAuthService`  بس هو في ال Test مش محتاج AuthService الحقيقي (اللي ممكن يكون بيتكلم مع قاعدة بيانات أو Redis أو Token Generator).  
إنت عايز تختبر الـ Controller نفسه: هل بيرجع Response صح؟ Status Code مناسب؟ بيتحقق من البيانات صح؟

فهنا بتعمل **Mock** للـ AuthService لانه هو في في السيناريو دا بيكون dependency وتقول له:

> لما يتنادي بـ Login("user", "pass")، رجّعلي نتيجة نجاح جاهزة.  
> أو  
> لما يتنادي بكلمة سر غلط، رجّعلي Failure.

وبعد كده تشوف رد فعل الـ Controller.  
هل رجع 200؟ هل رجّع 401؟  
كده بتختبر سلوك الـ Controller مش الـ Service.

---

### 5. لما تكون عايز تتأكد إن Method معينة اتنادت فعلا

أحيانا الكود اللي بتختبره مش بيرجع نتيجة مباشرة لكن بيعمل Action داخلي — زي إنه يسجل Log أو يبعت إشعار.  
في الحالة دي، بتستخدم الـ Mock عشان "تتجسس" على الاستدعاءات اللي حصلت.  
يعني بعد ما تشغل الاختبار، تقول للـ Mock:

> وريني اتنادى ولا لأ؟  
> كام مرة؟  
> وبأي Parameters؟

وده بيساعدك تعرف إن الكود نفذ الخطوات اللي المفروض يعملها.

---

### 6. لما تكون بتختبر كود بيمر بمراحل متعددة

افترض عندك `OrderService` بيعمل الخطوات التالية:

1. يتحقق من العميل.
    
2. يتأكد إن المنتج متاح.
    
3. يسحب المبلغ.
    
4. يسجل الطلب في قاعدة البيانات.
    
5. يبعث إيميل تأكيد.
    

كل خطوة من دول ممكن تبقى خدمة مختلفة (Dependency مختلفة).  
عشان تختبر السلوك النهائي أو أي حالة خاصة (زي فشل الدفع أو المنتج غير متاح)، بتستخدم Mocks لكل خدمة من دي، وتقولها ترجع ردود مختلفة حسب السيناريو اللي عايز تختبره.  
كده تقدر تختبر كل الحالات اللي ممكن تحصل من غير ما تدخل النظام الحقيقي كله في اللعبة.


### تعالي كدا نشوف مثال عادي من غير Mock 
## V1 For Testing 

```csharp
 public interface ICarServices
 {
     bool AddCar(Car car);
     bool RemoveCar(int Id);
     List<Car> GetAll();

 }
```

```csharp
 public class Car
{
    public int Id { get; set; }
    public string Name { get; set; } = string.Empty;
}

```

```csharp
public class CarService : ICarServices
{
    private readonly List<Car> _cars;
    public CarService(List<Car> cars)
    {
        _cars=cars;
    }

    public bool AddCar(Car car)
    {
        if (car == null)
        {
            return false;
        }
        if (_cars.Any(i => i.Id == car.Id))
            return false;
        _cars.Add(car);
        return true;
    }

    public List<Car> GetAll()
    {
        return _cars;
    }

    public bool RemoveCar(int Id)
    {
       
       if (id == 0) return false;

       var car = _cars.FirstOrDefault(i => i.Id == id);
       if (car == null) return false;

       _cars.Remove(car);
       return true;
    }
}
```

```csharp
 public class MockServices
 {
    [Fact]
    public void AddCar_Should_Add_Car_When_Valid()
    {
    // Arrange
    var cars = new List<Car>();
    var service = new CarService(cars);
    var car = new Car { Id = 2, Name = "BMW" };

    // Act
    var isAdded = service.AddCar(car);

    // Assert
    isAdded.Should().BeTrue();
    service.GetAll().Should().HaveCount(1);
    }

 }
```

## تعالي هنا ياصديقي بقا نفصص الموضوع دا واحدة واحدة : 
##### 1- لازم نعرف احنا بنعمل Test علي مين بالظبط ؟ لقينا ان احنا بنعمل Test علي ال `CarService` فاحنا لما نيجي نعمل منه Constructor هنلاقيه محتاج `<List<Car` 
##### 2- بكدا `<List<Car` بقت مطلوبة جوا ال `CarService` فهنعمل ليها Object جديد ليها جوا ك New
##### 3- لما باجي انادي الميثود اللي اسمها `AddCar` هحتاج اباصي ليها Car عادي لانه هو الداتا اللي بعوز اختبر عليها 

##### لو قولتلي ليه معملنهاش **mock** هزعل منك وهقولك اطلع اقرأ فوق تاني بعناية لانك مركزتش وبعد ما تقرأ بص علي الجملتين دول (عشان انت حبيبي بس) :

- ##### **الـ dependency** = حاجة الـ class بيستخدمها علشان يشتغل (زي خدمة أو repository أو API)
    
- ######  **الـ model (زي Car)** او <List<Car = مجرد data object (كائن بيانات) مش بيعمل أي logic، فملوش لازمة يتعمله Mock بالعكس انا ممكن اكون محتاج الداتا بتاعته عشان اجرب عليها
---
## V2 For Testing

- ### تعالي بقا تشوف لو هنستخدم mock
>اولا دول ال Service بتاعتنا 

```csharp
public interface ICarRepository
{
    bool Exists(int id);
    void Add(Car car);
    void Remove(Car car);
    Car? GetById(int id);
    List<Car> GetAll();
}
```

### ال Service يعتمد على Interface

```csharp
public class CarService : ICarService
{
    private readonly ICarRepository _repo;

    public CarService(ICarRepository repo)
    {
        _repo = repo;
    }

    public bool AddCar(Car car)
    {
        if (car is null) return false;
        if (_repo.Exists(car.Id)) return false;

        _repo.Add(car);
        return true;
    }

    public bool RemoveCar(int id)
    {
        if (id <= 0) return false;

        var car = _repo.GetById(id);
        if (car is null) return false;

        _repo.Remove(car);
        return true;
    }

    public List<Car> GetAll()
    {
        return _repo.GetAll();
    }
}
```

### هنا Unit Test بالـ Moq
```csharp
public class CarServiceTests
{
    [Fact]
    public void AddCar_Should_Call_Add_When_Not_Exists()
    {
        // Arrange
        var repoMock = new Mock<ICarRepository>();
        repoMock.Setup(r => r.Exists(It.IsAny<int>()))
                .Returns(false);

        var service = new CarService(repoMock.Object);
        var car = new Car { Id = 1, Name = "BMW" };

        // Act
        var result = service.AddCar(car);

        // Assert
        result.Should().BeTrue();
    }
}

```

#### تعالي نفهم سوا ونقول مشينا ازاي جاهز؟ يلا بينا 

### 1- احنا هنا بنعمل Test علي `CarService` فاحنا هنتعامل عادي ونشوف هو محتاج ايه وهنادي علي ايه جواه 
### 2- نيجي نشوف هنلاقي اننا في اول حاجة ان ال Constructor بيعتمد علي `ICarRepository`  ودي عبارة Service خارجية وجواها كلام كتير 
### 3- هنا بقا تيجي حاجة ال Mock اننا نعمل منها fake object نكمل بيه ودا اللي حصل هنا 
```csharp
 var repoMock = new Mock<ICarRepository>();
```

#### 4- بعد كدا بقا نشوف هنلاقي اننا بنادي جواه علي ال method اللي اسمها `Exists` اللي هي تبع `ICarRepository` فاحنا محتاجين نقوله لو نادينا ال method دي رجع لينا قيمة معينة احنا عاوزينها ودا بالظبط اللي حصل هنا ودي احد الاسباب اننا نعمل mock , فاحنا هنستخدم Method اسمها Setup طب بتعمل اي ؟

- : الـ `Setup()` بتستخدم **علشان تحدد سلوك (behavior)** الـ mock object لما يتم استدعاء method معينة طب يعني اي بردو ؟ يعني اكنك انت لما بتقول للـ mock: "لما حد ينادي الميثود دي رد بكذا"

```csharp
repoMock.Setup(r => r.Exists(It.IsAny<int>()))
                .Returns(false);
```
- هنا انا بقوله لو ناديت `Exists` بأي `int value` رجع `false` (هنا انا خليته يرجع false اجباري لما ينادي عليها ومش ينادي علي الاصلية) هتقولي ليه انت عاملها اي `int value` هقولك ياعزيزي انا عاوز اختبر ان ال Add تمام + مش هي اساس ال test اصلا ..مش هقعد اشوف ال list فيها كام obj والف وادور  (اشتري دماغك ياعزيزي وروق علي حالك كدا)

### 5- بعد كدا بقا نكريت ال service بتاعتنا عادي ونبعتلها قيمة ال object بتاع `ICarRepository` المطلوب (اللي هو اساسا mock) 

### 6- ولما هو يجي ينادي ال `AddCar` ونبعت معاها بقا قيمه ال Car اللي احننا حددناها (واللي بردو مش عملنا ليها mock ..واخد بالك ؟ (*^_^*)) ويلاقي جواها `Exists` يرجع `false` عادي جدا وبالتالي في نهاية ال Method يقوم يرجع true وبالتالي ال Test بتاعنا تمام جدا


----
#### تعالي ناخد كمان مثال ونفصصه كدا براحتنا وعاوزك تجاوب معايا (***هاااا جاوب معايا***)

## بص علي ال Service دي كدا 

```csharp

 private readonly IValidator<ResetPassword> _resetPasswordValidator;

 public AccountController(IValidator<ResetPassword>resetPasswordValidator)
 {
     _resetPasswordValidator = resetPasswordValidator;
 }

 [HttpPost("reset-password")]
 public async Task<ActionResult<Response<ResetPasswordResponse>>>ResetPasswordAsync(ResetPasswordRequest request)
 {
     var validation = await _resetPasswordValidator.ValidateAsync(request);
     if (!validation.IsValid)
     {
         var Errors = string.Join(',',validation.Errors.Select(e => e.ErrorMessage).ToList());
         return StatusCode((int)HttpStatusCode.BadRequest, _responseHandler.BadRequest<object>(Errors));
     }
     var response = await _authService.ResetPasswordAsync(request);
     return StatusCode((int)response.StatusCode,response);
 }
```

- #### تعالي بقا نشوف واحدة واحدة احنا محتاجين اي ؟

- #### 1- اولا احنا بنعمل Test علي  `AccountControoler` فكدا دا الاساس واي method داخليه هنعمل ليها mock ومنها `ResetPasswordAsync` و `resetPasswordValidator_`

- #### 2- عندنا `ResetPasswordRequest request` ودا الداتا بتاعة الRequest فهو  parameter بنجرب عليه وبنعمل Test من الداتا بتاعته فهو model او نقول عليه DTO عادي  

- #### 3- هنمر علي ال Validation فاحنا عاوزين نفترض ونعمل Test لو ال Validation تمام ومفيش مشاكل فاحنا كدا كدا بردو هنعمل mock ليه 

- #### 4- فاحنا عاوزين نقول لل `mockResetPasswordValidator_` لو جالك `resetPasswordRequest request` خليك تمام وعدي الدنيا فهنعمل اي بقا؟ 

- #### 5- عن طريق بردو Method اسمها Setup طب بتعمل اي (عارف اني كاتبها فوق بس افتكر معايا ياعزيزي)؟
  الـ `Setup()` بتستخدم **علشان تحدد سلوك (behavior)** الـ mock object لما يتم استدعاء method معينة طب يعني اي بردو ؟ يعني اكنك انت لما بتقول للـ mock: "لما حد ينادي الميثود دي رد بكذا"

- #### 6- طب في حالتنا احنا مفروض يرد بايه؟ مفروض بقا يرد ب `ValidationResult` من ال mock بدل ما استخدم ال Validator الحقيقي ودا اصلا دا نفس نوع ال result اللي بترجع منه في حالة انه Valid طب لو هو مش Valid (هنشوفها في المثال الجاي بس ركز هنا الاول ياعزيزي ومتزهقش مني كدا)

- #### 7- بعد كدا هنقابل بقا ال Method بتاعتنا اللي عاوزين نوصلها جوا ال `AccountController` وهي ال `ResetPasswordAsync`

- #### 8- هل هيبقي فيها جديد؟ (ولا اي حاجة ) هنلاقي بردو ان هي نفسها Dependency فهنعمل ليها بردو Setup وهنقولها لو جالك اي حاجة من `resetPasswordRequest request` رجعي `resetPasswordResponse` عادي وبس كدا 

- #### 9- كدا جينا لحبيبنا اللي عاوزين نعمل Test ليه من الاول وهو ال `AccountController` وبكدا نقدر ننادي علي `ResetPasswordAsync` براحتنا من غير مشاكل ولا وجع دماغ ونستقبل منه ال Result  

- #### 10 - وبكدا نقدر نقول لل Assert او ال FluentValidation لو هو رجع Ok ولا لأ او اي طريقة تانية تعجبك


```csharp
private readonly AccountController accountController;
private readonly Mock<IAuthService> _mockAuthForResetPassword;
private readonly Mock<IValidator<ResetPasswordRequest>> _mockResetPasswordValidator;
  public MockServices()
  {
      _mockAuthForResetPassword = new Mock<IAuthService>();
      _mockResetPasswordValidator = new();
      accountController = new AccountController(_mockAuthForResetPassword.Object,_mockResetPasswordValidator.Object);

  }
 

[Fact]
public async Task ResetPasswordAsync_ReturnsOk_OnSuccess()
{
    var resetPasswordRequest = new Application.DTO.Auth.ResetPassword.ResetPasswordRequest()
    {
        ConfirmPassword = "confirm",
        OTP = "123456",
        Password = "password",
        UserId = "user-123"
    };
    var resetPasswordResponse = new ResetPasswordResponse()
    {
        Email = "email@gmail.com",
        PhoneNumber = "9999999999",
        Role = "role",
        UserId = "user-123"
    };
    var validation = new ValidationResult();
    _mockResetPasswordValidator.Setup(b => b.ValidateAsync(resetPasswordRequest, default)).ReturnsAsync(validation);
    
    _mockAuthForResetPassword.Setup(b => b.ResetPasswordAsync(resetPasswordRequest)).Returns(Task.FromResult(_responseHandler.Success(resetPasswordResponse,"Password is reset successfully")));
    
    var result = await accountController.ResetPasswordAsync(resetPasswordRequest);
    
    var actionResult = Assert.IsType<ObjectResult>(result.Result);
    Assert.Equal(200, actionResult.StatusCode);
}

```

## تعالي بقا نشوف لو احنا عاوزين ال Case بتاعتنا ترجع `BadRequest` (ركز فيها كدا وقولي استنتجت اي)

```csharp
  public async Task ResetPasswordAsync_ReturnsBadRequest_OnFaliue()
  {
      var resetPasswordRequest = new Application.DTO.Auth.ResetPassword.ResetPasswordRequest()
      {
          ConfirmPassword = "confirm",
          OTP = "123456",
          Password = "password",
          UserId = "user-123"
      };
      
      var validation = new ValidationResult(new[]
      {
          new ValidationFailure(),
      });
      _mockResetPasswordValidator.Setup(b => b.ValidateAsync(resetPasswordRequest, default)).ReturnsAsync(validation);

      var result = await accountController.ResetPasswordAsync(resetPasswordRequest);
      var actionResult = Assert.IsType<ObjectResult>(result.Result);
      Assert.Equal(400, actionResult.StatusCode);
```

- #### 1- اول حاجة نوع ال Result اللي راجعة من ال Validation بتبقي List or Array من `ValidationFailure` طب ليه؟ لان اصلا انا بقوله لو Validation مش تمام يعني ال `ResetPasswordRequest` فيها داتا مش مسموحة اصلا فعشان كدا يرجع ب Failure 

- #### 2- انا بردو معملتش `resetPasswordResponse` ولا عملت Setup ل `mockAuthForResetPassword_` طب لييه؟ نعممممم ياعزيزي انت بتسأل ليه بجد ؟(طب ركز تاني) 
- 
- #### 3- جدع ياعزيزي عشان هي هترجع `BadRequest` من الValidation اصلا فهو مش هيخش علي ال `ResetPasswordAsync` ولا هيقربلها حتي فعشان كدا ملهاش لازمة اني استخدمها هنا (شايفك ياللي مجبتهاش من اول مرة بس ولا يهمك انا فخور بيك بردو ^ _ ^ )

---
#### تعالي نديها واحدة اكشن كدا ونفترض ان انت ياعزيزي مش بتعمل Response بنفسك زي اول مثال `ResetPasswordAsync_ReturnsOk_OnSuccess` وبتستخدم autoMapper علي طول ياتري اي الوضع ؟ تعالي نشوف لو احنا هنستخدمه فعلا ولو عاوزين mock وخلاص ونرجع داتا عادي ؟

### 1- لو خليناه mock بيرجع داتا احنا عاوزينها هيبقي اي الوضع؟
- عادي ياصديقي هنشتغل زي ماحنا هنعمل ليه mock ونحقنه جوا service عادي ونعمل Setup
- بص كدا علي المثال دا

```csharp
using AutoMapper;
using System.Collections.Generic;
using System.Threading.Tasks;

public class StudentService
{
    private readonly IStudentRepository _repository;
    private readonly IMapper _mapper;

    public StudentService(IStudentRepository repository, IMapper mapper)
    {
        _repository = repository;
        _mapper = mapper;
    }

    public async Task<List<StudentDto>> GetAllStudentsAsync()
    {
        var students = await _repository.GetAllStudentsAsync();
        return _mapper.Map<List<StudentDto>>(students);
    }
}

```

```csharp
using Xunit;
using Moq;
using AutoMapper;
using System.Collections.Generic;
using System.Threading.Tasks;

public class StudentServiceTests
{
    private readonly Mock<IStudentRepository> _mockRepo;
    private readonly Mock<IMapper> _mockMapper;
    private readonly StudentService _service;

    public StudentServiceTests()
    {
        _mockRepo = new Mock<IStudentRepository>();
        _mockMapper = new Mock<IMapper>();
        _service = new StudentService(_mockRepo.Object, _mockMapper.Object);
    }

    [Fact]
    public async Task GetAllStudentsAsync_ShouldReturnMappedDtos_UsingMockMapper()
    {
        // Arrange
        var students = new List<Student>
        {
            new Student { Id = 1, Name = "Ahmed", Age = 22 },
            new Student { Id = 2, Name = "Sara", Age = 20 }
        };

        var mappedStudents = new List<StudentDto>
        {
            new StudentDto { Id = 1, Name = "Ahmed" },
            new StudentDto { Id = 2, Name = "Sara" }
        };
        
    _mockRepo.Setup(r => r.GetAllStudentsAsync()).ReturnsAsync(students);

        
        _mockMapper.Setup(m => m.Map<List<StudentDto>>(students))
                   .Returns(mappedStudents);

        // Act
        var result = await _service.GetAllStudentsAsync();

        // Assert
        Assert.Equal(2, result.Count);
        Assert.Equal("Ahmed", result[0].Name);
        Assert.Equal("Sara", result[1].Name);
        
    }
}

```

##### شوفت هنا الدنيا بسيطة ازاي وزي ماحنا متفقين ومفيشس مشاكل عشان بنعمل تيست لميثود بتستخدم AutoMapper


### 2- تعالي بقا نشوف لو احنا اصلا عاوزين نستخدمه اصلا ونعمل test عليه هو مش مجرد mock والسلام ..تعالي نفهم اكتر
### في التطبيق الحقيقي (Production Code)

لو أنت شغال داخل ASP.NET Core مثلًا،  
عادة بتسجّل AutoMapper في الـ **Dependency Injection (DI)** كده:

```csharp
services.AddAutoMapper(typeof(Startup));
```

وساعتها النظام هو اللي بيعمل كل حاجة:

- بيقرأ كل الـ Profiles اللي في المشروع
    
- بيكون الـ MapperConfiguration
    
- بيعمل instance من `IMapper`
    
- ويحقنه في الكلاسات اللي محتاجاه (زي الـ Controllers أو الـ Services).
    

فأنت فعلاً **مش بتحتاج تكتب الكود اللي في الصورة دي جوه التطبيق**.

---

###  ثانيًا: في  (Unit Tests)

في التست بقى الوضع مختلف 

أنت مش مشغل الـ DI container ولا الـ Startup  
يعني مفيش system يكونلك AutoMapper تلقائي 

فلو عندك مثلاً `StudentService` بيستخدم AutoMapper:

```csharp
public class StudentService
{
    private readonly IMapper _mapper;

    public StudentService(IMapper mapper)
    {
        _mapper = mapper;
    }

    public StudentDto GetStudentDto(Student s)
    {
        return _mapper.Map<StudentDto>(s);
    }
}

```

ولما تيجي تختبر الكلاس ده:

```csharp
var service = new StudentService(mapper);
```

مش هتقدر تمرر `mapper` لأنه مش معمول أصلا
فانت محتاج تعمل Mapper يدويا كده:

```csharp
var config = new MapperConfiguration(c =>
 c.AddProfile(new StudentProfile()));

var mapper = new Mapper(config);

var service = new StudentService(_mapper);

```

>يعني الكود اللي في الصورة الهدف منه:  
**إنك تجهز Mapper حقيقي بنفسك داخل التست بدل ما تعتمد على النظام.**

- لو مش فاهم اوي مش مشكلة اصل المثال ده أقرب لـ Integration Test لأننا بنستخدم AutoMapper الحقيقي انا حبيت بس اوضح الفرق شوية 


```csharp
public class StudentService
{
    private readonly IMapper _mapper;

    public StudentService(IMapper mapper)
    {
        _mapper = mapper;
    }

    public List<StudentDto> MapStudents(List<Student> students)
    {
        return _mapper.Map<List<StudentDto>>(students);
    }
}

```

```csharp
public class StudentServiceTests
{
    private readonly IMapper _mapper;
    private readonly StudentService _service;

    public StudentServiceTests()
    {
        
        var config = new MapperConfiguration(cfg =>
        {
            cfg.AddProfile(new StudentProfile());
        });

        
        _mapper = new Mapper(config);

      
        _service = new StudentService(_mapper);
    }

    [Fact]
    public void MapStudents_ShouldReturnDtos()
    {
        // Arrange
        var students = new List<Student>
        {
            new Student { Id = 1, Name = "Ahmed", Age = 22 },
            new Student { Id = 2, Name = "Sara", Age = 20 }
        };

        // Act
        var result = _service.MapStudents(students);

        // Assert
        Assert.Equal(2, result.Count);
        Assert.Equal("Ahmed", result[0].Name);
        Assert.Equal(typeof(StudentDto), result[0].GetType());
    }

   
}

```
---

| الحالة                         | بتستخدم الكود ده؟ | ليه                                   |
| ------------------------------ | ----------------- | ------------------------------------- |
| داخل التطبيق (مع DI)           | ❌ لا              | لأن AutoMapper بيتكون تلقائيًا        |
| داخل Unit Test أو Console Test | ✅ أيوه            | علشان تجهز Mapper يدوي وتختبر المابات |
- ال AutoMapper الحقيقي بيتسجل داخل DI Container في ASP.NET Core تلقائيًا لكن في Unit Test بنبنيه يدوي لأن الـ container مش شغال جوه xUnit.

### شوية اكشن كدا علي جنب يمكن تحتاجها في يوم .. بكدا نقدر نفهم اننا لو عندنا حاجة بتيجي جوا ال DI واحنا عاوزين نستخدمها بنفسنا فهنكتشف اننا محتاجين نبنيها  

---
## القي نظرة كدا علي دول كدا
## MockService
```csharp
 public class MockServices
 {
     private readonly AccountController accountController;
     private readonly Mock<ILogger<AccountController>> _mockLogger;
     private readonly Mock<IAuthService> _mockAuth;
     private readonly Mock<IValidator<StudentRegisterRequestDto>> _mockStudentRegister;
     private readonly Mock<IValidator<LoginRequestDto>> _mockLogin;
     private readonly ResponseHandler _responseHandler;
     private readonly Mock<UserManager<User>> _mockUserMAnager;

     public MockServices()
     {
         _mockLogger = new();
         _mockAuth = new();
         _mockLogin = new();
         _responseHandler = new();
         _mockStudentRegister = new();
         //case I Would Use USerMAnager Must assign it to user Store;
         var setUp = new Mock<IUserStore<User>>();
         _mockUserMAnager = new Mock<UserManager<User>>(setUp.Object, null, null, null, null, null, null, null, null);
         
         
         accountController = new AccountController(_mockLogger.Object, _mockAuth.Object, _mockStudentRegister.Object, _responseHandler, _mockLogin.Object);
     }

     [Fact]
     public async Task Test_Account_Controller()
     {
         var loginRequest = new LoginRequestDto
         (
              "ahed@gmail.com",
              "password",
              "123456"
         );
         var LoginResponse = new Response<LoginResponseDto>()
         {
             StatusCode = System.Net.HttpStatusCode.OK,
             Message = "LogedIn Successfully",
             Data = new LoginResponseDto()
             {
                 AccessToken = "token",
                 RefreshToken = "refresh"
             }
         };
         var validationResult = new ValidationResult();

         _mockLogin.Setup(b => b.ValidateAsync(loginRequest, default)).ReturnsAsync(new ValidationResult());
         _mockAuth.Setup(a => a.LoginAsync(loginRequest)).ReturnsAsync(LoginResponse);

         // Assert
         var result = await accountController.LoginAsync(loginRequest);
         //var actionResult = Assert.IsType<ObjectResult>(result.Result);
         //Assert.Equal(200, actionResult.StatusCode);
         var actionResult = Assert.IsType<ObjectResult>(result.Result);
         actionResult.StatusCode.Should().Be((int)HttpStatusCode.OK);


     }
     [Fact]
     public async Task Test_Failure_Account_Controller()
     {
         var loginRequest = new LoginRequestDto("invalid", "", "");
         var validationResult = new ValidationResult(new[]
         {
             new ValidationFailure(),
         });

         var ValidateResult = _mockLogin.Setup(b => b.ValidateAsync(loginRequest, default)).ReturnsAsync(validationResult);


         var result = await accountController.LoginAsync(loginRequest);
         var actionResult = Assert.IsType<ObjectResult>(result.Result);
         actionResult.StatusCode.Should().Be((int)HttpStatusCode.BadRequest);
     }

```

---
## معلومة زيادة علي الطاير 

بص كدا يا عزيزي علي ال `End-Point` دي :

```csharp
 [HttpPost("change-password")]
 public async Task<ActionResult<Response<string>>>ChangePasswordAsync(ChangePassword request)
 {
     var validation = await _changePasswordValidator.ValidateAsync(request);
     if (!validation.IsValid)
     {
         var Errors = string.Join(',',validation.Errors.Select(e => e.ErrorMessage).ToList());
         return StatusCode((int)HttpStatusCode.BadRequest, _responseHandler.BadRequest<object>(Errors));
     }
   
      //Here buddy
      var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
    
      var response = await _authService.ChangePassword(userId,request);
     return StatusCode((int)response.StatusCode,response);
 }
```

- هنا لما انا اجي اعمل تيست محتاج يبقي عندي قيمة لل User ومحتاج اخلي ال `FindFirst` ترجع قيمة افتراضية زي ما بنعمل فاحنا هنعمل كدا 
``` csharp
 public class MockServices
 {
     private readonly AccountController accountController;
     private readonly Mock<ILogger<AccountController>> _mockLogger;
     private readonly Mock<IValidator<ChangePassword>> _mockChangePasswordValidator;
     private readonly ResponseHandler _responseHandler;
     private readonly Mock<UserManager<User>> _mockUserMAnager;
     public MockServices()
     {
         
         _mockChangePasswordValidator = new();
         _mockForgetPasswordValidator = new();
         //case I Would Use USerMAnager Must assign it to user Store;
         var setUp = new Mock<IUserStore<User>>();
         _mockUserMAnager = new Mock<UserManager<User>>(setUp.Object, null, null, null, null, null, null, null, null);


         accountController = new AccountController(_mockLogger.Object, _mockAuth.Object,
             _mockStudentRegister.Object, _responseHandler,
             _mockLogin.Object, _mockChangePasswordValidator.Object,
             _mockResetPasswordValidator.Object, _mockForgetPasswordValidator.Object);


         //for User claims
         var user = new ClaimsPrincipal(new ClaimsIdentity(new[]
          {
             new Claim(ClaimTypes.NameIdentifier, "user-123")
          }, "mock"));

         accountController.ControllerContext = new ControllerContext()
         {
             HttpContext = new DefaultHttpContext { User = user }
         };
     }

```

- ركزلي بقا في الحتة البسيطة دي 
``` csharp
            //for User claims
         var user = new ClaimsPrincipal(new ClaimsIdentity(new[]
          {
             new Claim(ClaimTypes.NameIdentifier, "user-123")
          }, "mock"));

         accountController.ControllerContext = new ControllerContext()
         {
             HttpContext = new DefaultHttpContext { User = user }
         };
```

- #### تقدر تخمن هنا انا عملت اي ؟
- انا هنا عملت `(User) ClaimPrinciple` يدوي من عندي انا عشان لما يجي يشوف ال Test بتاعي يلاقي ليه Value , هتقولي طب مانا ممكن اعمل كدا عادي تحت انا استفدت اي وفين اصلا ال method اللي مفروض اهندلها بال setup بتاعها؟ هنا بقا يجي دور ال `ClaimTypes` اني حطيت قيمة ال `Id` جواها وبعد كدا خليت ال `User` بتاعي جوا ال `HTTPContext`  عادي بحيث يبقي هو ال Default بتاعي.. وبس كدا يعزيزي انا معايا قيمة ال User وال Value اللي انا عاوز ارجعها 


- ### تعالي بقا نراجع مع بعض ونرتب افكارنا سوا 
1. **أنشأت ClaimsPrincipal (الـUser يدوي)**
    ```csharp
    var user = new ClaimsPrincipal(new ClaimsIdentity(new[]
     {
        new Claim(ClaimTypes.NameIdentifier, "user-123")
     }, "mock"));
    ```
    
     يعني إنت بتقول للمشروع وقت الـTest:
    
    > "اعتبر إن عندي مستخدم داخل السيستم اسمه user-123"  
    > وده بيحاكي بالظبط اللي بيحصل لما حد يسجل دخول فعلي (الـJWT أو الـCookie بيبعت Claims جوه الـUser).
    

---

2. **ربطت الـUser بالـHttpContext**
    
    ```csharp
    accountController.ControllerContext = new ControllerContext() 
    {
         HttpContext = new DefaultHttpContext { User = user }
    };
    ```
    
     يعني قلت للـController:
    
    > "خد الـHttpContext ده كأنه الريكوست اللي جاي من يوزر حقيقي، وجواه اليوزر ده"
    
    فبالتالي لما الكود جوه الكنترولر يعمل:
    
    ```csharp
    var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
    ```
    
    الـUser مش فاضي، وهيلاقي claim اسمها `NameIdentifier` فيها `"user-123"` 
    

---

###  ليه لازم أعمل كده في الـUnit Test؟

لأن في الـTest **مفيش Request حقيقي ولا Authentication Middleware**،  
فـ `User` بيبقي `null` دايمًا.  
لو ما عملتش الحتة دي، الكود جوه الكنترولر هيكسر أو هيرجع `null` بدل الـuserId.

فإنت كده **وفرت جو مصطنع شبه الحقيقي** يخلي الكنترولر يشتغل كأنه في runtime فعلاً.

---
- ### تعالي بقا نبص سوا علي Method ال Test هتمشي ازاي

```csharp
 public async Task test_success_change_password()
 {
     var changePasswordRequest = new ChangePassword()
     {
         NewPassword = "newpassword",
         ConfirmNewPassword = "newpassword",
         CurrentPassword = "oldpassword"
     };

     var validation = new ValidationResult();
     _mockChangePasswordValidator.Setup(b => b.ValidateAsync(changePasswordRequest, default)).ReturnsAsync(validation);

     _mockAuth.Setup(b => b.ChangePassword("user-123", changePasswordRequest)).Returns(Task.FromResult(_responseHandler.Success<string>(null, "password changes successfully")));
     var result = await accountController.ChangePasswordAsync(changePasswordRequest);
     var actionResult = Assert.IsType<ObjectResult>(result.Result);
     actionResult.StatusCode.Should().Be((int)HttpStatusCode.OK);
 }
```

### هي هنا مشيت زي اي مثود قديمة عادي ولكن احنا هندلنا جزء ال User فووق في ال Constructor بتاعنا وبس كدا دي كانت حاجة اضافية كدا حبيت اخليك تاخد بالك منها ياصديقي 

بص علي الريبو دا [Gharbawy1/AuthTemplate](https://github.com/Gharbawy1/AuthTemplate) وخدلك بصة في جزء الTest وعيش بقا 
 
---
## InMemoryDataBase

- احيانا بيبقي لازم نخترف كود الDataBase اللي هو `(ApplicationDbContext)` فاحنا مينفعش نباصيه علي طول عشان مش نبوظ الداتا بيز بتاعتنا وفي نفس الوقت ال mock مش حل فعال لانك عاوز تستخدم الداتا بيز بجد طب اي الحل ؟

- حل من الحلول اننا نخلي الداتا بيز بتاعتنا في الميموري ودا اللي اسمه `InMemoryDataBase` وبنتعامل عادي اكننا بسنتخدم ال `ApplicationDbContext` طب بنطبقه ازاي؟ يلا بينا 

```csharp
 internal class InMemoryDatabase:Infrastructure.Persistence.ApplicationDbContext
 {
     public InMemoryDatabase(DbContextOptions<ApplicationDbContext> options) : base(options)
     {
     }
     protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
     {
         //base.OnConfiguring(optionsBuilder);
         optionsBuilder.UseInMemoryDatabase(Guid.NewGuid().ToString());
     }

     public override void Dispose()
     {
         Database.EnsureDeleted();
         base.Dispose();
     }
 }
```

- احنا ورثنا من `ApplicationDbContext` عشان نتعامل عادي اكننا بنتعامل معاه وكمان نخلي الداتا بيز بتاعتنا مكانها في الميموري عن طريق ` protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)` وحددنا في ال 
`optionsBuilder.UseInMemoryDatabase(Guid.NewGuid().ToString());`
وخلناه `Guid` عشان نتجنب عشان ان الداتا بيز تبقي shared بين اكتر من method عشان محدش يبوظ فيها وبكدا منعنا التداخل بين الاختبارات و كل test بقا مستقل 

- - ال `Dispose()` بيضمن حذف الداتابيز الوهمية بعد انتهاء الاختبار → حافظ على نظافة الذاكرة. 

- تعالي نشوف ازاي بنستخدمها 

```csharp
 public class InMemoryTesting
 {
     private readonly AuthService _authService;
     private readonly Mock<ITokenService> _tokenService;
     private readonly Mock<ILogger<AuthService>> _logger;
     private readonly Mock<IEmailService> _emailService;
     private readonly Mock<IImageUploading> _imageUploading;
     private readonly InMemoryDatabase _context;
     private readonly Mock<UserManager<User>> _userManager;
     private readonly Mock<IOTPService> _otpService;
     private readonly ResponseHandler _responseHandler;

     public InMemoryTesting()
     {
     
     // هنا طريقة الاستخدام 
         var options = new DbContextOptionsBuilder<ApplicationDbContext>().Options;
         _context = new InMemoryDatabase(options);
         
         //--------------------------------------------
         
         
         
         _logger = new();
         _emailService = new();
         _imageUploading = new();
         _otpService = new();
         _tokenService = new();
         _responseHandler = new();
         var setUp = new Mock<IUserStore<User>>();
         _userManager = new Mock<UserManager<User>>(setUp.Object, null, null, null, null, null, null, null, null);

         _authService = new(_tokenService.Object, _logger.Object, _context, _userManager.Object, _imageUploading.Object, _responseHandler, _otpService.Object, _emailService.Object);

     }


---
## هههححححححح خدلك بريك كدا وهاتلك شكولاتاية جميلة وتعالي تاني عشان اللي جاي محتاجك فايق فيه اوي عشان هنستمتع مع بعض

---
## Theory & Inline
### الأول تعالي نفتكر ياصديقي يعني إيه `[Fact]`؟

لما بتكتب **Test method** عادية في xUnit، بتستخدم `[Fact]`:

```csharp
[Fact]
public void Add_ReturnsCorrectSum()
{
    var result = Add(2, 3);
    Assert.Equal(5, result);
}

```

ده بيختبر **حالة واحدة بس** اللي هي (2 + 3 = 5).  
لكن لو انت عايز تختبر **نفس الميثود بكذا قيمة مختلفة** هل   
هنعمل كذا test؟ لا طبعا ***(دا احنا مبرمجين وعارفين ان لو الكود اتكرر كذا مرة يبقي في مشكلة)***  
###  هنا بقى بييجي دور `[Theory]`

`[Theory]` يعني:

>اكني بقول ان “الميثود دي فيها **logic ثابت**، وعايز أجربه بكذا **Data مختلفة**

يعني بدل ما تكرّر نفس الكود، تديه كذا **مدخل مختلف** وتجرب.

- ### تعالي بص معايا علي دا :

```csharp
[Theory]
[InlineData(2, 3, 5)]
[InlineData(5, 5, 10)]
[InlineData(10, -2, 8)]
public void Add_ReturnsCorrectSum(int a, int b, int expected)
{
    var result = Add(a, b);
    Assert.Equal(expected, result);
}

```


- لما تشغّل الـ tests، xUnit هيشغّل **الميثود 3 مرات**  — مرة لكل `[InlineData]` ويشوف هل النتيجة بتاعت كل واحدة صح ولا لأ
- ![[Pasted image 20260126210658.png]]

---
### كدا احنا فهمنا يعني اي `InlineData` تعالي بقا ياصديقي نفهم حاجة غيرها ال `Theory` بتقدمها وهي ال `MemberData` ,  `ClassData` 

- #### تعالي نفهم سوا الفرق بينهم :
- `[InlineData]` → بتدي بيانات **ثابتة وبسيطة** لكل مرة بتشغل فيها الاختبار.
    
- `[MemberData]` → بتدي بيانات من **property أو method أو field**، ممكن تكون static، ممكن generate data dynamically.
    
- `[ClassData]` → بتدي بيانات من **class كامل** بيطبق `IEnumerable<object[]>`، وده كويس لو عايز data معقدة أو reusable.

---
>يعني **لو البيانات بسيطة جداً** → `[InlineData]` تمام.
>
>- بس لو البيانات كبيرة أو معقدة → هتبقى `[InlineData]` حاجة مش عمليةخالص ...ليه ؟ :
    
    - هتبقى طويلة جداً
        
    - هتتكرر
        
    - صعب تتعامل مع dynamic أو data جايه من method / json / csv
        


>-  **لو البيانات محتاجة توليد dynamically** → استخدم `[MemberData]` أو `[ClassData]`.

---
- ### كدا عرفنا ان ال `InlineData` للداتا البسيطة مش لل Classes الكبيرة وحوارات وشغل لف ودوران وكدا .. تعالي بقا نشوف اللف والدوران سوا ومتقلقش هتستمع صدقني جاهز؟ توكلنا علي الله 
----

## 1- `ClassData`
## الفكرة ببساطة 

- **الـ class ده لازم يطبق `<IEnumerable<object[]`**، بحيث كل `object[]` تمثل **مجموعة بيانات واحدة** لاختبار `[Theory]`.
    
- بيساعدك لما البيانات تكون :
    
    - **كبيرة**
        
    - **معقدة** (objects، lists، dictionaries)
        
    - **تحتاج Dynamic Generation** (مثلاً loops، random)
        
    - **تستخدمها في أكثر من test class**
        

---

## **طب اي هي متطلبات ClassData**

1. لازم يكون **class عام (public)**.
    
2. يطبق `<IEnumerable<object[].
    
3. كل `object[]` **داخل الـ enumerator بيكون set واحدة داخل ال Test** اللي  هيدخل في الاختبار.
    
4. مينفعش يكون static لأن xUnit هيعمل instance من ال Class  

- تعالي نشوف مثال بسيط كدا 

> دا ال Class بتاعنا اللي بنقول عليه 
```csharp
using System.Collections;
using Xunit;

public class SumClassData : IEnumerable<object[]>
{
    public IEnumerator<object[]> GetEnumerator()
    {
        // هنا بنعمل كل case بطريقة dynamically 
        yield return new object[] { new int[] {1, 2, 3}, 6 };
        yield return new object[] { new int[] {4, 5, 6}, 15 };
        yield return new object[] { new int[] {7, 8, 9}, 24 };
    }

    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

```
- - كل `yield return object[]` → **test case واحد**
    
- كل array جوهها: **inputs + expected result** 


> تعالي نشوف التيست هيبقي ازاي
```csharp
public class SumTests
{
    [Theory]
    [ClassData(typeof(SumClassData))]
    public void Test_Sum(int[] numbers, int expectedSum)
    {
        int actualSum = 0;
        foreach (var n in numbers)
            actualSum += n;

        Assert.Equal(expectedSum, actualSum);
    }
}

```

- ### هنا ال Test دا هيشتغل لكل object جوا ال `Enumerator` 
- ### احنا هنا جبنا لكل object + variable وحطيناها في class وجربنا بقا التيست بتاعنا 
---
## تعالي نشوف مثال كمان علي Class كداتا معقدة شوية ( مشيها معقدة وخلاص (*^_^*)  من غير تدقيق  )

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

public class PersonClassData : IEnumerable<object[]>
{
    public IEnumerator<object[]> GetEnumerator()
    {
        var names = new string[] { "Ahmed", "Mohmaed", "Ali" };
        var rand = new Random();

        foreach(var name in names)
        {
            yield return new object[]
            {
                new Person { Name = name, Age = rand.Next(20, 50) },
                true // دي خليها ك expected result
            };
        }
    }

    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

```

-  تعالي بقا نشوف هنعمل تيست ازاي 
```csharp
  [ClassData(typeof(PersonClassData))]
  public void Test_Person_For_Class_Data(Person person,bool expected)
  {
      bool actual = person.Age > 18;
      Assert.Equal(expected,actual);
  }

```

كدا احنا فهمنا ازاي ال `ClassData`:
    **مناسب للبيانات المعقدة** زي objects, lists, dictionaries, tuples، 
        

بيكون **Reusable**:  ممكن تستخدم نفس ال Class في اكتر من `[Theory]`
        
 **تنظيف الكود** :  بدل ما تحط بيانات كبيرة ومعقدة في InlineData أو MemberData، تعمل class مستقل ينظمهم

---
## 2- `MemberData`
## **الفكرة الأساسية**

- `[MemberData]` بتستخدم عشان تغذي الـ `[Theory]` ببيانات من  **(Member)** داخل الكود.
    
- العضو ده ممكن يكون:
    
    - **Property**
        
    - **Field**
        
    - **Method**
        
- وكل عضو بيرجع نوع من **`IEnumerable<object[]>`** (يعني مجموعة من test cases).
    
- كل `object[]` = مجموعة بيانات واحدة لاختبار واحد.


## **المتطلبات الأساسية**

1. الـ member (property/method/field) لازم يرجع `<IEnumerable<object[]
    
2. الـ member لازم يكون **static** في أغلب الحالات (علشان xUnit يقدر يستخدمه من غير ما ينشئ instance من الـ class).
    
3. لو member مش static → لازم تحدد `MemberType = typeof(SomeClass)` عشان يعرف يجيبه.
    

---

##  **فكرتها ببساطة**

> `[MemberData]` =
>  “هاتلي البيانات من عضو معين في الكلاس بدل ما أكتبها بإيدي في `[InlineData]`”.



---
## تعالي نشوف كذا مثال 

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

```

```csharp
using System.Collections.Generic;
using Xunit;

public class PersonMemberData: IEnumerable<Object[]>
{
    // هنا البيانات جاية من Property Static
    public static IEnumerable<object[]> PersonData =>
        new List<object[]>
        {
            new object[] { new Person { Name = "Ahmed", Age = 25 }, true },
            new object[] { new Person { Name = "Ali", Age = 17 }, false },
            new object[] { new Person { Name = "Sara", Age = 30 }, true }
        };
        
    public IEnumerator<object[]> GetEnumerator()
    {
      return (IEnumerator<Object[]>)PersonData;
    }
    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

```

```csharp
 [Theory]
 [MemberData(nameof(PersonMeberData.PersonData), MemberType = typeof(PersonMeberData))]
 public void Test_PersonAgeIsAdult_WithMethod(Person person, bool expected)
 {
     bool actual = person.Age > 18;
     Assert.Equal(expected, actual);
 }

```

``` csharp
[MemberData(nameof(PersonMeberData.GetPersons), MemberType = typeof(PersonMeberData))]
```

- هنا انا بحدد مكان ال Method ومكانها في اي Class
---
خد كمان مثال بس خد بالك اللي فوق كانت Property ودي Method (فرقت في اي هقولك عادي ولا حاجة حبيت انكشك بس )

```csharp

public class NumberTests: IEnumerable<Object[]>
{
    public static IEnumerable<object[]> GetEvenNumbers()
    {

    yield return new object[] { 2, true };
    yield return new object[] { 5, false };
    yield return new object[] { 8, true };

    for (int i = 10; i <= 14; i++)
    {
        bool isEven = i % 2 == 0;
        yield return new object[] { i, isEven };
    }

    // هنا انا عملتلك نوعين من الداتا ثابتة و dynamic
   }
   public IEnumerator<object[]> GetEnumerator()
   {
      return (IEnumerator<Object[]>)GetEvenNumbers();
   }
  IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

```

```csharp
  [Theory]
[MemberData(nameof(PersonMeberData.GetEvenNumbers),MemberType =typeof(PersonMeberData))] 
public void Test_IsEvenNumber(int number, bool expected)
{
    bool actual = number % 2 == 0;

    Assert.Equal(expected, actual);
}


```

---
## حاسك غرقت مني ومفهمتش الفرق بينهم اوي؟ مش كدا ؟ ورايا علي المثال الجاي
```csharp
public class UserTests
{
    
    public static IEnumerable<object[]> UserTestData =>
        new List<object[]>
        {
            new object[] { "Ahmed", 25, true },
            new object[] { "Nada", 17, false },
            new object[] { "", 30, false }
        };

    [Theory]
    [MemberData(nameof(UserTestData))]
    public void ValidateUser_ShouldReturnExpectedResult(string name, int age, bool expected)
    {
        bool result = IsValidUser(name, age);
        Assert.Equal(expected, result);
    }

    private bool IsValidUser(string name, int age)
        => !string.IsNullOrWhiteSpace(name) && age >= 18;
}

```

#### هنا الداتا بتاعتي بيانات بسيطة وثابتة ومش محتاجة logic عليها فانا استخدمت بقا ال `MemberData`
---

تخيل بقا إن عندنا **ملف JSON** فيه بيانات المستخدمين اللي هتختبرهم


```json
[
  { "Name": "Ahmed", "Age": 25, "Expected": true },
  { "Name": "Nada", "Age": 17, "Expected": false },
  { "Name": "", "Age": 30, "Expected": false },
  { "Name": "Ali", "Age": 40, "Expected": true }
]
```

#### في حالتنا بقا الداتا دي كبيرة وكمان احنا عاوزين نقرأهم من ملف ال Json نفسه فهنا يجي دور ال `ClassData`

```csharp
public class UserDataFromJson : IEnumerable<object[]>
{
    public IEnumerator<object[]> GetEnumerator()
    {
        var json = File.ReadAllText("users.json");
        var users = JsonSerializer.Deserialize<List<UserTestModel>>(json);

        foreach (var user in users)
        {
            yield return new object[] { user.Name, user.Age, user.Expected };
        }
    }

    IEnumerator IEnumerable.GetEnumerator() => GetEnumerator();
}

public class UserTestModel
{
    public string Name { get; set; }
    public int Age { get; set; }
    public bool Expected { get; set; }
}
```
##### شوف بقا اننا عملنا هنا load من الملف وبعد كدا حطينا ال Values جوا list من ال UserTestModel... هنا الTest اصبح واضح جدا وسهل نطبقه (يلا نبص عليه)
```csharp
public class UserTests
{
    [Theory]
    [ClassData(typeof(UserDataFromJson))]
    public void ValidateUser_ShouldReturnExpectedResult(string name, int age, bool expected)
    {
        bool result = IsValidUser(name, age);
        Assert.Equal(expected, result);
    }

    private bool IsValidUser(string name, int age)
        => !string.IsNullOrWhiteSpace(name) && age >= 18;
}
```
----
###### هتقولي ليه معملتش نفس الكلام دا ب `MemberData`؟ (سؤال رخم بس انت تؤمر ياصديقي )
- ال `ClassData` معمول مخصوص للحالات اللي فيها **تعقيد أو تكرار أو مصادر خارجية** زي المثال اللي فوق 
- ال  `[MemberData]`  كويس للبيانات **الثابتة والصغيرة** لازم static مش مناسب للـ IO أو الحسابات الكبيرة 

---
## **الفرق بين MemberData و ClassData باختصار** (دي من شات جبت الصراحة حاولت احط اي اختصار كدا O_O)

| المقارنة                      | `[MemberData]`            | `[ClassData]`    |
| ----------------------------- | ------------------------- | ---------------- |
| مصدر البيانات                 | property / field / method | class كامل       |
| لازم `IEnumerable<object[]>`؟ | ✅                         | ✅                |
| Static مطلوب؟                 | غالبًا نعم                | لا               |
| مناسب للبيانات البسيطة؟       | ✅                         | شوية معقد        |
| مناسب للديناميكية أو الكبيرة؟ | ⚠️ محدود                  | ✅ قوي            |
| سهل وسريع للاستخدام           | ✅ جدًا                    | محتاج إعداد أكتر |
| Reusable في كذا test class    | محدود                     | ✅ ممتاز          |
<br>
<br>


---
- ## بكدا اكون حاولت اجيب كل ال basics لل Xunit اكيد في مواضيع اكتر واكتر لكن انت لازم تقرأ اكتر ياصديقي وتجرب بنفسك وهتقابل مئات ال Cases زي جزء ال Private Method + Reflection, IQuerable و غيره من الCases اللي مش موجودة هنا بس دول محتاجين قعدة تانية لينا سوا مع كوباية الشاي الجميلة.. فشطارتك بقا انك بعد ما فهمت انك تحاول تهندلها وتفهم هي بتشتغل ازاي واي افضل طريقة للسيناريو اللي انت شغال عليه وواحدة واحدة هتلاقي نفسك بتتحسن 
##

- ## عذرا ياصديقي لو كان في حاجة مش واضحة ممكن تتواصل معايا ونقدر نهندلها ولو  انت استفدت فدي عندي بالدنيا كلها وياريت تدعيلي دعوة جميلة زيك كدا ولك بالمثل بأذن الرحمن 
##


- ## واتفكر دايما ان ربنا كريم اوي وعطاياها لاتحصي.. ماعليك غير انك تسعي وتكمل سعي وتحمد ربنا علي نعمه وتتوكل عليه ولازم تتيقن انك بتتوكل علي ربنا وبس كدا ياصديقي ربنا يوفقك ويعينك يارب والي لقاء اخر ان شاء الله 


###  Connect with me :- 

 <a href="https://www.linkedin.com/in/ahmed-zaher-62a652255/" target="_blank">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/linkedin/linkedin-original.svg" width="20"/>
   Ahmed Zaher
</a>


 <a href="https://x.com/AhmedZaher882" target="_blank">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/twitter/twitter-original.svg" width="20"/>
   Ahmed Zaher
</a>


 <a href="https://www.facebook.com/ahmed.t.zaher.2025" target="_blank">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/facebook/facebook-original.svg" width="20"/>
   Ahmed T. Zaher
</a>


 <a href="https://github.com/ahmed-tarek-2004" target="_blank">
  <img src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/github/github-original.svg" width="20"/> Ahmed Zaher
</a>

