# Airbnb CSS / Sass Stil Rehberi

*CSS ve Sass için en akıllıca yaklaşım*


## İçindekiler

  1. [Terminoloji](#terminology)
    - [Kural Bildirimi](#rule-declaration)
    - [Seçiciler](#selectors)
    - [Özellikler](#properties)
  1. [CSS](#css)
    - [Düzenleme](#formatting)
    - [Yorumlar](#comments)
    - [OOCSS ve BEM](#oocss-and-bem)
    - [ID Seçiciler](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Kenar](#border)
  1. [Sass](#sass)
    - [SözDizimi](#syntax)
    - [Sıralama](#ordering-of-property-declarations)
    - [Değişkenler](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
  1. [Çeviriler](#translation)

## Terminoloji

### Kural Bildirimi

Bir "kural bildirimi", bir seçiciye (veya  bir seçici grubuna) verilen eşlik grubu ile birlikte verilen addır. İşte bir örnek:

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Seçiciler

Bir kural bildiriminde, "seçiciler", DOM ağacındaki hangi öğelerin tanımlı özelliklere göre şekillendirileceğini belirleyen parçalardır.
Seçiciler, bir öğenin sınıfı, kimliği veya herhangi bir özniteliklerinin yanı sıra HTML öğelerini eşleştirebilir. Seçicilere bazı örnekler:

```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Özellikler
Son olarak, özellikler, bir kural bildiriminin seçilen öğelerine kendi stilini veren şeydir. Özellikler anahtar-değer çiftleri ve kural bildirimi bir veya daha fazla özellik bildirimi içerebilir. Özellik bildirimleri şuna benzer:

```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

## CSS

### Düzenleme
* Girinti için  sekmeler (2 boşluk) kullanın
* Sınıf adlarında camelCasing üzerinde çizgi kullanmayı tercih edin.
   - BEM (bkz. [OOCSS ve BEM] (# oocss-and-bem)) kullanıyorsanız, Alt Çizgiler (Underscores) ve PascalCasing kullanılabilir.
*  ID seçicilerini kullanmayın
* Bir kural bildiriminde birden çok seçici kullanırken, her seçiciye kendi satırını verin.
* Açılış parantezinden önce kural bildiriminde `{` yer bırakın
* Özelliklerde, `:` karakterinden sonra değil de bir boşluk koyun.
* Yeni bir satıra kural bildirimlerinin `` `` nın köşeli parantezlerini koyun
* Kural bildirimleri arasında boş satırlar koyun

**Kötü**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**İyi**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Yorumlar

*  Block yorumlar için satır yorumlarını (`//`  Sass dünyasında) tercih edin.
* Kendi hattında yorumlar tercih edin. Satır sonu açıklamalarından kaçının.
* Kendini anlatamayan  kod için ayrıntılı açıklamalar yazın:
   - Z-index'in kullanımı
   - Uyumluluk veya tarayıcıya özel kesmek

### OOCSS ve BEM
Aşağıdaki  nedenlerden dolayı OOCSS ve BEM'in bazı kombinasyonlarını teşvik ediyoruz:

  * CSS ve HTML arasında net ve sıkı ilişkiler yaratmaya yardımcı olur
  * Yeniden kullanılabilir, oluşturulabilir bileşenler yaratmamıza yardımcı oluyor
  * Daha az iç içe geçmiş ve daha düşük özgüllük sağlar
  * Ölçeklenebilir stil yapımında yardımcı olur

**OOCSS**, or “Object Oriented CSS”, is an approach for writing CSS that encourages you to think about your stylesheets as a collection of “objects”: reusable, repeatable snippets that can be used independently throughout a website.

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM**, or “Block-Element-Modifier”, is a _naming convention_ for classes in HTML and CSS. It was originally developed by Yandex with large codebases and scalability in mind, and can serve as a solid set of guidelines for implementing OOCSS.

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

We recommend a variant of BEM with PascalCased “blocks”, which works particularly well when combined with components (e.g. React). Underscores and dashes are still used for modifiers and children.

**Example**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` is the “block” and represents the higher-level component
  * `.ListingCard__title` is an “element” and represents a descendant of `.ListingCard` that helps compose the block as a whole.
  * `.ListingCard--featured` is a “modifier” and represents a different state or variation on the `.ListingCard` block.

### ID Seçiciler

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Kenar

Bir stilin kenarlık belirtmediğini belirtmek için `none` yerine `0` kullanın.

**Bad**

```css
.foo {
  border: none;
}
```

**İyi**

```css
.foo {
  border: 0;
}
```

## Sass

### Sözdizimi

* Use the `.scss` syntax, never the original `.sass` syntax
* Order your regular CSS and `@include` declarations logically (see below)

### Ordering of property declarations

1. Property declarations

    List all standard property declarations, anything that isn't an `@include` or a nested selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. `@include` declarations

    Grouping `@include`s at the end makes it easier to read the entire selector.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Nested selectors

    Nested selectors, _if necessary_, go last, and nothing goes after them. Add whitespace between your rule declarations and nested selectors, as well as between adjacent nested selectors. Apply the same guidelines as above to your nested selectors.

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

Prefer dash-cased variable names (e.g. `$my-variable`) over camelCased or snake_cased variable names. It is acceptable to prefix variable names that are intended to be used only within the same file with an underscore (e.g. `$_my-variable`).

### Mixins

Mixins should be used to DRY up your code, add clarity, or abstract complexity--in much the same way as well-named functions. Mixins that accept no arguments can be useful for this, but note that if you are not compressing your payload (e.g. gzip), this may contribute to unnecessary code duplication in the resulting styles.

### Extend directive

`@extend`'den kaçınılmalıdır, çünkü özellikle iç içe seçicilerle kullanıldığında anlamsız ve potansiyel olarak tehlikeli bir davranışı vardır.Seçici sırası daha sonra değişirse (örneğin, başka dosyalardalar ve dosyaların yüklendiği sırayla) en üst düzey yer tutucu seçicileri bile genişleterek sorunlara neden olabilirler.Gzipping, `@extend` kullanarak kazandığınız tasarrufların çoğunu yapmalıdır ve stil sayfalarını mixin'lerle güzelce kurabilirsiniz.

### İç içe Seçiciler

**Seçicileri üç kattan daha iç içe yerleştirmeyin!**

```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

Seçiciler bu kadar uzun olduğunda, muhtemelen şu CSS'yi yazıyorsunuzdur:

* HTML'ye güçlü bir şekilde bağlandı (kırılgan) * -VEYA- *
* Aşırı derecede spesifik (güçlü) * -VEYA- *
* Tekrar kullanılamaz

Tekrardan Uyarı : **ID seçicileri iç içe yazmayın**


İlk önce bir kimlik seçici kullanmanız gerekiyorsa (ve gerçekten yapmamaya çalışmanız gerekiyor) iç içe geçmiş olmamalıdır.
Kendinize bunu buluyorsanız, HTML'inizi tekrar gözden geçirmeniz veya böyle kuvvetli özgünlüğe neden ihtiyaç duyulduğunuzu anlamanız gerekir.İyi biçimlendirilmiş HTML ve CSS yazıyorsanız hiçbir zaman yapmamanız gerekiyor.

## Çeviriler

  Bu stil rehberi diğer dillerde de mevcuttur:

  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [ArvinH/css-style-guide](https://github.com/ArvinH/css-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [nao215/css-style-guide](https://github.com/nao215/css-style-guide)
  - ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [CodeMakeBros/css-style-guide](https://github.com/CodeMakeBros/css-style-guide)
  - ![PT-BR](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese**: [felipevolpatto/css-style-guide](https://github.com/felipevolpatto/css-style-guide)  
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [Nekorsis/css-style-guide](https://github.com/Nekorsis/css-style-guide)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [ismamz/guia-de-estilo-css](https://github.com/ismamz/guia-de-estilo-css)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [trungk18/css-style-guide](https://github.com/trungk18/css-style-guide)
  - ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Bahasa Indonesia**: [mazipan/css-style-guide](https://github.com/mazipan/css-style-guide)
  - ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [nazrdogan/css](https://github.com/nazrdogan/css)

